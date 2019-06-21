<template>
  <Page class="page">
    <ActionBar title="Liked Beers" />

    <StackLayout>
      <ScrollView scrollBarIndicatorVisible="false">
        <FlexboxLayout flexDirection="column">
          <SingleBeerList
                  class="SingleBeer"
                  v-for="beer in beers"
                  :key="beer.id"
                  :beer="beer" />
        </FlexboxLayout>
      </ScrollView>
    </StackLayout>

  </Page>
</template>

<script>
  const appSettings = require("tns-core-modules/application-settings")
  import SingleBeerList from '../SingleBeerList'

  export default {
    components: {
      SingleBeerList
    },
    data: () => ({
        beerResults: []
      }),
      computed: {
        beers() {
          console.log('beers computed')
          return this.beerResults
        }
      },
      methods: {
        getLikedBeers() {
          let liked = appSettings.getString('likedBeers', '[]');
          liked = JSON.parse(liked);
          let url = "https://api.punkapi.com/v2/beers?ids="+ liked.join('|') +"&per_page=10"
          console.log(url)
          fetch(encodeURI(url))
            .then(result => {
              return result.json()
            }).then( data => {
            this.beerResults = data
          })
        }
      },
      mounted() {
        this.getLikedBeers()
        this.getLikedBeers()
      }
  }
</script>

<style scoped>
  .page {
    background: #f8f8f8;
  }
</style>
