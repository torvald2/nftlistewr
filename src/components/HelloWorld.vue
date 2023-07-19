<template>
  <div>
  <h1>Lions Lister</h1>
  <b-form inline>
  <b-form-input  type="text" v-model="openseaApiKey" placeholder="openseaApiKey"></b-form-input>
  <b-button class="mt-2" variant="success" @click="list"> Start listing </b-button>
  </b-form>
  <b-container>
  <b-row>
    <b-col cols="8">
      <b-alert show :variant="message.variant" v-for="message in messages" :key="message.id" class="mt-2">{{ message.text }}</b-alert>

    </b-col>
  </b-row>
  </b-container>
  </div>
</template>

<script>
import { OpenSeaSDK, Chain } from "opensea-js";
import { ethers } from "ethers";
//0.053
//0.106
function sleep(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
}
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data(){
    return {
      openseaApiKey:"",
      messages:[
      ],
      prices:{
 
  
 '27': 0.106,//
 
 '76': 0.053,//

}
    }
  },

  methods:{
    async list(){
      this.messages.push({variant: "success", text:"Start listing",key:"00"})
      const provider = new ethers.providers.Web3Provider(window.ethereum);

      // Initialize the signer with the provider
const signer = provider.getSigner();
    const openseaSDK = new OpenSeaSDK(signer, {
    chain: Chain.Goerli,
  })
  const expirationTime = Math.round(Date.now() / 1000 + 60 * 60 * 24*180);
 
  for (const [key, value] of Object.entries(this.prices)) {
    const asset = {
    tokenAddress:  "0x5FEFe57C173497b0637a19BA864D460190123f03", // string
    tokenId:key, // string | number | BigNumber | null
  }
    try {
  await openseaSDK.createSellOrder({
    asset,
    accountAddress:"0x59E7f79b16dbf54A1cB51D84acD46B96a60c2015",
    startAmount: value,
    expirationTime,
  });
  this.messages.push({variant: "success", text:`Listed token ${key}`,key:key})

  await sleep(6000)


} catch (e) {
  this.messages.push({variant: "warning", text:`Error listing token ${key}. Trying to retry after 50 seconds.`,key:key})

  await sleep(30000)
  try {
  await openseaSDK.createSellOrder({
    asset,
    accountAddress:"0x59E7f79b16dbf54A1cB51D84acD46B96a60c2015",
    startAmount: value,
    expirationTime,
  });
 } catch (e) {
  this.messages.push({variant: "danger", text:`Error listing token ${key}. Error : ${e.message}`,key:key})

  await sleep(30000)
 }
}

  }

  
    }
  },
  async created(){
    await window.ethereum.send('eth_requestAccounts')

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
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
