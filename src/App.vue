<template>
  <div id="app">

    <div class="hero is-white is-gradient is-bold">
      <div class="hero-body">
        <h1 class="title">Rick & Morty <span class="has-text-success">Personajes</span></h1>

        <div class="field has-addons is-pulled-right">
          
          <div class="control">
            <input class="input is-rounded" type="text" v-model="search" @keydown.enter="searchData">
          </div>
          <div class="control">
            <button class="button is-success is-rounded" @click="searchData">Buscar</button>
          </div>

        </div>
      </div>
    </div>

    <div class="container">
      
      <div class="columns is-desktop is-mobile is-tablet is-multiline is-centered">
        <character v-for="char of characters" :key="char.id" :char="char" @showModal="loadModal"></character>
      </div>

      <nav class="pagination" role="pagination" aria-label="pagination">
        <a class="pagination-previous" @click="changePage(page-1)">Anterior</a>
        <ul class="pagination-list">
          <li>
            <a class="pagination-link is-current">{{ page }}</a>
          </li>
        </ul>
        <a class="pagination-next" @click="changePage(page+1)">Siguiente</a>
      </nav>

    </div>

    <div class="modal" :class="{'is-active': showModal}">
      <div @click="showModal=false" class="modal-background"></div>
      <div class="modal-card">
        <div class="modal-card-head">
          <div class="modal-card-title">{{ currentCharacter.name }}</div>
        </div>
        <div class="modal-card-body">
          <img :src="currentCharacter.image" :alt="currentCharacter.name">
          <p>Estado: <strong>{{ currentCharacter.status }}</strong></p>
          <p>Genero: <strong>{{ currentCharacter.gender }}</strong></p>
          <p>Especie: <strong>{{ currentCharacter.species }}</strong></p>
        </div>
        <div class="modal-card-foot">
          <button @click="showModal=false" class="button">Cancelar</button>
        </div>
      </div>
      <button @click="showModal=false" class="modal-close is-large" aria-label="close"></button>
    </div>

  </div>
</template>

<script>
import axios from 'axios'
import Character from './components/Character.vue'

export default {
  name: 'App',
  components: {
    Character
  },
  data() {
    return {
      characters: [],
      page: 1,
      pages: 1,
      search: '',
      showModal: false,
      currentCharacter: {}
    }
  },
  created() {
    this.fetch()
  },
  methods: {
    loadModal (id) {
      axios.
        get('https://rickandmortyapi.com/api/character/'+id)
          .then((res) => {
            this.showModal = true
            this.currentCharacter = res.data
            console.log(res.data)
          })
          .catch((err) => {
            console.log(err)
          })
    },
    searchData () {
      this.page = 1
      this.fetch()
    },
    changePage(page) {
      this.page = (page <= 0 || page > this.pages) ? this.page : page
      this.fetch()
    },
    fetch () {
      const params = {
        page: this.page,
        name: this.search
      }

      axios
        .get('https://rickandmortyapi.com/api/character', { params })
          .then((res) => {
            this.pages = res.data.info.pages
            this.characters = res.data.results
          })
          .catch((err) => {
            console.log(err)
          })
    }
  }
}
</script>

<style>
</style>
