<template>
  <v-app>
  <v-container class="form">
    <v-sheet>
      <v-container>
  <v-form>
    <v-col>
      <v-row>
        <p> Enter the data to write to XDC Network</p>
        <v-text-field placeholder="Amount" v-model="input" outlined></v-text-field>
      </v-row>
      <v-row>
        <v-btn @click="push()" block outlined> Push to chain</v-btn>
      </v-row>
    </v-col>
  </v-form>
  </v-container>
</v-sheet>
<br/><br/>

<v-sheet>
  <v-container>
<v-form>
  <v-col>
    <v-row>
      <v-btn @click="get()" block outlined> Get Data</v-btn>
    </v-row>
    <v-row>
       <h1 class="text-center">{{ balance}}</h1>
    </v-row>
  </v-col>
</v-form>
</v-container>
</v-sheet>
</v-container>
</v-app>
</template>

<script>
  import Web3 from "xdc3"
export default {
  name: 'IndexPage',
  // the connection to wallet is checked using when mounted
  async mounted () {
    if (window.ethereum) {
        window.web3 = new Web3(window.ethereum);
        this.connected = true;
      } else if (window.web3) {
        window.web3 = new Web3(window.web3.currentProvider);
      } else {
        window.alert("Non-Xinfin browser detected. You should consider trying XDCpay");
      }
      const web3 = window.web3;
      const accounts = await web3.eth.getAccounts();
      let accountBalance = await web3.eth.getBalance(accounts[0])

  },
  data: () => ({
    connected:false,
    balance:0,
    input:0,
  }),
  methods:{
    async get(){
      // the contract address is defined in the call because if we need to call a different contract we can do that 
      let contractAddress = "xdc4E09655eA0C15f2c40F22816d59E1a9Bf529627d"
      // import  the abi . please change the abi if you are using a different contract
      var abi = require('./abi.json');
      // declaring the contract
      let contract = new web3.eth.Contract(abi, contractAddress)
      // calling the retrieve function which will defined in the smart contract in the Storage.sol file
      let value =await contract.methods.retrieve().call()
      // the result is assigned to the balance variable to reflect in the UI
      this.balance = value;

    },
    async push(){
      let contractAddress = "xdc4E09655eA0C15f2c40F22816d59E1a9Bf529627d"
      var abi = require('./abi.json');
      let contract = new web3.eth.Contract(abi, contractAddress)
      // the store method defined in the smart contract Storage.sol is called and the integer value is passed to the funtion also the wallet address of the sender is specified
      let result = await contract.methods.store(this.input).send({from:'xdc4a0ff01c148baac9f0e944439627b175d1c5280b'});
      console.log(result);
    }
  }
}
</script>
<style>
  .form{
    width: 400px;
    margin: 10% auto;
  }
</style>
