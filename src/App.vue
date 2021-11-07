<template>
  <section class="galeria">
    <h2 class="galeria__container--titulo is-size-2">Galeria de pokemons</h2>
    <div class="galeria__conteiner column">
      <form class="galeria__form">
        <input
          class="galeria__form--inpt input is-primary"
          type="text"
          placeholder="Buscar pokemon pelo Nome"
          v-model="busca"
        />
        <button
          id="form_btn"
          class="galeria__form--btn button is-success"
          @click="buscar"
        >
          Buscar
        </button>
      </form>

      <ul class="card__container">
        <div
          class="card__container--itens"
          v-for="(poke, index) in filteredPokemons"
          :key="poke.url"
        >
          <pokemon :num="index + 1" :name="poke.name" :url="poke.url" />
        </div>
      </ul>
    </div>
  </section>
</template>

<script>
import axios from "axios";
import pokemon from "./components/pokemon";

export default {
  name: "App",
  data() {
    return {
      pokemons: [],
      filteredPokemons: [],
      busca: "",
    };
  },
  created: function () {
    axios
      .get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0")
      .then((res) => {
        console.log("pegou a lista de pokemom");
        this.pokemons = res.data.results;
        this.filteredPokemons = res.data.results;
      });
  },
  components: {
    pokemon,
  },
  methods: {
    buscar: function () {
      this.filteredPokemons = this.pokemons;
      if (this.busca == "" || this.busca == " ") {
        this.filteredPokemons = this.pokemons;
      } else {
        this.filteredPokemons = this.pokemons.filter(
          (pokemon) => pokemon.name.toUpperCase() == this.busca.toUpperCase()
        );
      }
    },
  },
};
</script>

<style>
.galeria {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 20px;
}

.galeria__form {
  margin-left: auto;
  margin-right: auto;
  text-align: center;
  max-width: 600px;
  margin-bottom: 10px;
  display: flex;
}

#form_btn {
  width: 30%;
  margin-left: 10px;
}

.card__container--itens {
  max-width: 500px;
  display: inline-block;
  border: 1px solid rgba(172, 172, 172, 0.747);
  border-radius: 5px;
  margin: 10px;
}
</style>
