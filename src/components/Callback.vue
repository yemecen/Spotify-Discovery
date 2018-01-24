<template>
  <div>

  </div>
</template>

<script>

export default {
  name: 'Callback',
  data () {
    return {
    }
  },
  created(){

  	   localStorage.setItem('AccessToken', this.$route.query.AccessToken);
	     localStorage.setItem('RefreshToken', this.$route.query.RefreshToken);
	  
       this.$router.push('/');

  }  
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
 
</style>
