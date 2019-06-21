<template>
    <Page class="page" actionBarHidden="true">
        <GridLayout orientation="vertical" width="100%" height="100%" columns="*" rows="*,auto">
            <StackLayout col="0" row="0">
                <ScrollView scrollBarIndicatorVisible="false">
                    <FlexboxLayout flexDirection="column" flexWrap="wrap">
                        <Label
                                text="Beers with highest ABV"
                                class="SectionTitle"
                                marginTop="30"
                                marginBottom="15"
                                marginLeft="20" />
                        <ScrollView row="1"
                                    orientation="horizontal"
                                    scrollBarIndicatorVisible="false"
                                    class="HighABVSection" >
                            <StackLayout orientation="horizontal">
                                <SingleBeerCard
                                        class="SingleBeerCard"
                                        v-for="beer in highABVResults"
                                        :key="beer.id"
                                        :beer="beer"
                                        marginLeft="20" />
                            </StackLayout>
                        </ScrollView>
                        <Label text="All beers"
                               class="SectionTitle"
                               marginTop="30"
                               marginBottom="15"
                               marginLeft="20" />
                        <SingleBeerList
                                class="SingleBeer"
                                v-for="beer in beerResults"
                                :key="beer.id"
                                :beer="beer" />
                        <StackLayout>
                            <ActivityIndicator :busy="loading" />
                            <Button text="Load More" v-if="!loading" class="LoadMoreButton" @tap="LoadMore" />
                        </StackLayout>
                    </FlexboxLayout>
                </ScrollView>
            </StackLayout>
            <StackLayout col="0" row="1"
                         orientation="horizontal"
                         backgroundColor="#FFFFFF"
                         borderTopWidth="1"
                         borderColor="#EEEEEE"
                         height="60">
                <Dock active="home" />
            </StackLayout>
        </GridLayout>
    </Page>
</template>

<script>
    import SingleBeerList from './SingleBeerList'
    import SingleBeerCard from "./SingleBeerCard"
    import Dock from './Dock'

    export default {
        data: () => ({
            highABVResults: [],
            beerResults: [],
            page: 1,
            loading: false,
            time: new Date()
        }),
        components: {
            Dock,
            SingleBeerCard,
            SingleBeerList
        },
        computed: {
          showBeersCount() {
              return this.highABVResults.length
          }
        },
        methods: {
            LoadMore() {
                this.loading = true;
                this.page++;
                fetch("https://api.punkapi.com/v2/beers?page="+ this.page +"&per_page=10").then(result => {
                    return result.json()
                }).then( data => {
                    this.beerResults = this.beerResults.concat(data)
                    this.loading = false;
                })
            }
        },
        mounted() {
            fetch("https://api.punkapi.com/v2/beers?abv_gt=15&per_page=8").then(result => {
                return result.json()
            }).then( data => {
                this.highABVResults = data.sort((a, b) => b.abv - a.abv);
            }).then(() => {
                fetch("https://api.punkapi.com/v2/beers?page=1&per_page=10").then(result => {
                    return result.json()
                }).then( data => {
                    this.beerResults = data
                })
            });
            this.time = new Date();
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
        background: #f8f8f8;
    }

    .SectionTitle {
        font-family: 'Popins-Black', sans-serif;
        font-weight: 800;
        padding: 8% 0;
    }

    .LoadMoreButton {
        background: #fdfdfd;
        margin: 20px 5%;
        padding-top: 10;
        padding-bottom: 10;
        color: #aaaaaa;
    }

    .info {
        font-size: 20;
    }
</style>
