<template>
  <div class="home">
    <Hero />

    <div class="container search">
      <input v-model.lazy="searchInput" type="text" placeholder="Search" @keyup.enter="$fetch" />
      <button v-show="searchInput !== ''" class="button" @click="clearSearch">Clear Search</button>
    </div>

    <div class="container movies">
      <div  v-if="searchInput !== ''" id="movie-grid" class="movies-grid">
        <div v-for="(movie, index) in searchedMovies" :key="index" class="movie">
          <div class="movie-img">
            <img :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`" aria-label="Movie image" />
            <p class="review">{{ movie.vote_average }}</p>
            <p class="overview">{{ movie.overview }}</p>
          </div>
          <div class="info">
            <p class="title">
              {{ movie.title.slice(0, 25)
              }}<span v-if="movie.title.length > 25">...</span>
            </p>
            <p class="release">
              Released:
              {{
                new Date(movie.release_date).toLocaleString('pt-br', {
                  month: 'long',
                  day: 'numeric',
                  year: 'numeric',
                })
              }}
            </p>
             <NuxtLink
              class="button button-light"
              :to="{ name: 'movies-movieid', params: { movieid: movie.id } }"
            >
              Get More Info
            </NuxtLink>
          </div>
        </div>
      </div>

      <div v-else id="movie-grid" class="movies-grid">
        <div v-for="(movie, index) in movies" :key="index" class="movie">
          <div class="movie-img">
            <img :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`" aria-label="Movie image" />
            <p class="review">{{ movie.vote_average }}</p>
            <p class="overview">{{ movie.overview }}</p>
          </div>
          <div class="info">
            <p class="title">
              {{ movie.title.slice(0, 25)
              }}<span v-if="movie.title.length > 25">...</span>
            </p>
            <p class="release">
              Released:
              {{
                new Date(movie.release_date).toLocaleString('pt-br', {
                  month: 'long',
                  day: 'numeric',
                  year: 'numeric',
                })
              }}
            </p>
             <NuxtLink
              class="button button-light"
              :to="{ name: 'movies-movieid', params: { movieid: movie.id } }"
            >
              Get More Info
            </NuxtLink>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import axios from 'axios'
import Hero from '~/components/Hero/Hero.vue'

export default Vue.extend({
  name: "IndexPage",
  components: { Hero },

  data() {
    return {
      movies: [],
      searchedMovies: [],
      searchInput: '',
    }
  },

  async fetch() {
    if(this.searchInput === '') {
      await this.getMovies()
      return
    }

    await this.searchMovies()
  },
  
  methods: {
    async getMovies() {
      const data = axios.get(
        `https://api.themoviedb.org/3/movie/now_playing?api_key=${process.env.API_KEY_MOVIEDB}&language=pt-BR&page=1`
      )

      const result = await data
      result.data.results.forEach(movie => {
        this.movies.push(movie)
      })
    },

    async searchMovies() {
      const data = axios.get(
        `https://api.themoviedb.org/3/search/movie?api_key=3b90a539021ff9e088bebcec85f41077&language=pt-BR&page=1&query=${this.searchInput}`
      )

      const result = await data
      result.data.results.forEach((movie) => {
        this.searchedMovies.push(movie)
      })
    },

     clearSearch() {
      this.searchInput = ''
      this.searchedMovies = []
    },
  }
})
</script>

<style src="./index.scss" lang="scss" scoped />
