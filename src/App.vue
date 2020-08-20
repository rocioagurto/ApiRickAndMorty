<template>
  <div id="app">
    <div class="hero is-dark is-bold" style="padding:4rem">
      <div class="container hero-body is-mobile is-centered" >
       
        <span class="subtitle" style="font-size:2rem">RICK&MORTY </span>
        <div class="field has-addons is-pulled-right ">
          <div class="control">
            <input v-model="search" type="text" class="input is-rounded" @keyup.enter="searchData">
          </div>
          <div class="control">
            <button class="button is-warning is-rounded" @click="searchData">Buscar</button>
          </div>
        </div>
      </div>
    </div>
    <div class="container is-centered">
      <div class="columns is-desktop is-mobile is-tablet is-multiline is-centered">
        <character
        @showModal="showModal"
        v-for="character of characters"
        :key="character.id"
        :character="character"
        />
      </div>
      <nav class="pagination is-rounded" style="margin: 1rem; margin-bottom: 2rem" role="navegation" aria-label="pagination">
        <a class="pagination-previous" @click="changePage(page-1)">Anterior</a>
        <ul class="pagination-list">
          <li>
            <a class="pagination-link is-current has-text-white" aria-label="Goto page 1">{{page}}</a>
          </li>
        </ul>
        <a class="pagination-next" @click="changePage(page+1)">Siguiente</a>
      </nav>
    </div>
    <div class="modal" :class="{'is-active' : modal}" v-if="modal">
      <div class="modal-background" @click="modal = false;"></div>
      <div class="modal-card">
        <div class="modal-card-head">
          <p class="modal-card-title">Acerca de: {{currentCharacter.name}}</p>
        </div>
        <div class="modal-card-body">
          <p>
            Género:
            <strong>{{currentCharacter.gender}}</strong>
          </p>
          <p>
            Estado:
            <strong>{{currentCharacter.status}}</strong>
          </p>
          <p>
            Especie:
            <strong>{{currentCharacter.species}}</strong>
          </p>
          <p>
            Tipo:
            <strong>{{currentCharacter.type}}</strong>
          </p>
        </div>
        <footer class="modal-card-foot">
          <button class="button is-danger" @click="modal = false">Cerrar</button>
        </footer>
      </div>
    </div>
  </div>
</template>


<script>
import axios from "axios";
import character from "./components/character";

export default {
  name: "App",
  components: {
    character
  },
  data() {
    return {
      characters: [],
      page: 1,
      pages: 1,
      search: "",
      modal: false,
      currentCharacter: {}
    };
  },
  created() {
    this.fetch();
  },
  methods: {
    fetch() {
      const params = {
        page: this.page,
        name: this.search
      };
      axios.get("https://rickandmortyapi.com/api/character/", { params })
      
        .then(res => {
          this.characters = res.data.results;
          this.pages = res.data.info.pages;
          console.log(res.data);
        })
        .catch(err => {
          console.log(err);
        });
    },
    changePage(page) {
      this.page = page <= 0 || page > this.pages ? this.page : page;
      this.fetch();
    },
    searchData() {
      this.page = 1;
      this.fetch();
    },
    showModal(id) {
      this.fetchOne(id);
    },
    async fetchOne(id) {
      let result = await axios.get(
        `https://rickandmortyapi.com/api/character/${id}/`
      );
      this.currentCharacter = result.data;
      this.modal = true;
      console.log(this.currentCharacter, "personaje");
    }
  }
};
</script>

