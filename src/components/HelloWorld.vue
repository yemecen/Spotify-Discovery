<template>
  <div class="hello">
      <div v-if="AccessToken == null && RefreshToken == null">
      <button v-on:click="Login"><img src="../assets/logo.png" width="300" height="300"></button>
      <h2>Spotify Discovery</h2>

    </div>
    
    <div v-if="AccessToken != null && RefreshToken != null">
        <img :src="User.images[0].url" width="200" height="200" class="img-rounded">
        
        <br>
        <code class="text-center">{{User.id}}</code>
        <br>
    
        <hr>
         <category></category> 
    </div>  

  </div>
</template>

<script>
import axios from 'axios'
import Category from '@/components/Category.vue'

export default {
  name: 'HelloWorld',
  components :{Category},
  data () {
    return {
      AuthApiUrl: 'http://localhost/api/public',
      AccessToken:'',
      RefresToken:'',
      User:[]
    }
  },
  
  methods:{
       Login(){
         
         axios.get(`${this.AuthApiUrl}/login`)
          .then(function (response) {
              console.log(response.data);
              window.location.href=response.data;
          })
          .catch(function  (error) {
            console.log(error);
          });
          
         
      },
      UserInfo(){

        axios.get('https://api.spotify.com/v1/me',{headers:{Authorization:'Bearer '+this.AccessToken}})
          .then((response)=>{
              this.User=response.data;
              //console.log(response.data);
          })
          .catch((error)=>{
            console.log(error);
          });
      }
  },

  created(){
      //console.log(localStorage.getItem('AccessToken'));
      this.AccessToken=localStorage.getItem('AccessToken');
      //console.log(localStorage.getItem('RefreshToken'));
      this.RefreshToken=localStorage.getItem('RefreshToken');

      this.UserInfo();
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
