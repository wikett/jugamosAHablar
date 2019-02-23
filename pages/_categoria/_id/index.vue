<template>
  <v-container fluid grid-list-xl>
    <v-layout row>
      <v-flex xs12 text-xs-center>
        <span class="titulo-categoria">- {{this.getCategoria(tema.categoria)}} -</span>
        <hr class="titulo-categoria">
        <!-- 
        <img src="~assets/images/shake-hands.svg" alt="Vuetify.js" width="80" class="mb-5">-->
        <blockquote class="blockquote">
          &#8220;{{tema.frase}}.&#8221;
          <footer>
            <small>
              <em>&mdash;{{tema.autor}}</em>
            </small>
          </footer>
        </blockquote>
      </v-flex>
    </v-layout>
    <v-layout row>
      <v-flex xs12 text-xs-right>
        <v-btn icon>
          <v-icon color="rgba(95, 184, 127, 0.78);">thumb_up</v-icon>
        </v-btn>
        <v-btn icon>
          <v-icon color="rgba(255, 229, 32, 0.48);">thumb_down</v-icon>
        </v-btn>
      </v-flex>
    </v-layout>
    <v-layout row>
      <v-flex x12 text-xs-center>
        <v-btn
          color="#683816"    
          @click="getRandom(tema.categoria)"      
          block
          dark
          large
        >Nueva frase</v-btn>
        <v-btn
          color="#3D9B7F"
          href="/"
          nuxt
          block
          dark
          large
        >Cambiar categoría</v-btn>
      </v-flex>
    </v-layout>
    <v-layout row>
      <v-flex xs12 text-xs-right>
        <span style="font-size:1rem;color:#ffa6b7;">Aporta tu frase</span>
        

           <v-dialog v-model="dialog" max-width="600px">
      <v-btn slot="activator"  icon style="margin: 0;">
          <v-icon style="margin:0; font-size:2.5rem; color:#F15573">add_circle_outline</v-icon>
        </v-btn>
      <v-card>
        <v-card-title>
          <span class="headline">Crea tu frase</span>
        </v-card-title>
        <v-card-text>
          <v-container grid-list-md>
            <v-layout wrap>
              <v-flex xs12>
                <v-select
                  :items=categorias 
                  item-text="c"
                  item-value="v"
                  label="Categoria*"
                  v-model=nuevaCategoria
                  required
                ></v-select>
              </v-flex>
              <v-flex xs12>
                <v-textarea
                  name="nuevaFrase"
                  label="Crea una nueva frase"
                  v-model=nuevaFrase
                ></v-textarea>
              </v-flex>
              <v-flex xs12>
                <v-select
                  :items="['Anónimo']"
                  v-model=tipoAutor
                  label="Autor*"
                  required
                ></v-select>
              </v-flex>
              <div v-show="tipoAutor=='Usuario'">
              <v-flex xs12>
                <h5><span>Crear usuario</span> | <span style="color:gray;">Login</span></h5>
                <v-text-field label="Usuario*" hint="Tu nombre artístico a la hora de crear frases" required></v-text-field>
              </v-flex>
               <v-flex xs12>
                <v-text-field label="Email*" hint="Tu nombre artístico a la hora de crear frases" required></v-text-field>
              </v-flex>
              <v-flex xs12>
                <v-text-field label="Password*" type="password" required></v-text-field>
              </v-flex>
              <v-flex xs12>
                <v-text-field label="Repite Password*" type="password" required></v-text-field>
              </v-flex>
              </div>
            </v-layout>
            <v-layout>
              <v-flex xs12>
                <small style="font-size:0.8rem;">*indicates required field</small>
                 <v-checkbox color="gray"
                  v-model="checkbox"
                  :label="`Acepto las condiciones de privacidad y uso`"
                ></v-checkbox>
              </v-flex>
            </v-layout>
          </v-container>
          
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1"  @click="dialog = false">Cerrar</v-btn>
          <v-btn color="blue darken-1"
            @click="crearAnonimo()"
            :loading="loading"
            :disabled="loading"
          >Guardar</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
     <v-dialog
      v-model="dialogCreado"
      width="500"
    >
      

      <v-card>
        <v-card-title
          class="headline green lighten-2"
          primary-title
        >
          ¡Muchas gracias!
        </v-card-title>

        <v-card-text style="font-size: 1.2rem;">
          Gracias por colaborar con ¿Jugamos a Hablar?. Esperemos que tu frase sirva de conversación, inspiración, riñas o risas. El caso que sirva para que la gente pueda interactuar.
        </v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="primary"
            flat
            @click="dialogCreado = false"
          >
            Cerrar
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
       
      </v-flex>
    </v-layout>
  </v-container>
</template>
<script>
import axios from "axios";
export default {
  data: () => ({
      dialog: false,
      dialogCreado: false,
      checkbox: true,
      tipoAutor: "",
      loading: false,
      nuevaFrase: "",
      nuevaCategoria: "",      
      categorias: [{c: 'Política', v: 0}, {c:'Estilo de Vida', v:1}, {c:'Educación', v:2}, {c:'Antes y Ahora', v:3}, {c:'Religión', v:4}, {c:'Sexo', v:5}, {c:'Menos de 18', v:6}, {c:'Futuro', v:7}, {c:'Miscelanea Fantasía', v:8}]
    }),
  watchQuery: true,
  asyncData({ params, error }) {
    console.log(`categoria: ${params.categoria}`);
    return axios
      .get(`https://conversaciones-api.herokuapp.com/temas/${params.id}`)
      .then(res => {
        console.log(res.data);
        return { tema: res.data };
      })
      .catch(e => {
        error({ statusCode: 404, message: "Post not found" });
      });
  },
  methods: {
    crearAnonimo: function(){
      this.loading = true;
      console.log(`this.nuevaCategoria: ${this.nuevaCategoria}`)
      console.log(`this.nuevaFrase: ${this.nuevaFrase}`)
      axios
        .post('https://conversaciones-api.herokuapp.com/temas', 
        {
          	categoria: this.nuevaCategoria.v,
	          frase: this.nuevaFrase,
	          autor: "anónimo"
        })
        .then(response => {
          this.loading = false;
          this.dialog = false;
          this.dialogCreado = true;
          
        })
        .catch(error =>{
          console.log(`error: ${error}`)
        })
      
    },
     getCategoria: function(categoria){
      switch (categoria) {
        case 0:
          return "Política";
          break;
        case 1:
          return "Estilo de vida";
          break;
        case 2:
          return "Educación";
          break;
        case 3:
          return "Antes y ahora";
          break;
        case 4:
          return "Religión";
          break;
        case 5:
          return "Sexo";
          break;
        case 6:
          return "Menos de 18";
          break;
        case 7:
          return "Futuro";
          break;
        case 8:
          return "Miscelanea Fantasía";
          break;
        case 666:
          return "Random";
          break;
      
        default:
          break;
      }
    },
    getCategoriaURL: function(categoria){
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
    getCategoriaNombre: function(categoria) {
      switch (categoria) {
        case "politica":
          return 0;
          break;
        case "estilo-de-vida":
          return 1;
          break;
        case "educacion":
          return 2;
          break;
        case "antes-y-ahora":
          return 3;
          break;
        case "religion":
          return 4;
          break;
        case "sexo":
          return 5;
          break;
        case "menos-18":
          return 6;
          break;
        case "futuro":
          return 7;
          break;
        case "miscelanea-fantasia":
          return 8;
          break;
        case "random":
          return 666;
          break;

        default:
          break;
      }
    },
    getRandom: function(categoria){
      console.log(`getRandom: ${categoria}`)
      axios
        .get('https://conversaciones-api.herokuapp.com/temas/getRandom/'+categoria)
        .then(response => {
          console.log(response.data.id)
          this.$router.push(`/${this.getCategoriaURL(categoria)}/${response.data.id}`);
        })
        .catch(error =>{
          console.log(`error: ${error}`)
        })
      
    }
  }
};
</script>

<style lang="scss" scoped>
.titulo-categoria {
  color: #ffffffab;
  border-color: #ffffffab;
}
.blockquote {
  font-size: 1.6rem;
  padding: 0;
  margin-top: 80px;
}
h3 {
  font-size: 3rem;
  margin-top: 25px;
  margin-bottom: 25px;
  font-weight: 600;
  color: #ebebeb;
}
hr {
  border-top: 1px solid rgba(243, 237, 237, 0.4);
}
.group-botones {
  background-color: transparent;
  border: none;
  font-size: 1.5rem;
  color: white;
}
.mas-opciones {
  color: #a29b9b;
}
.autor {
  color: #5cd9d9;
}
</style>
