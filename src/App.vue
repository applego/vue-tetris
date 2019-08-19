<template>
  <div>
    <myheader></myheader>
    <myheader></myheader>
    <p v-if="msg.length > 0">
      {{msg}}
    </p>
    <p v-else>
      no text
    </p>
    <input type="text" v-model="msg">
    <button @click="clear()">clear</button>
    <p>button ver.</p>
    <input type="text" v-model="poscode">
    <button @click="clear_pos()">clear</button>
    <button @click="showcity()">show city</button>
    <p v-if="poscode.length > 0">
      {{city}}
    </p>
    <p v-else>
      no text
    </p>
  </div>
</template>

<script>
import myheader from './components/myheader'

export default {
  components:{
    myheader
  },
  data(){
    return{
      msg:'Hello World!',
      poscode:'10504',
      city:''
    }
  },
  methods:{
    clear(){
      this.msg = ''
    },
    clear_pos(){
      this.poscode = ''
    },
    showcity(){
      var url = 'http://www.geonames.org/postalCodeLookupJSON?postalcode='+this.poscode+'&country=US';
      fetch(url)
      .then(response=>{
        return response.json()
      })
      .then(json=>{
        this.city = json.postalcodes[0].adminName1
      })
      .catch((err)=>{
        this.city = err //エラー処理
      });
    }
  },
  created(){
    fetch('http://www.geonames.org/postalCodeLookupJSON?postalcode=10504&country=US')
    .then(response=>{
      return response.json()
    })
    .then(json=>{
      this.msg = json.postalcodes[0].adminName1
    })
    .catch((err)=>{
      this.msg = err //エラー処理
    });
  }
}
</script>