<template>
  <web3-connection-placeholder :classes="['page-content']" tag="main">
    <div class="container">
      <div class="card card-body mx-auto mt-5 form-container">
        <web3-form :callback="handleSend">
          <fieldset class="mb-3">
            <label for="">Send To:</label>
            <input
              v-model="receiver"
              class="form-control"
              type="text"
              placeholder="Receiver Address"
            />
          </fieldset>
          <fieldset class="mb-3">
            <label for="">Amount ({{balanceOf('ETH')}}):</label>
            <input
              v-model.number="amount"
              class="form-control"
              type="text"
              placeholder="Send Amount"
            />
          </fieldset>
          <button class="btn btn-success w-100" type="submit">Send</button>
        </web3-form>
      </div>
    </div>
    <div
      slot="connect"
      class="d-flex h-100 w-100 align-self-center justify-content-center"
    >
      <div class="text-center">
        <p>Please, connect your wallet to use the app</p>
        <button class="btn btn-primary btn-lg" @click="$nuxt.$emit('popups.unlockwallet.open')">
          Connect
        </button>
      </div>
    </div>
  </web3-connection-placeholder>
</template>

<script>
import { mapActions } from "vuex";
import user from "@/mixins/user";

export default {
	name: 'SendEth',
	mixins: [user],
	data() {
		return {
			receiver: null,
			amount: null,
		};
	},
	methods: {
		...mapActions({
			send: "user/assets/sendNative",
		}),
		handleSend() {
			return this.$web3.eth.sendTransaction({
				from: this.address,
				to: this.receiver,
				value: this.$web3.utils.toWei(this.amount.toString()),
			});
		},
	},
};
</script>

<style lang="scss">
.form-container {
  max-width: 400px;
}
</style>
