<template>
  <div class="home">
    <!-- Movie Card -->
    <div class="feature-card">

      <!-- Routre link with IMDb id -->
      <router-link to="/movie/tt10324164">
        <!-- Poter -->
        <img class="featured-img" src="https://images.immediate.co.uk/production/volatile/sites/4/2022/05/Prehistoric-Planet-hero-55cf8a0.jpg?quality=90&webp=true&resize=940,400" alt="Prehistoric Planet Poster">

        <!-- Detais -->
        <div class="details">
          <!-- Title -->
          <h3>Prehistoric Planet</h3>
          <!-- Details -->
          <p>Travel back 66 million years to when majestic dinosaurs and extraordinary creatures roamed the lands, seas and skies.</p>
        </div>

      </router-link>

    </div>
    <!-- Search Bar -->
    <form action="" class="search-box" @submit.prevent="SearchMovies()">
      <input  type="text" placeholder="What are your looking for" v-model="search">
      <!-- Suvmit btn -->
      <input type="submit" value="Search">
    </form>

    <!-- Movies List -->
    <div class="movies-list">

      <div class="movie" v-for="movie in movies" :key="movie.imdbID">

        <router-link :to="'/movie/' + movie.imdbID" class="movie-link" > 
          <!-- Poster -->
          <div class="product-image">
            <img  :src="movie.Poster" alt="Movie Poster" />
            <!-- Type -->
            <h3 class="type">{{ movie.Type}}</h3>
          </div>  
          
          <!-- Movie Details -->
          <div class="detail">
            <!-- Year -->
            <p class="year">{{ movie.Year }}</p>
            <!-- Title -->
            <h3 class="product-title">{{ movie.Title}}</h3>
          </div>
          
        </router-link>
        
      </div>
      
    </div>
  </div>
</template>

<script>
// Going to use Coposition API
import {ref} from 'vue'
import env from '@/env.js'

export default {
  setup () {
    const search = ref('');
    const movies = ref([]);

    const SearchMovies = () => {
      // checking the search value 
      if ( search.value != '') {
        console.log(search.value)
        // clean the input field
        
        // fetch
        fetch(`https://www.omdbapi.com/?apikey=${env.apikey}&s=${search.value}`)
        .then( response => response.json() )
        .then( data => { 
          movies.value = data.Search;
          search.value = '';
        })
      }
    }

    return {
      search,
      movies,
      SearchMovies
    }

  }
}

</script>

<style lang="scss">
.home {
  .feature-card {
    position: relative;
    max-width: 900px;
    margin: 0 auto;

    .featured-img {
      display: block;
      position: relative;
      height: 300px;
      width: 100%;
      // object-fit: cover going to preserve the right aspect ratio
      object-fit: cover;
      z-index: 0;
    }

    .details {
      position: absolute;
      left: 0;
      right: 0;
      bottom: 0;
      // overlay
      background-color: rgba(0, 0, 0, 0.6);
      padding: 16px;
      z-index: 1;

      h3 {
        color: #FFF;
        margin-bottom: 16px;
      }

      p {
        color: #FFF;
      }
    }
  }

  .search-box {
  max-width: 900px;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 16px;

    input {
      display: block;
      appearance: none;
      outline: none;
      border: none;
      background: none;

      &[type="text"] {
        width: 100%;
        color: #FFF;
        background-color: #496583;
        padding: 10px 16px;
        text-align: center;
        font-size: 20px;
        border-radius: 8px;
        margin-bottom: 15px;
        transition: 1.4s;

        &::placeholder {
          color: #f3f3f3;
        }
        &:focus {
          box-shadow: 0px 3px 6px rgba(0, 0, 0, 0.2);
        }
      }

      &[type="submit"] {
        width: 100%;
        max-width: 300px;
        // vue green
        background-color: #42b883;
        padding: 16px;
        border-radius: 8px;
        font-size: 20px;
        color: #FFF;
        text-transform: uppercase;
        transition: 0,4s;

        &:active {
          background-color: #3b8070;
        }
      }
    }  
  }

  .movies-list {
    display: flex;
    align-items: center;
    justify-content:space-around;
    flex-wrap: wrap;
    margin: 0px 8px;

    .movie {
      max-width: 50%;
      flex: 1 1 50%;
      padding: 16px 8px;
      @media (min-width: 960px) {
        
        max-width: 30%;
        flex: 1 1 30%;
        padding: 16px 8px;
    
      }

      .movie-link {
        display: flex;
        flex-direction: column;
        height: 100%;

        .product-image {
          position: relative;
          display: block;

          img {
            display: block;
            width: 100%;
            height: 275px;
            object-fit: cover;
          }

          .type {
            position: absolute;
            padding: 8px 16px;
            background-color: #42b883;
            color: #FFF;
            bottom: 16px;
            left: 0px;
            text-transform: capitalize;
          }
        }

        .detail {
          background-color: #496583;
          padding: 16px 8px;
          flex: 1 1 100%;
          border-radius: 0px 0px 8px 8px;

          .year {
            color: #aaa;
            font-size: 14px;
          }

          .product-title {
            color: #FFF;
            font-weight: 600;
            font-size: 18px;
          }

        }

      }     
    }
  }
  
}


</style>
