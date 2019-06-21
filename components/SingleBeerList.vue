<template>
  <FlexboxLayout class="SingleBeer" flexDirection="row" alignItems="center" @tap="goToBeerPage(beer.id)">
    <FlexboxLayout class="SingleBeerImg text-center" flexDirection="column" alignContent="center" justifyContent="center">
      <StackLayout orientation="vertical" paddingTop="10" paddingBottom="10" >
        <Image :src="beer.image_url"  height="80" width="20" stretch="fill" />
      </StackLayout>
    </FlexboxLayout>
    <StackLayout orientation="vertical" class="SingleBeerText">
      <Label :text="beer.name" class="SingleBeerName" textWrap="true" />
      <Label :text="beerDescription" class="beerDescription" textWrap="true" />
    </StackLayout>
  </FlexboxLayout>
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
    data: () => ({

    }),
      computed: {
        htmlImgTag() {
          return "<img src='" + this.beer.image_url + "' height='70' width='auto' style='margin: 0 auto' />";
        },
        beerDescription() {
          return this.beer.description.slice(0,50) + '...'
        }
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
  };
</script>

<style scoped lang="scss">
  // Start custom common variables
  @import '../app-variables';
  // End custom common variable

  .SingleBeer {
    margin: 20px 5%;
    background: #fff;
    border-radius: 5%;
    padding-right: 15%;
    width: 100%;
  }

  .SingleBeerText {
    font-size: 12px;
    color: #666;
    width: 80%;
  }

  .SingleBeerName {
    font-size: 18px;
    font-weight: 800;
    font-family: 'Popins-Black', sans-serif;
    line-height: 1em;
    color: #212121;
  }

  .beerDescription {
    width: 100%;
    margin-top: 5%;
    word-break: break-all
  }

  .SingleBeerImg {
    width: 20%;
  }

  .SingleBeerImage {
    margin: 0 auto;
  }
</style>
