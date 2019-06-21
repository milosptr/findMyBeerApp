<template>
  <StackLayout>
    <GridLayout rows="*" columns="*,*,*">
      <Image col="0" row="0" @tap="goToHome()" height="25"
             width="25" margin="10" :src="homeIcon"/>
      <Image col="1" row="0" @tap="goToSearch()" height="25"
             width="25" margin="10" :src="searchIcon"/>
      <Image col="2" row="0" @tap="goToProfile()" height="60"
             width="25" margin="10" :src="profileIcon"/>
    </GridLayout>
  </StackLayout>
</template>


<script>
  import Home from './Home'
  import Login from './Login'
  import Profile from './Profile'
  import Search from './Search'

  const appSettings = require("tns-core-modules/application-settings")
  const navigationSettings = {
    transition: {
      name: "fade",
      duration: 300,
      curve: "easeIn"
    },
    transitioniOS: {
      name: "fade",
      duration: 300,
      curve: "easeIn"
    },
    transitionAndroid: {
      name: "fade",
      duration: 300,
      curve: "easeIn"
    },
    clearHistory: true
  }



  export default {
    props: {
      active: {
        type: String,
        default: 'home'
      }
    },
    data: () => ({
      loginStatus: false,
    }),
    computed: {
      homeIcon() {
        if (this.active === 'home')
          return '~/images/icons/home-active.png'
        return '~/images/icons/home.png'
      },
      searchIcon() {
        if (this.active === 'search')
          return '~/images/icons/search-active.png';
        return '~/images/icons/search.png'
      },
      profileIcon() {
        if (this.active === 'profile')
          return '~/images/icons/user-active.png';
        return '~/images/icons/user.png';
      }
    },
    methods: {
      goToHome() {
        console.log('Clicked')
        if (this.active !== 'home') this.$navigateTo(Home, navigationSettings)
      },
      goToSearch() {
        console.log('Clicked')
        if (this.active !== 'search') this.$navigateTo(Search, navigationSettings)
      },
      goToProfile() {
        console.log('Clicked')
        if (this.loginStatus) {
          this.$navigateTo(Profile, navigationSettings)
        } else {
          this.$navigateTo(Login, navigationSettings)
        }
      },
    },
    mounted() {
      this.loginStatus = appSettings.getBoolean('loginStatus', false);
    }
  }
</script>
