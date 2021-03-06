<template>
  <div class="mobile-info-block d-flex align-items-center">
    <interface-balance-modal ref="balanceModal" :balance="accountBalance" />
    <div class="balance-contents">
      <div class="info-block-title text-uppercase font-reset-disabled mb-1">
        {{ $t('common.balance.wallet') }}
      </div>
      <div class="info-block-value text-monospace pl-3">
        {{ accountBalance
        }}<span class="font-reset-disabled">{{
          network.type.currencyName
        }}</span>
      </div>
      <div class="py-2"></div>

      <div v-if="false" class="pl-3">
        <div class="equivalent-values-title font-reset-disabled">
          <i class="fa fa-angle-right" aria-hidden="true"></i>
          {{ $t('interface.check-balance.equivalent') }}
        </div>
        <ul class="equivalent-values-data pl-3">
          <li
            v-for="ev in equivalentValues"
            :key="ev.key"
            class="d-flex align-items-center"
          >
            <img
              :src="
                require(`@/assets/images/currency/${ev.name.toLowerCase()}.svg`)
              "
              alt
              class="mr-2"
            />
            <div class="mr-2 text-monospace">{{ ev.name }}</div>
            <div class="text-monospace">{{ ev.value }}</div>
          </li>
        </ul>
      </div>
    </div>

    <div class="balance-contents ml-auto" @click="showBalanceModal">
      <i class="setting fa fa-ellipsis-v" aria-hidden="true"></i>
    </div>
  </div>
</template>

<script>
import { mapState, mapActions } from 'vuex';
import { Toast } from '@/helpers';
import BigNumber from 'bignumber.js';
import InterfaceBalanceModal from '@/layouts/InterfaceLayout/components/InterfaceBalanceModal';

export default {
  components: { 'interface-balance-modal': InterfaceBalanceModal },
  data() {
    return {
      equivalentValues: [
        {
          name: 'BTC',
          value: ''
        },
        {
          name: 'REP',
          value: ''
        },
        {
          name: 'CHF',
          value: ''
        },
        {
          name: 'USD',
          value: ''
        },
        {
          name: 'EUR',
          value: ''
        },
        {
          name: 'GBP',
          value: ''
        }
      ]
    };
  },
  computed: {
    ...mapState('main', ['network', 'web3', 'account']),
    accountBalance() {
      return this.web3.utils.fromWei(
        new BigNumber(this.account.balance).toFixed(),
        'ether'
      );
    }
  },
  watch: {
    balance() {
      this.fetchBalanceData();
    }
  },
  mounted() {
    this.getBalance();
    this.fetchBalanceData();

    //console.log(this.equivalentValues);
  },
  methods: {
    ...mapActions('main', ['setAccountBalance']),
    showBalanceModal() {
      this.getBalance();
      this.$refs.balanceModal.$refs.balance.show();
    },
    getBalance() {
      if (this.account.address) {
        this.web3.eth
          .getBalance(this.account.address.toLowerCase())
          .then(res => {
            this.setAccountBalance(res);
          })
          .catch(err => {
            Toast.responseHandler(err, Toast.ERROR);
          });
      }
    },
    async fetchBalanceData() {
      if (this.online) {
        const newArr = [];
        const url = 'https://cryptorates.mewapi.io/convert/ETH';
        const fetchValues = await fetch(url);
        const values = await fetchValues.json();
        delete values['lastCalled'];
        Object.keys(values).forEach(item => {
          if (
            this.equivalentValues.find(curr => {
              return curr.name === item;
            })
          ) {
            const objectRes = {
              name: item,
              value: new BigNumber(this.accountBalance)
                .multipliedBy(new BigNumber(values[item]))
                .decimalPlaces(18)
                .toFixed()
            };
            newArr.push(objectRes);
          }
        });
        this.equivalentValues = newArr;
      }
    }
  }
};
</script>

<style lang="scss" scoped>
@import 'MobileBalanceBlock.scss';
</style>
