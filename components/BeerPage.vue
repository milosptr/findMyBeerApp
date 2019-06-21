<template>
  <Page class="page" actionBarHidden="true" backgroundSpanUnderStatusBar="true">

    <ScrollView scrollBarIndicatorVisible="false">
      <StackLayout>
        <FlexboxLayout flexDirection="column">
          <StackLayout class="FeatureImageSection" paddingTop="20" paddingBottom="20">
            <Image :src="beer.image_url" class="FeatureImage" />
          </StackLayout>
          <FlexboxLayout justifyContent="flex-end">
            <FlexboxLayout class="LikeHolder" alignItems="center" justifyContent="center" @tap="likeHandler">
              <Image v-if="!liked" src="~/images/icons/like.png" width="32" height="32" stretch="fill" />
              <Image v-if="liked" src="~/images/icons/liked.png" width="32" height="32" stretch="fill" />
            </FlexboxLayout>
          </FlexboxLayout>
          <StackLayout marginLeft="20" marginRight="20">
            <Label :text="beer.tagline" class="BeerTagline" />
            <Label :text="beer.name" class="BeerTitle" />
            <FlexboxLayout flexDirection="row" marginTop="10">
              <Image src="~/images/icons/percentage.png" marginRight="5" width="16" height="16" stretch="fill" />
              <Label :text="abvText" class="BeerInfoText" />
              <Image src="~/images/icons/calendar.png"
                     marginRight="5"
                     marginLeft="15"
                     width="16"
                     height="16"
                     stretch="fill" />
              <Label :text="brewedText" class="BeerInfoText" />
            </FlexboxLayout>
            <StackLayout marginTop="30">
              <Label text="Description" class="BeerSecondTitle" />
              <Label :text="beer.description" textWrap="true" class="BeerInfoText" marginTop="5" />
            </StackLayout>
            <StackLayout marginTop="30">
              <Label text="Food Pairing" class="BeerSecondTitle" marginBottom="5" />
              <Label
                      v-for="(pairing, index) in beer.food_pairing"
                      :key="index"
                      :text="pairing"
                      textWrap="true"
                      class="BeerInfoText" />
            </StackLayout>
          </StackLayout>
        </FlexboxLayout>
        <FlexboxLayout flexDirection="column" class="WhiteBackground">
          <Label text="Brewers Tips"
                 class="BeerSecondTitle"
                 marginTop="20"
                 marginLeft="20"
                 marginRight="20"/>
          <FlexboxLayout marginLeft="20" marginRight="20" marginTop="5"  marginBottom="20">
            <Image src="~/images/icons/beer.png" width="50" height="50" />
            <Label :text="beer.brewers_tips"
                   textWrap="true"
                   class="BeerInfoText"
                   marginLeft="15"
                   marginRight="20"
            />
          </FlexboxLayout>
        </FlexboxLayout>
        <FlexboxLayout
          flexDirection="row"
          marginLeft="20"
          marginRight="20"
          marginTop="30"
          marginBottom="30"
          >
          <FlexboxLayout
                  flexDirection="column"
                  alignItems="flex-start"
                  class="BeerVolume"
                  v-if="hasBoilVolume">
            <Label text="Boil Volume" class="BeerSecondTitle" />
            <StackLayout>
              <Image src="~/images/icons/boil-volume.png"
                     width="40"
                     marginTop="10"
                     marginBottom="5"
              />
            </StackLayout>
            <Label :text="boilVolumeText"
                   class="BeerInfoText"
                   marginTop="5" />
          </FlexboxLayout>
          <FlexboxLayout
                  flexDirection="column"
                  alignItems="flex-start"
                  class="BeerVolume"
                  v-if="hasVolume">
            <Label text="Volume" class="BeerSecondTitle" />
            <StackLayout>
              <Image src="~/images/icons/volume.png"
                     width="40"
                     marginTop="10"
                     marginBottom="5"
              />
            </StackLayout>
            <Label :text="volumeText" class="BeerInfoText" marginTop="5" />
          </FlexboxLayout>
        </FlexboxLayout>
      </StackLayout>
    </ScrollView>
  </Page>
</template>

<script>
  const appSettings = require("tns-core-modules/application-settings");

  export default {
    props: {
      beerId: {
        type: Number,
        default: 0
      }
    },
    data: () => ({
      beer: {},
      liked: false
    }),
    computed: {
      beerTitle() {
        return this.beer.name
      },
      abvText() {
        return "ABV: " + this.beer.abv + "%"
      },
      brewedText() {
        return "Brewed: " + this.beer.first_brewed
      },
      hasVolume() {
        return this.beer.volume
      },
      hasBoilVolume() {
        return this.beer.boil_volume
      },
      volumeText() {
        return this.beer.volume.value + ' ' + this.beer.volume.unit
      },
      boilVolumeText() {
        return this.beer.boil_volume.value + ' ' + this.beer.boil_volume.unit
      },
    },
    methods: {
      likeHandler() {
        let liked = appSettings.getString('likedBeers', '[]');
        liked = JSON.parse(liked);

        !this.liked ? this.like(liked) : this.unlike(liked);
      },
      like(liked) {
        if(liked.indexOf(this.beerId) === -1)
          liked.push(this.beerId);

        let likedString = JSON.stringify(liked);

        appSettings.setString('likedBeers', likedString);
        this.liked = true;
        console.log('AFTER', likedString, 'AFTER');
      },
      unlike(liked) {
        let index = liked.indexOf(this.beerId);
        if(index !== -1)
          liked.splice(index, 1);

        let likedString = JSON.stringify(liked);

        appSettings.setString('likedBeers', likedString);
        this.liked = false;
        console.log('AFTER', likedString, 'AFTER');
      }
    },
    mounted() {
      fetch("https://api.punkapi.com/v2/beers/" + this.beerId).then(result => {
        return result.json()
      }).then( data => {
        this.beer = data[0]
      });

      let likedString = appSettings.getString('likedBeers', '[]')
      if(JSON.parse(likedString).indexOf(this.beerId) !== -1)
        this.liked = true;
    }
  };
</script>

<style scoped lang="scss">
  // Start custom common variables
  @import '../app-variables';
  // End custom common variables

  .page {
    background: #f8f8f8;
  }

  .action-bar {
    background: #5F374F;
  }

  .LikeHolder {
    background: #fdfdfd;
    border-color: #f1f1f1;
    border-width: 1px;
    border-radius: 50%;
    width: 60;
    height: 60;
    text-align: center;
    margin-top: -30;
    margin-right: 20;
  }

  .FeatureImageSection {
    background: #fff;
    text-align: center;
    width: 100%;
  }

  .FeatureImage {
    height: 300;
    margin: 0 auto;
  }

  .BeerTagline {
    font-family: "Popins-SemiBold", sans-serif;
    font-weight: 500;
    font-size: 12;
  }

  .BeerTitle {
    font-family: "Popins-Black", sans-serif;
    font-size: 24;
    font-weight: 700;
  }

  .BeerSecondTitle {
    font-family: "Popins-Black", sans-serif;
    font-size: 18;
    font-weight: 600;
  }

  .BeerInfoText {
    color: #aaaaaa;
    font-size: 14;
  }

  .WhiteBackground {
    background: #fff;
    margin-top: 30;
  }

  .BeerVolume {
    width: 50%;
  }
</style>
