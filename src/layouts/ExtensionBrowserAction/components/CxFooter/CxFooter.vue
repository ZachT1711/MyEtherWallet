<template>
  <div class="footer">
    <!-- Modal -->
    <feedback-modal />
    <div class="wrap">
      <div>
        <div class="grid-col-1-1-1-2 footer-contents">
          <div
            v-for="(item, index) in footerContent"
            :ref="item.class"
            :key="item.title + index"
            :class="item.class"
          >
            <div class="content-title" @click="toggler(item.class)">
              <h3 class="lite">{{ $t(item.title) }}</h3>
              <p class="open" @click="openContent(item.class)">
                <i class="fa fa-plus" aria-hidden="true" />
              </p>
              <p class="close" @click="closeContent(item.class)">
                <i class="fa fa-minus" aria-hidden="true" />
              </p>
            </div>
            <div class="content-links">
              <div class="content-links-animation-block">
                <div
                  v-for="(content, idx) in item.contents"
                  :key="content.text + idx"
                  class="content"
                >
                  <div v-if="$t(content.text) === $t('common.customerSupport')">
                    <customer-support :no-icon="true" />
                  </div>
                  <router-link
                    v-else-if="content.to !== undefined"
                    :to="content.to"
                  >
                    <p>{{ $t(content.text) }}</p>
                  </router-link>
                  <a
                    v-else-if="content.to === undefined"
                    :href="content.href"
                    :aria-label="$t(content.text)"
                    target="_blank"
                    rel="noopener noreferrer"
                  >
                    <p v-if="item.class === 'e2'">
                      {{ $t(`${content.text}`) }}
                    </p>
                    <p v-else>{{ $t(content.text) }}</p>
                  </a>
                </div>
              </div>
            </div>
          </div>
          <div class="donate-us">
            <div class="content-title">
              <h3 class="lite">
                <i18n path="footer.title.love" tag="h3">
                  <img
                    slot="heart"
                    src="~@/assets/images/icons/heart.svg"
                    alt
                  />
                </i18n>
              </h3>
            </div>
            <div class="links">
              <p>{{ $t('footer.donation.desc') }}</p>

              <a
                :href="'https://etherscan.io/address/' + ethDonationAddress"
                target="_blank"
                rel="noopener noreferrer"
              >
                <p :data-eth="ethDonationAddress" class="crypto-link">
                  <img src="@/assets/images/currency/eth.svg" alt />
                  &nbsp;{{ $t('footer.donation.ether') }}
                </p>
              </a>

              <a
                href="https://blockchain.info/address/1DECAF2uSpFTP4L1fAHR8GCLrPqdwdLse9"
                target="_blank"
                rel="noopener noreferrer"
              >
                <p
                  class="crypto-link no-padding"
                  data-btc="1DECAF2uSpFTP4L1fAHR8GCLrPqdwdLse9"
                >
                  <img src="@/assets/images/currency/btc.svg" alt />
                  &nbsp;{{ $t('footer.donation.bitcoin') }}
                </p>
              </a>
            </div>
          </div>
        </div>
        <div class="flex-space-between foot-note">
          <div class="links">
            <div v-for="(link, index) in lowerLinks" :key="link.title + index">
              <router-link v-if="link.hasOwnProperty('to')" :to="link.to">
                <span>{{ $t(link.title) }}</span>
              </router-link>
              <a
                v-else
                :href="link.href"
                target="_blank"
                rel="noopener noreferrer"
              >
                <span>{{ $t(link.title) }}</span>
              </a>
            </div>
          </div>
          <div class="copyright">
            <p>
              {{ $t('footer.pricing-p') }}
              <a
                href="https://coingecko.com/"
                target="_blank"
                rel="noopener noreferrer"
                >{{ $t('footer.coingecko') }}</a
              >
              <br />
              {{ $t('footer.copyright') }}
            </p>
          </div>
          <div class="social">
            <a
              v-for="link in links"
              :key="link.class"
              :href="link.to"
              :aria-label="link.to"
              rel="noopener noreferrer"
              target="_blank"
            >
              <i :class="'fa ' + link.class" />
            </a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapState } from 'vuex';
import FeedbackModal from '@/components/FeedbackModal';
import CustomerSupport from '@/components/CustomerSupport';
import affiliates from './affiliates.js';
const version = VERSION;
import { Misc } from '@/helpers';

export default {
  components: {
    'feedback-modal': FeedbackModal,
    'customer-support': CustomerSupport
  },
  data() {
    const isMewCx = Misc.isMewCx();
    return {
      isMewCx: isMewCx,
      version: version,
      lowerLinks: [
        {
          title: 'footer.feedback',
          href: 'mailto:support@myetherwallet.com'
        },
        {
          title: 'footer.privacy',
          to: '/privacy-policy'
        },
        {
          title: 'common.terms',
          to: '/terms-of-service'
        },
        {
          title: `v${version}`,
          href: `https://github.com/MyEtherWallet/MyEtherWallet/releases/tag/v${version}`
        }
      ],
      footerContent: [
        {
          class: 'e1',
          title: 'footer.title.discover',
          contents: [
            {
              text: 'footer.units',
              to: '/convert-units'
            },
            // {
            //   text: 'footer.advanced',
            //   to: '/advanced-tools'
            // },
            {
              text: 'footer.extension',
              href: 'https://www.mewcx.com'
            },
            {
              text: 'Buy a Hardware wallet',
              to: '/hardware-wallet-affiliates'
            },
            {
              text: 'footer.sendOffline',
              to: '/send-offline-helper'
            },
            {
              text: 'verifyMessage.title',
              to: '/verify-message'
            },
            {
              text: 'footer.view-wallet-info',
              to: '/view-wallet-info'
            },
            {
              text: 'Submit Dapp',
              to: '/dapp-submission'
            }
          ]
        },
        {
          class: 'e2',
          title: 'footer.title.affiliates',
          contents: affiliates
        },
        {
          class: 'e3',
          title: 'footer.title.mew',
          contents: [
            {
              text: 'footer.about',
              to: '/#about-mew'
            },
            {
              text: 'footer.team',
              to: '/team'
            },
            {
              text: 'common.faqs',
              to: '/#faqs'
            },
            {
              text: 'common.mewtopia',
              href: 'https://www.mewtopia.com'
            },
            {
              text: 'common.customerSupport',
              href: 'mailto:support@myetherwallet.com'
            },
            {
              text: 'common.help-center',
              href: 'https://kb.myetherwallet.com'
            }
          ]
        }
      ],
      links: [
        {
          to: 'https://www.facebook.com/myetherwallet',
          class: 'fa-facebook'
        },
        {
          to: 'https://www.twitter.com/@myetherwallet',
          class: 'fa-twitter'
        },
        {
          to: 'https://www.instagram.com/myetherwallet',
          class: 'fa-instagram'
        },
        {
          to: 'https://www.linkedin.com/company/myetherwallet',
          class: 'fa-linkedin'
        },
        {
          to: 'https://www.github.com/myetherwallet',
          class: 'fa-github'
        },
        {
          to: 'https://www.reddit.com/r/myetherwallet',
          class: 'fa-reddit-alien'
        },
        {
          to: 'https://www.medium.com/@myetherwallet',
          class: 'fa-medium'
        },
        {
          to: 'https://t.me/myetherwalletofficial',
          class: 'fa-telegram'
        }
      ]
    };
  },
  computed: {
    ...mapState('main', ['ethDonationAddress'])
  },
  mounted() {
    if (Misc.isMewCx()) {
      this.footerContent[0].contents = this.footerContent[0].contents.filter(
        item => {
          if (item.to !== '/send-offline-helper') return item;
        }
      );

      this.footerContent[2].contents = this.footerContent[2].contents.filter(
        item => {
          if (item.to !== '/#about-mew' && item.to !== '/#faqs') return item;
        }
      );
    }
  },
  methods: {
    openFeedbackModal() {
      this.$children[0].$refs.feedback.show();
    },
    openContent(element) {
      const openButton = document.querySelector('.' + element + ' .open');
      const closeButton = document.querySelector('.' + element + ' .close');
      const content = document.querySelector('.' + element + ' .content-links');
      openButton.style.display = 'none';
      closeButton.style.display = 'block';
      content.classList.remove('mobile-hide');
    },
    closeContent(element) {
      const openButton = document.querySelector('.' + element + ' .open');
      const closeButton = document.querySelector('.' + element + ' .close');
      const content = document.querySelector('.' + element + ' .content-links');
      openButton.style.display = 'block';
      closeButton.style.display = 'none';
      content.classList.add('mobile-hide');
    },
    toggler(ref) {
      const el = this.$refs[ref][0];
      el.classList.toggle('content-open');
    }
  }
};
</script>

<style lang="scss" scoped>
@import 'CxFooter.scss';
</style>
