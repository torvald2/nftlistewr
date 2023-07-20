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
      prices:{'1': 260,
 '3': 260,
 '4': 130,
 '5': 130,
 '6': 130,
 '7': 130,
 '8': 130,
 '9': 260,
 '11': 260,
 '12': 260,
 '14': 130,
 '15': 260,
 '16': 130,
 '18': 260,
 '19': 260,
 '20': 130,
 '21': 130,
 '23': 130,
 '24': 260,
 '25': 130,
 '26': 260,
 '27': 260,
 '29': 130,
 '30': 130,
 '31': 130,
 '33': 260,
 '34': 130,
 '35': 130,
 '36': 130,
 '37': 260,
 '38': 130,
 '39': 130,
 '41': 260,
 '42': 260,
 '43': 260,
 '44': 260,
 '45': 130,
 '46': 260,
 '47': 130,
 '48': 260,
 '49': 130,
 '50': 130,
 '51': 260,
 '52': 260,
 '53': 130,
 '54': 130,
 '55': 260,
 '56': 260,
 '57': 130,
 '59': 260,
 '60': 260,
 '61': 260,
 '63': 130,
 '64': 260,
 '65': 260,
 '66': 130,
 '67': 260,
 '68': 130,
 '69': 130,
 '70': 260,
 '71': 130,
 '72': 130,
 '73': 260,
 '74': 260,
 '75': 130,
 '76': 130,
 '77': 260,
 '78': 260,
 '79': 130,
 '80': 130,
 '82': 130,
 '83': 260,
 '84': 130,
 '85': 260,
 '86': 130,
 '87': 130,
 '88': 260,
 '90': 130,
 '91': 260,
 '92': 260,
 '93': 260,
 '94': 260,
 '95': 130,
 '96': 260,
 '97': 260,
 '98': 260,
 '99': 260,
 '100': 260,
 '101': 260}
    }
  },

  methods:{
    async list(){
      this.messages.push({variant: "success", text:"Start listing",key:"00"})
      const provider = new ethers.providers.Web3Provider(window.ethereum);

      // Initialize the signer with the provider
const signer = provider.getSigner();
    const openseaSDK = new OpenSeaSDK(signer, {
    chain: Chain.Polygon,
    apiKey: this.openseaApiKey
  })
  const expirationTime = Math.round(Date.now() / 1000 + 60 * 60 * 24*180);
 
  for (const [key, value] of Object.entries(this.prices)) {
    const asset = {
    tokenAddress:  "0x3d13b01b0D01fcd3e481598051E79B038816ACc4", // string
    tokenId:key, // string | number | BigNumber | null
  }
    try {
  await openseaSDK.createSellOrder({
    asset,
    accountAddress:"0x6f39F2c479876bF3f3bB616Bf63812EEB1686b1C",
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
    accountAddress:"0x6f39F2c479876bF3f3bB616Bf63812EEB1686b1C",
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
