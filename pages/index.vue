<template>
  <v-container fluid grid-list-xl>
    <h2>Categorias</h2>
    <v-layout row justify-center>
      <v-flex xs10>
        <!-- <v-card dark color="primary">
          <v-card-text>one</v-card-text>
        </v-card> -->
        <v-btn
          :loading="loadingPolitica"
          :disabled="loading"
          color="#7F382D"
          @click="getRandom(0)"
          block
          dark
          large
    >
      Política
    </v-btn>
    
    <v-btn
          :loading="loadingEstiloDeVida"
          :disabled="loading"
          color="#D35C4A"
          @click="getRandom(1)"
          block
          dark
          large
    >
      Estilo de Vida
    </v-btn>
    <v-btn
          :loading="loadingEducacion"
          :disabled="loading"
          color="#5762D5"
          @click="getRandom(2)"
          block
          dark
          large
    >
      Educación
    </v-btn>
    <v-btn
          :loading="loadingAntesYAhora"
          :disabled="loading"
          color="#ABC4AB"
          @click="getRandom(3)"
          block
          dark
          large
    >
      Antes y ahora
    </v-btn>
    <v-btn
          :loading="loadingReligion"
          :disabled="loading"
          color="#DC7F9B"
          @click="getRandom(4)"
          block
          dark
          large
    >
      Religión
    </v-btn>
    <v-btn
          :loading="loadingSexo"
          :disabled="loading"
          color="#A04668"
          @click="getRandom(5)"
          block
          dark
          large
    >
     Sexo
    </v-btn>
    
    <v-btn
          :loading="loadingMenos18"
          :disabled="loading"
          color="#857E61"
          @click="getRandom(6)"
          block
          dark
          large
    >
      Menos de 18
    </v-btn>
    <v-btn
          :loading="loadingFuturo"
          :disabled="loading"
          color="#FA9F42"
          @click="getRandom(7)"
          block
          dark
          large
    >
     Futuro
    </v-btn>
    <v-btn
          :loading="loadingMiscelaneaFantasia"
          :disabled="loading"
          color="#45062E"
          @click="getRandom(8)"
          block
          dark
          large
    >
      Miscelanea Fantasía
    </v-btn>
      </v-flex>
      
    </v-layout>
  </v-container>
</template>

<script>
import axios from 'axios'

export default {
  
  data () {
      return {
        loader: null,
        loading: false,
        loadingPolitica: false,
        loadingEstiloDeVida: false,
        loadingEducacion: false,
        loadingAntesYAhora: false,
        loadingReligion: false,
        loadingSexo: false,
        loadingMenos18: false,
        loadingFuturo: false,
        loadingMiscelaneaFantasia: false,
      }
  },
  methods:{
    getCategoria: function(categoria){
      switch (categoria) {
        case 0:
          return "politica";
          break;
        case 1:
          return "estilo-de-vida";
          break;
        case 2:
          return "educacion";
          break;
        case 3:
          return "antes-y-ahora";
          break;
        case 4:
          return "religion";
          break;
        case 5:
          return "sexo";
          break;
        case 6:
          return "menos-18";
          break;
        case 7:
          return "futuro";
          break;
        case 8:
          return "miscelanea-fantasia";
          break;
        case 666:
          return "random";
          break;
      }
    },
    cargandoFrase: function(categoria, estado){
      switch (categoria) {
            case 0:
              this.loadingPolitica = estado;
              break;
            case 1:
              this.loadingEstiloDeVida = estado;
              break;
            case 2:
              this.loadingEducacion = estado;
              break;
            case 3:
              this.loadingAntesYAhora = estado;
              break;
            case 4:
              this.loadingReligion = estado;
              break;
            case 5:
              this.loadingSexo = estado;
              break;
            case 6:
              this.loadingMenos18 = estado;
              break;
            case 7:
              this.loadingFuturo = estado;
              break;
            case 8:
              this.loadingMiscelaneaFantasia = estado;
              break;
          }
    },
    getRandom: function(categoria){
      this.cargandoFrase(categoria, true);
      this.loading = true;
      axios
        .get('https://conversaciones-api.herokuapp.com/temas/getRandom/'+categoria)
        .then(response => {
          this.cargandoFrase(categoria, false);
          this.loading = false;
          this.$router.push(`/${this.getCategoria(categoria)}/${response.data.id}`);
        })
        .catch(error =>{
          console.log(`error: ${error}`)
        })
      
    }
  }
}
</script>

<style>
  .boton-rosa {
    background-color: pink;
  }
  .custom-loader {
    animation: loader 1s infinite;
    display: flex;
  }
  @-moz-keyframes loader {
    from {
      transform: rotate(0);
    }
    to {
      transform: rotate(360deg);
    }
  }
  @-webkit-keyframes loader {
    from {
      transform: rotate(0);
    }
    to {
      transform: rotate(360deg);
    }
  }
  @-o-keyframes loader {
    from {
      transform: rotate(0);
    }
    to {
      transform: rotate(360deg);
    }
  }
  @keyframes loader {
    from {
      transform: rotate(0);
    }
    to {
      transform: rotate(360deg);
    }
  }
</style>