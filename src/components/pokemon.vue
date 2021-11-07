<template>
  <!-- <div class="pokemons__card"> -->
    <!--     <h1>{{ num }} {{ name | upper }}</h1>
    <small>{{ url }}</small> -->

    <li class="card">
      <div class="card-image">
        <figure>
          <img :src="currentImg" alt="Placeholder image" />
        </figure>
      </div>
      <div class="card-content">
        <div class="media">
          <div class="media-left">
            <figure class="image is-48x48">
              <img :src="backImg" alt="Placeholder image" />
            </figure>
          </div>
          <div class="media-content">
            <p class="title is-4">{{ num }} - {{ name | upper }}</p>
            <p class="subtitle is-6">{{ pokemon.type }}</p>
          </div>
        </div>

        <div class="content">
          <span class="pokemon__btn button is-primary " @click="spriteAnterior">Imagem anterior</span>
          <span class="pokemon__btn button is-info " @click="proximoSprite">Proxima imagem</span>

        </div>
      </div>
    </li>
  <!-- </div> -->
</template>

<script>
import axios from "axios";

export default {
  created: function () {
    axios.get(this.url).then((res) => {
      // console.log(res.data.types[0].type.name);
      this.pokemon.type = res.data.types[0].type.name;
      this.pokemon.front = res.data.sprites.front_default;
      this.pokemon.back = res.data.sprites.back_default;
      this.currentImg = this.pokemon.front;
      this.backImg = this.pokemon.back;
      console.log(this.pokemon);
    });
  },
  data() {
    return {
      isFront : true,
      currentImg : '',
      backImg : '',
      pokemon: {
        type: "",
        front: "",
        back: "",
      },
    };
  },
  props: {
    num: Number,
    name: String,
    url: String,
  },
  filters: {
    upper: function (value) {
      var newNome = value[0].toUpperCase() + value.slice(1);
      return newNome;
    },
  },
  methods : {
      proximoSprite : function() {
          if (this.isFront) {
              this.isFront = false;
              this.backImg = this.currentImg
              this.currentImg = this.pokemon.back;
          }
      },
      spriteAnterior : function() {
          if (this.isFront == false) {
              this.isFront = true;
              this.backImg = this.currentImg
              this.currentImg = this.pokemon.front;
          }
      }      
  }
};
</script>

<style>

.pokemons__card {
  margin-top: 1px;
  margin-bottom: 10px;
}

.pokemon__btn:last-child {
  margin-left: 10px;
}

</style>