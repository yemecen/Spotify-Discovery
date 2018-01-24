<template>
  <div class="container">
	    <div class="row">
		   
		    <div v-for="item in categories.items" class="col-xs-1">
		      <img :src="item.icons[0].url" width="80" height="80" class="img-circle"><br>
		      <a v-on:click="CategoryPlayList" :id="item.id" style="cursor:pointer">{{item.name}}</a>
		    </div>	
	    </div>

	    <div class="row"> 
	    	<hr>
	    	<div  v-for="item in playlists.items" class="demo col-xs-1">
	      	  <a v-on:click="PlayListSongList" :id="`${item.id}_${item.owner.id}`" style="cursor:pointer">{{item.name}}</a>
	      	</div>
	    </div>

	    <div class="row"> 
	    	<hr>
	    	<div  v-for="item in tracks.items" class="col-xs-1">
	      	  <code v-on:click="Play" :id="item.track.id" style="cursor:pointer">{{item.track.album.artists[0].name}}-{{item.track.name}}</code>
	      	</div>
	    </div>
    
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'Category',
  data () {
    return {
    	categories:[],
    	playlists:[],
    	tracks:[],
    	track:[]
    }
  },
  methods:{
  	CategoryPlayList(e){
  		let categoryId=e.target.id;

  		axios.get(`https://api.spotify.com/v1/browse/categories/${categoryId}/playlists`,{params:{locate:'tr_TR',country:'TR'},headers:{Authorization:'Bearer '+localStorage.getItem('AccessToken')}})
	          .then((response)=>{
	              this.playlists=response.data.playlists;
	              this.tracks=[];
	          })
	          .catch((error)=>{
	            console.log(error);
	          });
  	},
  	PlayListSongList(e){
  		let playlistIdAndOwner=e.target.id;
  		let playlistIdAndOwnerSplit=playlistIdAndOwner.split("_")// [0] playlist id, [1] Owner(user_id)

  		axios.get(`https://api.spotify.com/v1/users/${playlistIdAndOwnerSplit[1]}/playlists/${playlistIdAndOwnerSplit[0]}/tracks`,{headers:{Authorization:'Bearer '+localStorage.getItem('AccessToken')}})
	          .then((response)=>{
	              this.tracks=response.data;
	              //console.log(this.tracks);
	          })
	          .catch((error)=>{
	            console.log(error);
	          });
  	},
  	Play(e){
  		let trackid=e.target.id;

  		axios.get(`https://api.spotify.com/v1/tracks/${trackid}`,{headers:{Authorization:'Bearer '+localStorage.getItem('AccessToken')}})
	          .then((response)=>{
	              this.track=response.data;
	          })
	          .catch((error)=>{
	              console.log(error);
	          });
  		
  		axios.put(`https://api.spotify.com/v1/me/player/play`,{context_uri:this.track.album.uri,offset:{position:(this.track.track_number-1)}},{headers:{Authorization:'Bearer '+localStorage.getItem('AccessToken')}})
	          .then((response)=>{
	              console.log("Playing");
	          })
	          .catch((error)=>{
	              console.log(error);
	          });
  	}
  },
  created(){
 
	 axios.get('https://api.spotify.com/v1/browse/categories',{params:{locate:'tr_TR',country:'TR',limit:20},headers:{Authorization:'Bearer '+localStorage.getItem('AccessToken')}})
	          .then((response)=>{
	              this.categories=response.data.categories;
	              //console.log(response.data);
	          })
	          .catch((error)=>{
	            console.log(error);
	          });
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
 .demo{
 	color:blue;
 }
</style>
