<template>
  <Page class="page">
    <ActionBar title="Search">
      <ActionItem @tap="onDone"
                  ios.systemIcon="0" ios.position="right"
                  text="Done" android.position="right" />
    </ActionBar>

    <StackLayout>
      <ScrollView scrollBarIndicatorVisible="false">
        <StackLayout>
          <SearchBar id="searchBar" hint="Search for your beer..." v-model="searchQuery" :clear="onClear" :submit="searchForBeer" />
          <ActivityIndicator :busy="loading" />
          <ListView for="beer in beerResults" v-if="!loading">
            <v-template>
              <Label :text="beer.name" class="SearchResults" @tap="goToBeerPage(beer.id)" />
            </v-template>
          </ListView>
        </StackLayout>
      </ScrollView>
    </StackLayout>
  </Page>
</template>

<script>
  import Dock from './Dock'
  import Home from './Home'
  import BeerPage from './BeerPage'

  import {navigationSettings} from '../shared/config'

  export default {
    components: {
      Dock
    },
    data: () => ({
      beerResults: [],
      searchQuery: '',
      loading: false,
    }),
    computed: {
      searchForBeer() {
        if (this.searchQuery !== ''){
          this.loading = true;
          console.log(this.searchQuery, ' PRETRAGA')
          fetch("https://api.punkapi.com/v2/beers?beer_name="+ encodeURIComponent(this.searchQuery.trim()) +"&per_page=10").then(result => {
            return result.json()
          }).then( data => {
            this.beerResults = data;
            this.loading = false;
          })
        }
      }
    },
    methods: {
      onClear() {

      },
      onSubmit() {

      },
      onDone() {
        this.$navigateTo(Home, navigationSettings)
      },
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

<style>
  .SectionTitle {
    font-family: 'Popins-Black', sans-serif;
    font-weight: 800;
    padding: 8% 20;
  }

  .SearchResults {
    font-family: "Popins-Regular", sans-serif;
    font-size: 18;
    font-weight: 400;
    font-size: 16;
    padding: 10 20;
  }
</style>
