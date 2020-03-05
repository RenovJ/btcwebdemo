<template>
  <div class="hello">
    <h1>{{ greeting }}</h1>
    <h2>Get balance</h2>
    <div>
      BTC address to check balance<br/>
      <input v-model="balance_addr" placeholder="Enter BTC address to check balance"></input>
      <button v-on:click="getBalance">Get</button>
      <div class="mt-2">Value: {{ balance }} BTC</div>
    </div>
    <br/><br/><br/>
    <h2>Transfer BTC</h2>
    <div>
      BTC amount to transfer(BTC)<br/>
      <input v-model="transfer_amount" placeholder="Enter BTC amount to transfer"></input><br/>
      Sender address<br/>
      <input v-model="transfer_from" placeholder="Enter sender address"></input><br/>
      Sender private key<br/>
      <input v-model="transfer_privkey" placeholder="Enter sender private key"></input><br/>
      Receiver address<br/>
      <input v-model="transfer_to" placeholder="Enter receiver address"></input><br/>
      <button v-on:click="transfer">Transfer</button>
      <div class="mt-2">Result: {{ resultTransfer }}</div>
    </div>
  </div>
</template>

<script>
var bitcoinTransaction = require('bitcoin-transaction');

export default {
  name: 'HelloWorld',
  data () {
    return {
        greeting: 'BTC Web demo',
        balance: '',
        resultTransfer: '',
        balance_addr: '',
        transfer_amount: '',
        transfer_from: '',
        transfer_privkey: '',
        transfer_to: '',
    }
  },
  methods: {
    getBalance: function () {
      bitcoinTransaction.getBalance(this.balance_addr, { network: "mainnet", balanceProvider: bitcoinTransaction.providers.balance.mainnet.blockchain}).then((balanceInBTC) => {
        this.balance = balanceInBTC
      });
    },
    transfer: function () {
      bitcoinTransaction.sendTransaction({
        from: this.transfer_from,
        to: this.transfer_to,
        privKeyWIF: this.transfer_privkey,
        btc: this.transfer_amount,
        network: "mainnet",
        utxoProvider: bitcoinTransaction.providers.utxo.mainnet.blockchain,
        pushtxProvider: bitcoinTransaction.providers.pushtx.mainnet.blockchain,
      }).then((result) => {
        this.resultTransfer = result
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
