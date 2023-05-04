<template>
  <div class="hello">
    <h2 class="title">The<span>Anime</span>App</h2>
    <header>
          <div class="container" >
              <form @submit.prevent="search">
                <input type="text" v-model="inputText" placeholder="Write Anime Name..."/>
                <input type="submit" value="Search">
              </form>
          </div>
    </header>
    
    <div class="cards" v-if="animeList.length > 0">

        <div class="card" v-for="anime in animeList" :key="anime.mal_id">
          
          <a :href="anime.url" target="_blank" v-if="anime.url">
            <img :src="anime.images.jpg.image_url" alt="Anime Img">
          </a>
          <h4>{{ anime.title.length >= 15 ? anime.title.slice(0, 15) + "..." : anime.title }}</h4>
          <a class="trailer" :href="anime.trailer.url" target="_blank" v-if="anime.trailer.url">Trailer</a>
          
        </div>
    </div>
    <div class="not-found" v-if="animeList.length <= 0">
      <h3 class="errors" v-if="errors">{{ errors }}</h3>
    </div>

  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'AnimeCard',
  data() {
    return {
      inputText: "",
      animeList :[],
      errors: ""
    }
  },
  methods: {
    search() {
      if (this.inputText != "" && this.inputText != null) {
        this.animeList = axios.get(`https://api.jikan.moe/v4/anime?q=${this.inputText}`)
        .then(res => {
          this.animeList = res.data.data;
          
          if (this.animeList <= 0) {
              this.errors = `'${this.inputText}' Anime Not Found :(`
          }
          this.inputText = ""
        })
        
      }
    }
  }
}
</script>


<style scoped>
.hello {
  margin: 100px 0;
}
.hello .title {
    color: #aba9a9;
    text-transform: uppercase;
    font-weight: bolder;
    font-size: 40px;
    margin: 30px auto;
}
.hello .title span {
  color: #000;
}
.container form input[type = "text"] {
    padding: 10px;
    display: block;
    margin: 10px auto;
    width: 70%;
    border: none;
    background-color: #eee;
    text-align: center;
    font-size: 20px;
    border-radius: 20px 0px 20px 0;
    transition: .8s;
}
.container form input[type = "text"]:focus {
  outline: none;
  background-color: #656565;
  color: #fff;
  border-radius: 0px 20px 0px 20px;
}
.container form input[type = "submit"] {
  padding: 10px 20px;
  color: #fff;
  background-color: #42b983;
  border-radius: 10px;
  font-size: 16px;
  user-select: none;
  border: none;
}
.cards {
    display: flex;
    flex-wrap: wrap;
    gap: 20px;
    justify-content: center;
    margin: 100px 0;
}
.cards .card {
    background-color: transparent;
    padding: 20px;
    border-radius: 10px;
    transition: .5s;
    width: 200px;
    position: relative;
    box-shadow: 0 0 10px #eee;
    height: 350px;
}

.card h4 {
    margin: 5px auto;
    color: #2c3e50;
}
.card img {
  border-radius: 10px;
  width: 100%;
  margin: 20px auto;
  cursor: pointer;
  transition: .6s;
  height: 200px;
}
.card img:hover {
  transform: scale(1.1);
}
.card .trailer {
    position: absolute;
    bottom: 10px;
    right: 10px;
    background-color: #42b983;
    color: #fff;
    padding: 6px 15px;
    border-radius: 10px;
    font-size: 15px;
    text-transform: capitalize;
    font-weight: bold;
    letter-spacing: .5px;
    cursor: pointer;
}
.errors {
    background-color: #42b983;
    width: fit-content;
    margin: 30px auto;
    padding: 15px;
    border-radius: 10px;
    color: #fff;
    font-size: 20px;
}

@media screen and (max-device-width: 400px) {
    .cards .card {
      width: 150px;
    }
}
@media screen and (max-device-width: 500px) {
    .cards .card {
      width: 190px;
    }
}
</style>
