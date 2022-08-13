<template>
  <div>
    <LoadingES v-if="$fetchState.pending" />
    <div v-else class="container single-movie">
      <NuxtLink class="button" :to="{ name: 'index' }">Back</NuxtLink>
      <div class="movie-info">
        <div class="movie-img">
          <img
            :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`"
            alt=""
          />
        </div>
        <div class="movie-content">
          <h1>Title: {{ movie.title }}</h1>
          <a href="https://lookmovie.la/watch-movie/taken-2008-full-movie-123-free-online-wjbazmn/35w7o4kk4q?WatchNow=1">watch</a>
          <p class="movie-fact tagline">
            <span>Tagline:</span> "{{ movie.tagline }}"
          </p>
          <p class="movie-fact">
            <span>Released:</span>
            {{
              new Date(movie.release_date).toLocaleString('en-us', {
                month: 'long',
                day: 'numeric',
                year: 'numeric',
              })
            }}
          </p>
          <p class="movie-fact">
            <span>Duration:</span> {{ movie.runtime }} minutes
          </p>
          <p class="movie-fact">
            <span>Revenue:</span>
            {{
              movie.revenue.toLocaleString('en-us', {
                style: 'currency',
                currency: 'USD',
              })
            }}
          </p>
          <p class="movie-fact"><span>Overview:</span> {{ movie.overview }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  head() {
    return {
      title: this.movie.title,
      meta: [
        {
          hid: 'description',
          name: 'description',
          content:  this.movie.overview
        }
      ]
    }
  },
  data() {
    return {
      movie: '',
    }
  },
  async fetch() {
    await this.getSingleMovie()
  },
  methods: {
    async getSingleMovie() {
      const data = axios.get(
        `https://api.themoviedb.org/3/movie/${this.$route.params.id}?api_key=4a7f07ee852309cd38576cb75989d50a&language=en-US`
      )
      const result = await data
      this.movie = result.data
      const videos = axios.get(
        `https://api.themoviedb.org/3/movie/${this.$route.params.id}/watch/providers?api_key=4a7f07ee852309cd38576cb75989d50a&language=en-US`
      )
      const videosResult = await videos
      console.log('videosResult', videosResult.data);

      // console.log('this.$route.params.id', this.$route.params.id);
    },

  },
  fetchDelay: 1000,
}
</script>

<style lang="scss">
.single-movie {
  color: #fff;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 32px 16px;
  .button {
    align-self: flex-start;
    margin-bottom: 32px;
  }
  .movie-info {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 32px;
    color: #fff;
    @media (min-width: 800px) {
      flex-direction: row;
      align-items: flex-start;
    }
    .movie-img {
      img {
        max-height: 500px;
        width: 100%;
        @media (min-width: 800px) {
          max-height: 700px;
          width: initial;
        }
      }
    }
    .movie-content {
      h1 {
        font-size: 56px;
        font-weight: 400;
      }
      .movie-fact {
        margin-top: 12px;
        font-size: 20px;
        line-height: 1.5;
        span {
          font-weight: 600;
          text-decoration: underline;
        }
      }
      .tagline {
        font-style: italic;
        span {
          font-style: normal;
        }
      }
    }
  }
}
</style>