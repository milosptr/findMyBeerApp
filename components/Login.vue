<template>
  <Page class="page">
    <FlexboxLayout flexDirection="column" justifyContent="center">
      <StackLayout orientation="vertical">
        <Image src="~/images/find-my-beer-logo.png" class="LogoImage" height="250" width="250" stretch="fill" />
      </StackLayout>
      <StackLayout orientation="vertical" class="LoginSection">
        <Label text="Login" class="LoginText" />
        <Label text="Username or password are wrong" v-if="showErrors" class="text-danger" />
        <TextField hint="Email"
                   v-model="email"
                   secure="false"
                   returnKeyType="next"
                   keyboardType="email"
                   autocorrect="false"
                   maxLength="30"
                   class="input input-border text-center" />
        <TextField hint="Password"
                   v-model="password"
                   secure="true"
                   returnKeyType="login"
                   keyboardType="email"
                   autocorrect="false"
                   maxLength="30"
                   class="input input-border text-center" />
        <Button @tap="login" text="Login" class="LoginButton" />
      </StackLayout>
      <Button @tap="guestMode" text="Proceed as Guest" />
    </FlexboxLayout>
  </Page>
</template>

<script>
  import Home from './Home'

  const appSettings = require("tns-core-modules/application-settings");

  export default {
    data: () => ({
      email: '',
      password: '',
      showErrors: false,
    }),
    computed: {

    },
    methods: {
      login() {
        if (this.email === 'admin' && this.password === 'password') {
          appSettings.setBoolean("loginStatus", true);
          this.goToHome()
        } else {
          this.showErrors = true
        }
      },
      guestMode() {
        appSettings.setBoolean("loginStatus", false);
        this.goToHome()
      },
      goToHome() {
        this.$navigateTo(Home)
      },
    },
    mounted() {
    }
  };
</script>

<style scoped lang="scss">
  // Start custom common variables
  @import '../app-variables';

  .LoginSection {
    margin-top: 10%;
    margin-bottom: 10%;
  }

  .LogoImage {
    flex: 300px;
    height: 300px;
    width: 300px;
  }

  .LoginText {
    font-family: 'Popins-Black', sans-serif;
    font-weight: 800;
    font-size: 24px;
    color: #212121;
    margin-left: 20%;
    margin-bottom: 36px;
  }

  .text-danger {
    font-size: 12px;
    margin-left: 20%;
  }

  .LoginButton {
    width: 60%;
    padding: 24px 0;
    text-align: center;
    color: #fff;
    background: #5F374F;
    border-radius: 5%;
    margin-top: 40px;
  }

  .input {
    width: 60%;
    padding: 16px 0;
    border-bottom-width: 2px;
    border-bottom-color: #666;
    text-align: left;
    margin-bottom: 40px;
  }

</style>
