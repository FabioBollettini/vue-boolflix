<template>
    <main>

        <!-- MOVIES -->
        <h2 v-show="movies.length > 0">MOVIES</h2>
        <section v-show="movies.length > 0" class="movies">
            <ul @click="click(); getID(movie.id)" v-for="movie in movies" :key="movie.id" class="card">

                <!-- poster img -->
                <div class="poster">
                    <!-- IMG -->
                    <img class="poster-img" v-if="movie.poster_path !== null" :src="imgURL + movie.poster_path" alt="img">
                    <img class="poster-img" v-else src="https://www.altavod.com/assets/images/poster-placeholder.png" alt="img">

                    <!-- info on hover -->
                    <div class="info">
                        <li>Titolo: {{ movie.title }}</li>
                        <!-- decision printing film or tv-show -->
                        <li>Titolo originale: {{ movie.original_title }}</li> 
                        <li>
                            <!-- flag image on language if possible -->
                            Lingua originale: 
                            <img 
                                v-if="isFlag(movie.original_language)" 
                                :src="require(`@/assets/${movie.original_language}.png`)" 
                                :alt="movie.original_language"
                                class="flag-img"
                            >
                            <span v-else>{{ movie.original_language }}</span>
                        </li>

                        <!-- vote -->
                        <li>
                            Voto:
                            <span 
                                v-for="(fullStars, i) in Math.ceil(movie.vote_average / 2)"
                                :key="i.id"
                            >
                                <i class="fas fa-star"></i>
                            </span>
                            <span 
                                v-for="(emptyStars, a) in 5 - Math.ceil(movie.vote_average / 2)"
                                :key="a.id"
                            >
                                <i class="far fa-star"></i>
                            </span>
                        </li>

                        <!-- overview -->
                        <li>
                            Trama: {{ movie.overview }}
                        </li>
                    </div>

                </div>      
            </ul>

            <!-- 
                MORE INFO
             -->
            <div v-show="isActive" class="more-info">
                <img class="poster-img" v-if="actualMovie.poster_path !== null" :src="imgURL + actualMovie.poster_path" alt="img">
                <img class="poster-img" v-else src="https://www.altavod.com/assets/images/poster-placeholder.png" alt="img">
                
               <ul>
                    <li>Titolo: {{ actualMovie.title }}</li>
                    <li>Titolo originale: {{ actualMovie.original_title }}</li>
                    <li>
                        <!-- flag image on language if possible -->
                        Lingua originale: 
                        <img 
                            v-if="isFlag(actualMovie.original_language)" 
                            :src="require(`@/assets/${actualMovie.original_language}.png`)" 
                            :alt="actualMovie.original_language"
                            class="flag-img"
                        >
                        <span v-else>{{ actualMovie.original_language }}</span>
                    </li>

                    <!-- overview -->
                    <li>
                        Trama: {{ actualMovie.overview }}
                    </li>

                    <li>
                        <a :href="ytURL + actualKey">Trailer</a>
                    </li>
               </ul>
            </div>
        </section>

        <!-- TV-SHOWS -->
        <h2 v-show="series.length > 0">TV-SHOWS</h2>
        <p class="no-results" v-show="movies.length === 0 && series.length === 0">La tua ricerca non hai ricevuto risultati.</p>
        <section v-show="series.length > 0" class="movies">
            <ul  v-for="movie in series" :key="movie.id" class="card">

                <!-- poster img -->
                <div class="poster">
                    <!-- IMG -->
                    <img class="poster-img" v-if="movie.poster_path !== null" :src="imgURL + movie.poster_path" alt="img">
                    <img class="poster-img" v-else src="https://www.altavod.com/assets/images/poster-placeholder.png" alt="img">
                    
                    <!-- info on hover -->
                    <div class="info">
                        <li>Titolo: {{ movie.name }}</li>
                        <!-- decision printing film or tv-show -->
                        <li>Titolo originale: {{ movie.original_name }}</li> 
                        <li>
                            <!-- flag image on language if possible -->
                            Lingua originale: 
                            <img 
                                v-if="isFlag(movie.original_language)" 
                                :src="require(`@/assets/${movie.original_language}.png`)" 
                                :alt="movie.original_language"
                                class="flag-img"
                            >
                            <span v-else>{{ movie.original_language }}</span>
                        </li>

                        <!-- vote -->
                        <li>
                            Voto:
                            <span 
                                v-for="(fullStars, i) in Math.ceil(movie.vote_average / 2)"
                                :key="i.id"
                            >
                                <i class="fas fa-star"></i>
                            </span>
                            <span 
                                v-for="(emptyStars, a) in 5 - Math.ceil(movie.vote_average / 2)"
                                :key="a.id"
                            >
                                <i class="far fa-star"></i>
                            </span>
                        </li>

                        <!-- overview -->
                        <li>
                            Trama: {{ movie.overview }}
                        </li>
                    </div>
                </div>
            
                
            </ul>
        </section>
    </main>
</template>

<script>
import axios from "axios";

export default {
    name: "Content",
    props: {
        movies: Array,
        series: Array,
    },
    data() {
        return {
            flagsImg: ["it", "en"],
            imgURL:"http://image.tmdb.org/t/p/w342/",
            movieURL: "https://api.themoviedb.org/3/movie/",
            ytURL: "https://www.youtube.com/watch?v=",
            actualMovie: [],
            actualKey: "",
            isActive: false,
        }
    },
    methods: {
        isFlag(lang) {
            return this.flagsImg.includes(lang);
        },
        click() {
            this.isActive = !this.isActive;
        },
        getID(id) {
            axios.get(this.movieURL + id, {
                params: {
                    api_key: "9bf8ee5302ba920d832dcf6e411abb44",
                    append_to_response: "videos",
                }
            })
            .then(res => {
                console.log(res.data.videos.results[0].key)
                this.actualMovie = res.data;
                this.actualKey = res.data.videos.results[0].key;
            })
            .catch(err => {
                console.log(err);
            });
        }
    }
}
</script>

<style scoped lang="scss">
@import "@/styles/variables.scss";
@import "@/styles/mixins.scss";

main {
    flex-grow: 1;
    background-color: $secondary-bg-col;

    h2 {
        text-align: center;
        padding: 20px;
    }

    .no-results {
        margin-top: 50px;
        text-align: center;
    }

    .movies {
        @include center("both");
        flex-direction: row;
        flex-wrap: wrap;
        position: relative;

        .card {
            width: calc(100% / 4 - 40px);
            height: 500px;
            margin: 20px 20px;
            
            .poster {
                width: 100%;
                height: 100%;
                position: relative;

                .poster-img {
                    width: 100%;
                    height: 100%;
                    object-fit: fill;
                }

                .info {
                    width: 100%;
                    height: 100%;
                    padding: 25px;
                    background-color: black;
                    position: absolute;
                    top: 0%;
                    opacity: 0;
                    overflow: auto;
                    transition: opacity 0.3s;

                    &:hover,
                    &:active {
                        opacity: 1;
                        box-shadow: 0px 0px 50px 10px dodgerblue;
                    }

                    li {
                        margin: 15px 0;
                    }
                }

            }
        }

        .more-info {
            width: 1000px;
            height: 600px;
            background-color: black;
            z-index: 5;
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            display: flex;

            .poster-img {
                @include center("both");
                height: 500px;
                padding: 30px;
            }

            ul {
                padding: 30px;

                li {
                    margin: 30px 0;
                }
            }
        }

        .flag-img {
            height: 0.8rem;
        }

    }
}

</style>