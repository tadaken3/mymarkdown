<template>
<nav class="global-nav">
    <h1>
    <img class="logo" src="../assets/logo.png" alt="MyMarkdown" title="MyMarkdown">
    </h1>
    <div class="userInfo" v-if="user">
        <span class="userName">{{user.displayName}}</span>
        <button class="logoutBtn" @click="logout">
            logout
            <i class="fa fa-sign-out"></i>
        </button>
    </div>
</nav>
</template>

<script>
export default {
    name: 'global-nav',
    props:['user'],
    methods: {
        logout: function() {
            firebase.auth().signOut()
        },
    }
}
</script>

<style lang="scss" scoped>
    .global-nav{
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 55px;
        padding: 10px 10px 0;
        box-sizing: border-box;
        text-sizing: left;
        background-color: #fff;
        border-bottom: #ccc 1px solid;
}
h1 {
    display: inline-block;
}
.userInfo{
    float: right;
}
.userName {
    margin-rigth: 20px;
}
.logo {
    height: 40px;
}
.logoutBtn {
    i {
        margin-left: 5px;
    }
}
</style>