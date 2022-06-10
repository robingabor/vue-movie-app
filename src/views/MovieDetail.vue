<template>
    <div class="movie-detail">
        <h2>{{movie.Title }}</h2>
        <p>{{ movie.Year }}</p>
        <img :src="movie.Poster" alt="Movie Poster" class="featured-img" />
        <p>{{ movie.Plot  }}</p>
    </div>
    
</template>

<script>
import { ref, onBeforeMount } from 'vue'
import { useRoute } from 'vue-router'
import env from '@/env.js'

export default {
    setup () {
        const route = useRoute();
        const movie = ref({});

        onBeforeMount( () => {
            fetch(`https://www.omdbapi.com/?apikey=${env.apikey}&i=${route.params.id}&plot=full`)
                .then( response => response.json() )
                .then( data => { 
                    movie.value = data;
                    console.log(data)
                })
        }) 

        return {
            route,
            movie,
            onBeforeMount
        }
    }
}
</script>

<style lang="scss">
.movie-detail {
    padding: 16px;
    max-width: 900px;
    margin: 0 auto;   
    
    h2 {
        color: #FFF;
        font-size: 28px;
        font-weight: 600px;
        margin-bottom: 16px;
    }

    .featured-img {
        display: block;
        max-width: 200px;        
        margin-bottom: 16px;
    }

    p {
        color: #FFF;
        font-size: 18px;
        line-height: 1.5;
    }

}
</style>