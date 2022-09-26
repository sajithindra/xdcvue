# xdcvue

The following example shows how to connect to the xdc waller and configure it with the vue.js using Nuxt.js 

The smartcontract used in the example is Storage.sol which can be found inside the contract in the remix deauls page.

How to test the code 

1. compile the Storage.sol in the remix
2. go to ui folder and install dependencies using "npm install " or npm init, if you don't have nuxt.js please install it too
3. use the smartcontract address in the code in the line 64 in index.vue file under the pages folder inside ui folder in the project folder
4. in the line 75 the send funtion argument please replace the value agains the 'from' key with your wallet id 
5. during the store function call wait until the transaction is completed and notification is poped up in the chrome window
