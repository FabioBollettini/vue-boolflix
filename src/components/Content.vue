<template>
    <main>

        <!-- MOVIES -->
        <section v-show="movies.length > 0" class="movies">
            <h2>MOVIES</h2>
            <ul  v-for="movie in movies" :key="movie.id">

                <!-- poster img -->
                <li><img :src="imgURL + movie.poster_path" alt="img"></li>

                <!-- decision printing film or tv-show -->
                <li>Titolo: {{ movie.title == undefined ? movie.name : movie.title }}</li>
                <li>Titolo originale: {{ movie.title == undefined ? movie.original_name : movie.original_title }}</li> 
                <li>
                    <!-- flag image on language if possible -->
                    Lingua originale: 
                    <img 
                        v-if="isFlag(movie.original_language)" 
                        :src="require(`@/assets/${movie.original_language}.png`)" 
                        :alt="movie.original_language"
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
            </ul>
        </section>

        <!-- TV-SHOWS -->
        <section v-show="series.length > 0" class="tv-shows">
            <h2>TV-SHOWS</h2>
            <ul  v-for="movie in series" :key="movie.id">

                <!-- poster img -->
                <li><img :src="imgURL + movie.poster_path" alt="img"></li>

                <!-- decision printing film or tv-show -->
                <li>Titolo: {{ movie.title == undefined ? movie.name : movie.title }}</li>
                <li>Titolo originale: {{ movie.title == undefined ? movie.original_name : movie.original_title }}</li> 
                <li>
                    <!-- flag image on language if possible -->
                    Lingua originale: 
                    <img 
                        v-if="isFlag(movie.original_language)" 
                        :src="require(`@/assets/${movie.original_language}.png`)" 
                        :alt="movie.original_language"
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
            </ul>
        </section>
    </main>
</template>

<script>
export default {
    name: "Content",
    props: {
        movies: Array,
        series: Array,
    },
    data() {
        return {
            flagsImg: ["it", "en"],
            imgURL:"http://image.tmdb.org/t/p/w500/",
        }
    },
    methods: {
        isFlag(lang) {
            return this.flagsImg.includes(lang);
        },
    }
}
</script>

<style scoped lang="scss">
@import "@/styles/variables.scss";

main {
    background-color: $secondary-bg-col;
}

</style>