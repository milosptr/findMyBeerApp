<template>
  <Page class="page">
    <ActionBar title="Account">
        <ActionItem @tap="onDone"
                    ios.systemIcon="0" ios.position="right"
                    text="Done" android.position="right" />
    </ActionBar>


      <StackLayout>
        <ScrollView scrollBarIndicatorVisible="false" marginTop="30">
          <StackLayout>
            <StackLayout class="text-left">
              <Button text="Liked beers" @tap="goToLikedBeersPage" class="Button" />
              <Button text="My profile" @tap="goToMyProfilePage" class="Button" />
              <Button text="Logout" @tap="logout" class="Button" marginTop="30" />
            </StackLayout>
          </StackLayout>
        </ScrollView>
      </StackLayout>
  </Page>
</template>

<script>
  const appSettings = require("tns-core-modules/application-settings")

  import Home from './Home'
  import Dock from './Dock'
  import Login from './Login'
  import LikedBeers from './account/LikedBeers'
  import MyProfile from './account/Profile'

  import {navigationSettings} from '../shared/config'

  export default {
    components: {
      Dock
    },
    data: () => ({}),
    computed: {
    },
    methods: {
      goToLikedBeersPage() {
        this.$navigateTo(LikedBeers)
      },
      goToMyProfilePage() {
        this.$navigateTo(MyProfile)
      },
      logout() {
        appSettings.setBoolean('loginStatus', false);
        this.$navigateTo(Login, navigationSettings)
      },
      onDone() {
        this.$navigateTo(Home, navigationSettings)
      }
    }
  }
</script>

<style scoped>
  .page {
    background: #f8f8f8;
  }

  .Button {
    background: #fff;
    padding-top: 10;
    padding-bottom: 10;
    padding-left: 20;
    padding-right: 20;
    font-size: 16;
    border-width: 1px;
    border-color: #eee;
  }

  .SectionTitle {
    font-family: 'Popins-Black', sans-serif;
    font-weight: 800;
    padding: 8% 0;
  }
</style>
