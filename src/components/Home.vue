<template>
   <div id="home">
   <h1>{{ msg }}</h1>
   <button @click="googleLogin">Google アカウントでログイン</button>
   </div>
</template>

<script>
export default {
   name: 'home',
   data () {
   return {
   msg: 'Welcome to MyMarkdown'
   }
   },
   methods: {
     googleLogin: function() {
       firebase.auth().signInWithRedirect(
            new firebase.auth.GoogleAuthProvider()
       );
     }
   }
  }
</script>

<style lang="scss" scoped>
button {
  border: none;
  border-radius: 20px;
  line-height: 24px;
  padding: 0 8px;
  background: #0099e4;
  color: #fff;
  cursor: pointer;
}

</style>
