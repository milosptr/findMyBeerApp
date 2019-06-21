<template>
    <AbsoluteLayout class="SingleBeerCard">
      <Label
          :text="abvText"
          left="100" top="5"
          class="ABVText text-center" />
      <FlexboxLayout
              flexDirection="column"
              class="text-center"
              orientation="vertical"
              left="0"
              top="15"
              width="135"
              @tap="goToBeerPage(beer.id)">
        <StackLayout orientation="vertical" marginBottom="15">
          <Image :src="beer.image_url"  height="120" width="35" stretch="fill" />
        </StackLayout>
        <Label :text="beerName" class="SingleBeerName" textWrap="true" />
      </FlexboxLayout>
    </AbsoluteLayout>

</template>

<script>
  import BeerPage from './BeerPage'

  export default {
    props: {
      beer: {
        type: Object,
        default: {}
      }
    },
    data: () => ({}),
    computed: {
      abvText() {
        return this.beer.abv + '%'
      },
      beerName() {
        return this.beer.name.slice(0,20)
      },
    },
    methods: {
      goToBeerPage(id) {
        this.$navigateTo(BeerPage, {
          props: {
            beerId: id
          }
        })
      }
    }
  }
</script>

<style scoped lang="scss">
  // Start custom common variables
  @import '../app-variables';

  .SingleBeerCard {
    background: #fff;
    border-radius: 5%;
    width: 150;
    height: 650px;
    margin-left: 5%;
    margin-right: 5%;
  }

  .ABVText {
    background: #ac341a;
    padding: 2 4;
    font-size: 10;
    font-weight: bold;
    color: #fff;
    border-radius: 50%;
  }

  .SingleBeerName {
    font-family: "Popins-Black", sans-serif;
    font-weight: 800;
    font-size: 16px;
  }
</style>
