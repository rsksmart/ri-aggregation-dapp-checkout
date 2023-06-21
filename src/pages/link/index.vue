<template>
  <div class="linkPage container h-full pt-5 px-3 md:py-10">
    <zk-modal v-model="wrongDataModal" @close="wrongDataModal = false">
      <template slot="header">
        <div class="withIcon text-red">
          <i class="fad fa-info-square" />
          <div>Wrong data</div>
        </div>
      </template>
      <template slot="default">
        <div class="text-center">Some of the data is missing or inputted values are invalid.<br />Please, try again.</div>
      </template>
    </zk-modal>

    <zk-modal v-model="successModal" :not-closable="true" @close="successModal = false">
      <template slot="header">
        <div class="withIcon text-green">
          <i class="fad fa-box-check" />
          <div>Payment link created</div>
        </div>
      </template>
      <template slot="default">
        <div class="text-center leading-tight mb-4">Your payment link has been created!<br />Now you can share it with someone.</div>
        <div class="successLinkContainer">
          <zk-input ref="linkInput" size="sm" :value="paymentLink" readonly @click="$refs.linkInput.focus()" />
          <zk-defbtn id="copy-link" v-popover:copy-link.bottom @click="copyLink()">
            <span>Copy</span>
            <i class="fal fa-clipboard" />
          </zk-defbtn>
          <zk-defbtn outline square @click="twitterShare()">
            <i class="fab fa-twitter" />
          </zk-defbtn>
          <zk-defbtn outline square @click="facebookShare()">
            <i class="fab fa-facebook-f" />
          </zk-defbtn>
          <popover name="copy-link" event="click" class="text-center block text-xs" :delay="100"> Link copied! </popover>
        </div>
      </template>
      <template slot="footer">
        <div class="flex items-center justify-center flex-wrap">
          <zk-defbtn class="mr-3" :disabled="previewLoading" outline @click="successModal = false">
            <i class="far fa-arrow-left" />
            <span>Close</span>
          </zk-defbtn>
          <zk-defbtn :loader="previewLoading" :to="`/link/${paymentHash}`" @click.native="previewLoading = true">
            <span>Try now</span>
            <i v-if="!previewLoading" class="far fa-arrow-right" />
          </zk-defbtn>
        </div>
      </template>
    </zk-modal>

    <div class="linkHeader">
      <nuxt-link to="/link" class="flex items-stretch justify-center">
        <svg width="120" height="30" viewBox="0 0 105 30" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path
            d="M28.544 23V5.384H30.896V23H28.544ZM39.368 23L33.56 15.2H36.32L42.224 23H39.368ZM30.008 16.664V14.576H34.712C35.448 14.576 36.08 14.424 36.608 14.12C37.136 13.816 37.544 13.4 37.832 12.872C38.12 12.328 38.264 11.712 38.264 11.024C38.264 10.336 38.12 9.728 37.832 9.2C37.544 8.656 37.136 8.232 36.608 7.928C36.08 7.608 35.448 7.448 34.712 7.448H30.008V5.384H34.28C35.56 5.384 36.68 5.576 37.64 5.96C38.6 6.344 39.344 6.936 39.872 7.736C40.4 8.536 40.664 9.568 40.664 10.832V11.216C40.664 12.48 40.392 13.512 39.848 14.312C39.32 15.112 38.576 15.704 37.616 16.088C36.672 16.472 35.56 16.664 34.28 16.664H30.008ZM50.6823 23.432C49.5623 23.432 48.5783 23.24 47.7303 22.856C46.8823 22.472 46.1623 21.968 45.5703 21.344C44.9943 20.704 44.5543 19.992 44.2503 19.208C43.9623 18.424 43.8183 17.624 43.8183 16.808V16.376C43.8183 15.56 43.9703 14.76 44.2743 13.976C44.5783 13.176 45.0263 12.464 45.6183 11.84C46.2103 11.2 46.9303 10.696 47.7783 10.328C48.6263 9.944 49.5943 9.752 50.6823 9.752C51.7703 9.752 52.7383 9.944 53.5863 10.328C54.4343 10.696 55.1543 11.2 55.7463 11.84C56.3383 12.464 56.7863 13.176 57.0903 13.976C57.3943 14.76 57.5463 15.56 57.5463 16.376V16.808C57.5463 17.624 57.3943 18.424 57.0903 19.208C56.8023 19.992 56.3623 20.704 55.7703 21.344C55.1943 21.968 54.4823 22.472 53.6343 22.856C52.7863 23.24 51.8023 23.432 50.6823 23.432ZM50.6823 21.368C51.6423 21.368 52.4583 21.16 53.1303 20.744C53.8183 20.312 54.3383 19.736 54.6903 19.016C55.0583 18.296 55.2423 17.488 55.2423 16.592C55.2423 15.68 55.0583 14.864 54.6903 14.144C54.3223 13.424 53.7943 12.856 53.1063 12.44C52.4343 12.024 51.6263 11.816 50.6823 11.816C49.7543 11.816 48.9463 12.024 48.2583 12.44C47.5703 12.856 47.0423 13.424 46.6743 14.144C46.3063 14.864 46.1223 15.68 46.1223 16.592C46.1223 17.488 46.2983 18.296 46.6503 19.016C47.0183 19.736 47.5383 20.312 48.2103 20.744C48.8983 21.16 49.7223 21.368 50.6823 21.368ZM61.1411 23V5.48H63.4451V23H61.1411ZM59.3651 7.304V5.48H63.4451V7.304H59.3651ZM68.0083 23V5.48H70.3123V23H68.0083ZM66.2323 7.304V5.48H70.3123V7.304H66.2323ZM79.1714 23.384C77.6514 23.384 76.4834 22.888 75.6674 21.896C74.8514 20.904 74.4434 19.472 74.4434 17.6V10.16H76.7474V18.056C76.7474 19.064 77.0194 19.856 77.5634 20.432C78.1074 20.992 78.8434 21.272 79.7714 21.272C80.7314 21.272 81.5154 20.968 82.1234 20.36C82.7314 19.736 83.0354 18.888 83.0354 17.816V10.16H85.3394V23H83.5154V17.504H83.8034C83.8034 18.768 83.6354 19.84 83.2994 20.72C82.9634 21.584 82.4594 22.248 81.7874 22.712C81.1314 23.16 80.2914 23.384 79.2674 23.384H79.1714ZM89.8018 27.8V10.184H91.6258V15.728L91.1938 15.68C91.2738 14.304 91.5618 13.184 92.0578 12.32C92.5698 11.44 93.2338 10.792 94.0498 10.376C94.8658 9.96 95.7538 9.752 96.7138 9.752C97.6418 9.752 98.4818 9.928 99.2338 10.28C100.002 10.632 100.658 11.112 101.202 11.72C101.746 12.328 102.162 13.032 102.45 13.832C102.738 14.632 102.882 15.48 102.882 16.376V16.808C102.882 17.704 102.73 18.552 102.426 19.352C102.122 20.152 101.69 20.856 101.13 21.464C100.586 22.072 99.9298 22.552 99.1618 22.904C98.4098 23.256 97.5778 23.432 96.6658 23.432C95.7538 23.432 94.8818 23.232 94.0498 22.832C93.2178 22.432 92.5298 21.784 91.9858 20.888C91.4418 19.992 91.1298 18.816 91.0498 17.36L92.1058 19.28V27.8H89.8018ZM96.3058 21.416C97.1698 21.416 97.9218 21.216 98.5618 20.816C99.2018 20.4 99.6978 19.832 100.05 19.112C100.402 18.376 100.578 17.536 100.578 16.592C100.578 15.632 100.402 14.792 100.05 14.072C99.6978 13.352 99.2018 12.792 98.5618 12.392C97.9218 11.976 97.1698 11.768 96.3058 11.768C95.5378 11.768 94.8258 11.944 94.1698 12.296C93.5138 12.632 92.9858 13.128 92.5858 13.784C92.2018 14.424 92.0098 15.2 92.0098 16.112V17.216C92.0098 18.096 92.2098 18.848 92.6098 19.472C93.0098 20.096 93.5378 20.576 94.1938 20.912C94.8498 21.248 95.5538 21.416 96.3058 21.416Z"
            fill="black" />
          <path
            d="M22.769 15.8L21.5602 15.0013L22.7699 14.2016C23.5398 13.6931 24 12.8305 24 11.8951C24 10.9598 23.5398 10.0981 22.7699 9.58864L13.4659 3.44224C12.5755 2.85209 11.4245 2.85308 10.5331 3.44224L1.23102 9.58864C0.461149 10.0972 0.000964723 10.9598 0.000964723 11.8951C0.000964723 12.7381 0.374322 13.519 1.01105 14.0384L2.44563 14.9964L1.23005 15.799C0.460184 16.3075 0 17.1701 0 18.1055C0 18.9484 0.373357 19.7294 1.01009 20.2487L4.60088 22.6457L4.60474 22.6427L10.5312 26.5594C10.9769 26.8534 11.4872 27 11.9976 27C12.5079 27 13.0183 26.8525 13.464 26.5594L17.7195 23.7473L17.7591 23.7217L19.3904 22.6437L22.7671 20.413C23.5369 19.9044 23.9971 19.0418 23.9971 18.1065C23.9971 17.1711 23.5369 16.3095 22.7671 15.8H22.769ZM2.31829 12.4922C2.03079 12.3023 1.99992 12.0112 1.99992 11.8951C1.99992 11.7791 2.03079 11.487 2.31829 11.2981L11.6213 5.15072C11.8529 4.99827 12.15 4.99827 12.3806 5.15072L21.6827 11.2981C21.9702 11.4879 22.001 11.7791 22.001 11.8951C22.001 12.0112 21.9702 12.3023 21.6827 12.4922L19.7233 13.7876L13.4669 9.65356C12.5764 9.06341 11.4255 9.06439 10.5341 9.65356L4.27769 13.7876L2.31925 12.4932L2.31829 12.4922ZM17.8874 15.0003L12.3796 18.6396C12.1491 18.793 11.8509 18.792 11.6204 18.6396L6.1136 15.0003L11.6213 11.3601C11.8529 11.2076 12.15 11.2076 12.3806 11.3601L17.8874 14.9993V15.0003ZM21.6827 18.7025L12.3806 24.8509C12.15 25.0043 11.8519 25.0033 11.6213 24.8509L2.31925 18.7035C2.03176 18.5137 2.00088 18.2225 2.00088 18.1065C2.00088 17.9904 2.03176 17.6983 2.31925 17.5094L4.27479 16.217L4.60281 16.4363L4.60666 16.4334L10.5331 20.35C10.9788 20.6441 11.4892 20.7906 11.9995 20.7906C12.5099 20.7906 13.0202 20.6431 13.4659 20.35L17.7214 17.5379L17.761 17.5124L19.3924 16.4344L19.7233 16.216L21.6827 17.5104C21.9702 17.7002 22.001 17.9914 22.001 18.1074C22.001 18.2235 21.9702 18.5146 21.6827 18.7045V18.7025Z"
            fill="black" />
        </svg>
        <div class="brandContainer text-violet -dark text-2xl font-bold flex flex-col lg:flex-row items-end md:items-start md:gap-2 mr-5 lg:justify-start leading-1">
          <h1 class="leading-1 -mb-1 lg:m-0 w-auto">Checkout</h1>
          <span v-if="!isMainnet" class="networkName text-sm font-light"> {{ currentNetwork }}</span>
        </div>
      </nuxt-link>
      <ul
        v-if="!showAddLink"
        class="feature-list zk-container font-light text-dark mt-4 mr-auto ml-auto mb-6 text-sm md:text-md flex-col items-center text-gray-600 dark:text-gray-100">
        <li class="flex-grow headline big text-violet mb-3">Get paid in tokens with RIF Rollup:</li>
        <li class="mb-3">
          <i class="fad fa-check text-violet mr-2" />
          <span>Blazing-fast & cost efficient</span>
        </li>
        <li class="mb-3">
          <i class="fad fa-check text-violet mr-2" />
          <span>Permissionless visual link builder</span>
        </li>
        <li class="mb-3">
          <i class="fad fa-check text-violet mr-2" />
          <span>Up to {{ maxPayments }} transactions with different recipients</span>
        </li>
        <li class="mb-3">
          <i class="fad fa-check text-violet mr-2" />
          <span><a href="https://zksync.io/api/sdk/checkout/" CLASS="lightLink" target="_blank">RIF Rollup Checkout SDK</a>&nbsp;for the fully-featured checkout</span>
        </li>
        <li class="mb-3">
          <i class="fad fa-check text-violet mr-2" />
          <span>Up to 70 transaction with customizable description & purpose using SDK</span>
        </li>
      </ul>
    </div>
    <div ref="paymentsContainer" class="linkBody py-4 md:py-10">
      <!-- eslint-disable-next-line vue/no-v-html -->
      <h2 class="mx-auto text-center zk-container headline big text-violet mb-3" v-html="createLinkBlockTitle" />
      <zk-defbtn v-if="!showAddLink" class="mx-auto mt-5" big @click="enableLink()">Try it now</zk-defbtn>
      <template v-if="showAddLink">
        <div v-for="index in payments.keys()" :key="index" class="paymentContainer w-full py-2 md:py-1">
          <payment-item v-model="payments[index]" :display-index="payments.length >= 2" :display-delete="payments.length >= 2" :index="index" @delete="deletePayment(index)" />
        </div>
      </template>
      <zk-defbtn v-if="showAddLink" outline class="mx-auto mt-5" :disabled="payments.length >= maxPayments" @click="addPayment()">Add another transaction</zk-defbtn>
      <div v-if="showAddLink" class="text-gray text-sm text-center leading-tight pt-2" :class="{ 'text-dark': payments.length >= maxPayments }">
        {{ payments.length >= 5 ? `${payments.length}/` : "Up to " }}{{ maxPayments }} transactions
      </div>
    </div>
    <div class="linkFooter filter rounded-b px-5">
      <zk-max-height v-show="payments.length < 3" :value="!validConfig && showAddLink" class="mt-0 md:mt-5 md:mt-7 zk-container mx-auto">
        <div>
          <zk-note class="notificationNote">
            <template slot="icon">
              <i class="text-gray text-xl fal fa-info-square" />
            </template>
            <template slot="default">
              <div class="text-sm text-gray font-light">
                To unlock <span class="font-normal">“Build your payment link”</span> button below make sure to fill-up <span class="font-normal">“Receiver ETH address”</span> and
                <span class="font-normal">“Amount”</span> {{ payments.length > 1 ? `for each of ${payments.length} transactions` : `of the transaction` }}.
              </div>
            </template>
          </zk-note>
        </div>
      </zk-max-height>
      <zk-defbtn v-if="showAddLink" class="mx-auto mt-5 md:mt-5 shadow-sm" :outline="!validConfig" :disabled="!validConfig" big @click="generate()"
        >Build your payment link</zk-defbtn
      >
      <div class="poweredBy pt-5 md:pt-10 pb-0 md:pb-5 flex items-center justify-between">
        <block-footer :full-footer-menu="true" />
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import { Network } from "@rsksmart/rif-rollup-js-sdk/build/types";
import { copyToClipboard } from "@rsksmart/rif-rollup-nuxt-core/utils";
import { FACEBOOK_URL, TWEET_URL } from "@/plugins/build";
import { encrypt } from "@/plugins/link";
import { checkAddress } from "@/plugins/utils";
import { PaymentItem } from "@/types";

export default Vue.extend({
  layout: "link",
  data() {
    return {
      addLinkMode: false,
      payments: [] as PaymentItem[],
      wrongDataModal: false,
      successModal: false,
      previewLoading: false,
      paymentHash: "",
      maxPayments: 20,
    };
  },
  computed: {
    currentNetwork(): Network {
      return this.$store.getters["zk-provider/network"];
    },
    isMainnet(): boolean {
      return this.currentNetwork === "mainnet";
    },
    paymentLink(): string {
      return window.location.origin + "/link/" + this.paymentHash;
    },
    showAddLink(): boolean {
      return this.addLinkMode;
    },
    createLinkBlockTitle(): string {
      const ethNetwork = this.isMainnet ? "" : `<strong>${this.currentNetwork}</strong>`;
      return this.showAddLink ? `Compose batch of your ${ethNetwork} payments:` : `Build instant ${ethNetwork} payout link in 5 min`;
    },
    validConfig(): boolean {
      if (this.payments.length < 1) {
        return false;
      }
      for (const payment of this.payments) {
        if ((!this.isValidDomain(payment.address, payment.token) && !checkAddress(payment.address)) || !payment.amount) {
          return false;
        }
      }
      return true;
    },
  },
  methods: {
    deletePayment(index: number) {
      this.payments.splice(index, 1);
    },
    addPayment() {
      if (this.payments.length >= this.maxPayments) {
        return;
      }
      let address = "";
      let token = "ETH";
      if (this.payments.length > 0) {
        address = this.payments[this.payments.length - 1].address;
        token = this.payments[this.payments.length - 1].token;
      }
      this.payments.push({
        address,
        amount: "",
        token,
      });
      this.getDomainAddress(address, token);
    },
    enableLink() {
      this.addLinkMode = true;
      this.addPayment();
    },
    generate() {
      for (const payment of this.payments) {
        if (!payment.address || !payment.amount) {
          return (this.wrongDataModal = true);
        }
      }
      this.paymentHash = encrypt(this.payments);
      this.successModal = true;
    },
    copyLink() {
      copyToClipboard(this.paymentLink);
    },
    twitterShare() {
      window.open(`${TWEET_URL}${encodeURIComponent(this.paymentLink)}`, "_blank");
    },
    facebookShare() {
      window.open(`${FACEBOOK_URL}${encodeURIComponent(this.paymentLink)}`, "_blank");
    },
    isValidDomain(address: string, token: string): boolean {
      return (this as any).$domainResolver.isValidAddress(address, token);
    },
    async getDomainAddress(currentAddress: string, token: string) {
      if (!this.isValidDomain(currentAddress, token)) {
        await (this as any).$domainResolver.lookupDomain(currentAddress, token);
      }
    },
  },
});
</script>
