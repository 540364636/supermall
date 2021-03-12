<template>
    <div id="profile">
       <nav-bar class="profile-nav-bar">
           <div slot="center">个人中心</div>
       </nav-bar>
       <profile-login-info></profile-login-info>
       <profile-business-info></profile-business-info>
       <profile-mall-list></profile-mall-list>
       <profile-shopping-list></profile-shopping-list>
    </div>
</template>

<script>
  import NavBar from "components/common/navbar/NavBar"
  import profileLoginInfo from "./childComps/ProfileLoginInfo"
  import profileBusinessInfo from "./childComps/ProfileBusinessInfo"
  import profileMallList from "./childComps/ProfileMallList"
  import profileShoppingList from "./childComps/ProfileShoppingList"

  export default {
    name:"Profile",
      components:{
        NavBar,
        profileLoginInfo,
        profileBusinessInfo,
        profileMallList,
        profileShoppingList
      }
    }
</script>

<style scoped>
  .profile-nav-bar {
    background-color: var(--color-tint);
    color: #fff;
  }
</style>