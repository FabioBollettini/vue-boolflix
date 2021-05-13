<template>
  <div id="app">

    <TopBar @search="getMovies"/>

    <!-- MOVIES -->
    <Content :movies="movieList" :series="tvList"/>

  </div>
</template>

<script>
import TopBar from './components/TopBar.vue';
import Content from './components/Content.vue';
import axios from "axios";

export default {
    name: 'App',
    components: {
        TopBar,
        Content,
    },
    data() {
        return {
            movieList: [],
            tvList: [],
            movieURL: "https://api.themoviedb.org/3/search/",
            popularURL: "https://api.themoviedb.org/3/movie/popular",
        }
    },
    created() {
        axios.get(this.popularURL, {
                params: {
                    api_key: "9bf8ee5302ba920d832dcf6e411abb44",
                    language: "it-IT",
                    page: 1,
                }
            })
            .then(res => {
                // console.log(res.data.results);
                // console.log(search);
                this.movieList = res.data.results;
            })
            .catch(err => {
                console.log(err);
            });
        
    },
    methods: {
        getMovies(search) {
            // API CALL movies
            if (this.search !== "") {
                axios.get(this.movieURL + "movie", {
                        params: {
                            api_key: "9bf8ee5302ba920d832dcf6e411abb44",
                            query: search,
                            language: "it-IT",
                        }
                    })
                    .then(res => {
                        // console.log(res.data.results);
                        // console.log(search);
                        this.movieList = res.data.results;
                    })
                    .catch(err => {
                        console.log(err);
                    });
                    
                // API CALL tv
                axios.get(this.movieURL + "tv", {
                        params: {
                            api_key: "9bf8ee5302ba920d832dcf6e411abb44",
                            query: search,
                        }
                    })
                    .then(res => {
                        // console.log(res.data.results);
                        // console.log(search);
                        this.tvList = res.data.results;
                    })
                    .catch(err => {
                        console.log(err);
                    });

            }
        }
    },
}

</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Merriweather+Sans:wght@500&display=swap');

@import "@/styles/general.scss";
@import "@/styles/mixins.scss";

</style>
