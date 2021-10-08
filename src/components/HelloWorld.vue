<template>
    <div class="hello">
    <div v-if="!isConnect">
    <button  type="button" @click='connect()'>Connect Wallet</button>
    </div>
    <div v-if="isConnect">
    <span>Connected ! your address is {{ sender }}</span>
    </div>
    <br>
    <br>
                <input v-model="tokenid" placeholder="token_id" size="100" />
                <br>
                <br>
     
            
                <input v-model="recipient" placeholder="recipient" size="100"/>
                <br>
                <br>


    <button type="button" @click='transfer()'>Transfer</button>
    <br>
    <br>
    <span>donate : terra1rvg5x3uhqlel0dv9zwh53pcrt5hk9ykpq7u3gd</span>
    <br>
    <br>
    <span><a href="https://github.com/athlona64/cw721transfer">Github</a></span>

 
  </div>
</template>

<script>
import {Extension, Wallet, MsgExecuteContract, MsgSend } from "@terra-money/terra.js";

export default {
   data () {
      return {
          name: 'HelloWorld',
          props: {
            msg: String
          },
          wallet: Wallet,
          tokenid:'',
          extension:'',
          recipient:'',
          isConnect: false,
          sender:''
     
   
      }
    },
    mounted() {


    },
    methods : {
    async connect() {
      console.log('click connect');
        
       this.wallet =  Wallet;
       this.extension = new Extension();
      this.extension.connect();
       this.extension.on("onConnect", xxx=>{
     
        this.sender = xxx.address;

        this.isConnect = true;
       })
      


 
   
    },
    async transfer()  {
      console.log(this.wallet);


      const execute = new MsgExecuteContract(
        this.sender, // sender
        'terra1f89xq3qhu98v4jch4y5xcrkhl9gytrne99x74t', // contract account address
        {
          "transfer_nft": {
              "recipient": this.recipient,
              "token_id": this.tokenid
            } 
        }, // handle msg
      );
      console.log(execute);
      console.log(MsgSend);
          this.extension.post({
            msgs: [execute]
          });     
      // const executeTx = await this.wallet.key.createAndSignTx({
      //   msgs: [execute]
      // });

      // const executeTxResult = await terra.tx.broadcast(executeTx);
      // console.log(executeTxResult);
    }
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
