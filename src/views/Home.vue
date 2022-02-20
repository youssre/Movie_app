<template>
  <div class="home">
    <div class="feature-card">
      <router-link to="/movie/tt0409591">
        <img src="https://m.media-amazon.com/images/M/MV5BZmQ5NGFiNWEtMmMyMC00MDdiLTg4YjktOGY5Yzc2MDUxMTE1XkEyXkFqcGdeQXVyNTA4NzY1MzY@._V1_.jpg" alt="naruto" class="featured-img">
        <div class="detail">
          <h3>Naruto</h3>
          <p>
            Many years ago, in the hidden village of Konoha, lived a great demon
             fox. When it swung one of it's nine tails, a tsunami occurred. The
              fourth hokage sealed this demon fox inside a boy in exchange for 
              his own life. Naruto was that boy, and he grew up with no family,
               and the villagers hated him thinking that he himself was the demon
                fox. Naruto's dream is to become Hokage, and have the villagers
                 acknowledge him.
          </p>
        </div>
      </router-link>
    </div>

    <form @submit.prevent="searchMovies()" class="search-box">
      <input type="text" placeholder="type your search here!" v-model="search">
      <input type="submit" value="Search">
    </form>

    <div class="movies-list">
      <div class="movie" v-for="movie in movies" :key="movie.imdbID">
        <router-link :to="'/movie/' + movie.imdbID" class="movie-link">
          <div class="product-img">
            <img :src="movie.Poster" alt="Movie Poster">
            <div class="type">{{movie.Type}}</div>
          </div>
          <div class="detail">
            <p class="year">{{movie.Year}}</p>
            <h3>{{movie.Title}}</h3>
          </div>
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
import {ref} from 'vue'
import env from '../env'
export default {
  setup(){
    const search = ref('');
    const movies = ref([])

    const searchMovies = ()=>{
      if(search.value != ''){
        fetch(`http://www.omdbapi.com/?apikey=${env.apikey}&s=${search.value}`)
          .then(res => res.json())
          .then(data=>{
            movies.value = data.Search;
            search.value = ''
            console.log(movies.value);
          })
      }
    }
    return{
      search,
      movies,
      searchMovies
    }
  }
}
</script>
<style lang="scss">
  .feature-card{
    position: relative;

    .featured-img{
      display: block;
      width: 100%;
      height: 300px;
      object-fit: cover;

      position: relative;
      z-index: 0;
    }
    .detail{
      position: absolute;
      left: 0;
      right: 0;
      bottom: 0;
      background-color: rgba(0,0,0,0.6);
      padding: 16px;
      z-index: 1;

      h3{
        color: white;
        margin-bottom: 16px;
      }
      p{
        color: white;
      }
    }
  }

  .search-box{
    display: flex;
    flex-direction: column;
    justify-content: center;
    padding: 16px;
    align-items: center;

    input{
      display: block;
      appearance: none;
      border: none;
      background: none;
      outline: none;

      &[type = 'text']{
        width: 100%;
        color: #FFF;
        background-color: #496583;
        font-size: 20px;
        padding: 10px 16px;
        border-radius: 8px;
        margin-bottom: 15px;
        transition: 0.4s;

        &::placeholder{
          color: #f3f3f3;
        }
        &:focus{
          box-shadow: 0px 3px 6px rgba(0,0,0,0.2);
        }
      }
      &[type = 'submit']{
          width: 100%;
          max-width: 300px;
          background-color: #42B883;
          padding: 16px;
          border-radius: 8px;
          color: #fff;
          font-size: 20px;
          text-transform: uppercase;
          transition: 0.4s;
        }
        &:active{
          background-color: #3B8070;
        }
    }
  }
  .movies-list{
      display: flex;
      flex-wrap: wrap;
      margin: 0px 8px;

      .movie{
        max-width: 50%;
        flex: 1 1 50%;
        padding: 16px 8px;

        @media only screen and (min-width: 800px) {
          max-width: 33.33%;
        }
        @media only screen and (max-width: 400px) {
          min-width: 100%;
        }
        .movie-link{
          display: flex;
          flex-direction: column;
          height: 100%;

          .product-img{
            position: relative;
            display: block;

            img{
              display: block;
              width: 100%;
              height: 275px;
              object-fit: cover;
            }
            .type{
              position: absolute;
              left: 0;
              bottom: 16px;
              padding: 8px 16px;
              background-color: #42B883;
              color: #FFF;
              text-transform: capitalize;
            }
          }
          .detail{
            background-color: #496583;
            padding: 16px 8px;
            flex:  1 1 100%;
            border-radius: 0px 0px 8px 8px;

            .year{
              color: #aaa;
              font-size: 14px;

            }
            h3{
              color: #FFF;
              font-weight: 600;
              font-size: 17px;
            }
          }
        }
      }
    }
    
</style>