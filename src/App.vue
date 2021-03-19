<template>
    <div id="app" class="container col-12">
        <div class="row">
            <div class="col-8 container mt-3">
                <div class="row">
                    <div>
                        <h1 class="display-1">Top 250 IMDb</h1>
                    </div>
                    <div class="ml-2">
                        <div
                            class="textHover  mb-0" v-bind:class="{ 'display-3': movieSel, 'text-warning': movieSel, 'h3': !movieSel}" v-on:click="showMovies">Movies</div>
                        <div
                            class="textHover mb-0" v-bind:class="{ 'display-3': !movieSel, 'text-warning': !movieSel, 'h3': movieSel}" v-on:click="showSeries">Series</div>
                    </div>
                </div>
                <div class="row">
                    <div id="search" class="col-12 form-outline">
                        <input v-model="search" type="search" id="searchField" class="form-control" placeholder="Search..."/>
                    </div>
                </div>
                <div id="movies">
                    <div class="row">
                        <div v-on:click="getData(item.id)" class="col-xl-3 col-lg-4 col-md-4 col-sm-6 mt-3" v-for="item in filteredList" :key="item.id">
                            <Card class="textHover" :src="item.image" :title="item.title" :year="item.year" :rating="item.imDbRating"></Card>
                        </div>
                    </div>
                </div>
            </div>
            <div class="col-3">
                <div class="position-fixed container col-3 bg-dark text-white shadow-lg h-100 overflow-auto">
                    <div class="my-4 mx-1">
                        <h1 class="text-warning">Details</h1>
                        <p v-if="!details">Click on a movie or series to get some detailed information.</p>
                        <div v-if="details">
                            <img class="w-100" :src="details.image" alt="Card image cap" loading="lazy">
                            <h5 class="text-warning mt-2">Titel</h5>
                            {{details.title}}
                            <div class="row">
                                <div class="col">
                                    <h5 class="text-warning mt-2">Runtime</h5>
                                    {{details.runtimeStr}}
                                </div>
                                <div class="col">
                                    <h5 class="text-warning mt-2">Year</h5>
                                    {{details.year}}
                                </div>
                            </div>

                            <div class="row">
                                <div class="col" v-if="details.directors">
                                    <h5 class="text-warning mt-2">Directors</h5>
                                    {{details.directors}}
                                </div>
                                <div class="col">
                                    <h5 class="text-warning mt-2">IMDb Rating</h5>
                                    {{details.imDbRating}}
                                </div>
                            </div>
                            
                            <h5 class="text-warning mt-2">Stars</h5>
                            {{details.stars}}
                            
                            <h5 class="text-warning mt-2">Plot</h5>
                            {{details.plot}}
                            <h5 class="text-warning mt-2">Awards</h5>
                            {{details.awards}}

                            <div class="mt-2">
                                <a :href="'https://www.imdb.com/title/' + details.id" class="text-warning">To IMDb page</a>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
import Card from './components/Card.vue';
export default {
    name: 'App',
    components: {
        Card
    },
    data () {
        return {
            search: '',
            movies: null,
            series: null,
            movieSel: true,
            details: null
        }
    },
    mounted () {
        axios
        .get('https://imdb-api.com/en/API/Top250Movies/' + process.env.VUE_APP_API_KEY)
        .then(response => {
            this.movies = response.data.items
            console.log(this.movies)
        })
    },
    methods: {
        getData: function(id) {
            axios
            .get('https://imdb-api.com/en/API/Title/' + process.env.VUE_APP_API_KEY + '/' + id + '/')
            .then(response => {
                console.log(response.data)
                this.details = response.data
            })
        },
        showMovies: function() {
            this.movieSel = true
        },
        showSeries: function() {
            this.movieSel = false
            console.log(this.movieSel)
            if (!this.series) {
                axios
                .get('https://imdb-api.com/en/API/Top250TVs/' + process.env.VUE_APP_API_KEY)
                .then(response => {
                    this.series = response.data.items
                    console.log(this.series)
                })
            }
        }
    },
    computed: {
        filteredList() {
            if (this.movieSel) {
                if (this.search.length >= 3) {
                    return this.movies.filter(item => {
                        return item.title.toLowerCase().includes(this.search.toLowerCase())
                    })
                } else {
                    return this.movies
                }
            } else {
                if (this.search.length >= 3) {
                    return this.series.filter(item => {
                        return item.title.toLowerCase().includes(this.search.toLowerCase())
                    })
                } else {
                    return this.series
                }
            }
            
            
        }
    }
}
</script>

<style>

.textHover:hover {
    cursor: pointer;
}

</style>
