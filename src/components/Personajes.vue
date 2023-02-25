<script>
import axios from 'axios'

let API_URL = `https://rickandmortyapi.com/api/character`

export default {

  data() {
    return {
      info: [],
      personajes: [],
      cont:2,
      search:"",
      id: 0,
      infoUno:[],
      personaje: [],
      mostrar: false,
    }
  },

  mounted() {
    axios.get(API_URL)
      .then((response) => {
        this.info = response.data.info;
        this.personajes = response.data.results;
      })
  },


  methods: {

    //metodo para pagina anterior
    pagRe(num) {
      API_URL='https://rickandmortyapi.com/api/character/?page='+(num-1)
      console.log(API_URL)
      axios.get(API_URL)
      .then((response) => {
        console.log(response.config)
        this.info = response.data.info;
        console.log(this.info)
        this.personajes = response.data.results;
      })
      this.cont = this.cont-1
      
      
    },
    //metodo para pagina siguiente
    pag(num) {
      API_URL='https://rickandmortyapi.com/api/character/?page='+num
      console.log(API_URL)
      axios.get(API_URL)
      .then((response) => {
        console.log(response.config)
        this.info = response.data.info;
        console.log(this.info)
        this.personajes = response.data.results;
      })
      this.cont++
    },
      //informacion de un personaje
  InfoPer(id) {
      
      //url de consumo API
      API_URL='https://rickandmortyapi.com/api/character/'+id 
      console.log(API_URL)
      //lo obtiene
      axios.get(API_URL)
      //
      .then((response) => {
        console.log(response) //regresa datos de un solo personaje
        //contiene el array de cada personaje con sus datos
        this.infoUno = response.data;
        console.log(this.infoUno)
      })
      this.id++
      this.mostrar=true
      console.log(this.mostrar)
    }
  },

    computed: {
    //metodo para buscar
    filtrarBusqueda(){
      //busca en cada personaje
      return this.personajes.filter((personaje) => {
        //busca en el nombre del personaje
        return personaje.name.toLowerCase().includes(this.search.toLowerCase())
      })
    },

  }
  }

  



</script>

<template>
  <div >
  <h2 class="text-2xl py-3">Hay <strong>{{ info.count }} </strong> personajes en el programa de Rick & Morty</h2>
  <!--buscador por nombre por pagina-->
  <div class="flex justify-align-center mx-auto  my-5 ">
      
      <h3 class="text-2xl text-center my-3 mx-5 sm:py-2">Buscar</h3>
      <input type="text" v-model="search" placeholder="Buscar por nombre">
    </div>




</div>
<div class="grid grid-cols-2 text-left">
    <div class="bg-purple-200">
      <!--Listado de cada 20 personajes o sea pagina-->
      <div class="text-2xl my-10 text-black mx-5 sm:py-2">
        <ul class="text-xl">
          <li v-for="p in personajes">
            <!--Llamando función obtener info personal por personaje-->
            <button class="hover:underline hover:text-blue-700  "  @click="InfoPer(p.id)"> {{ p.id }} .- {{ p.name }}</button>
            
          </li>
        </ul>
      </div>
      <div class="px-44 space-x-0.5">
<button class="border-2 border-rose-600  bg-blue-400 px-4 p-1 text-lg" v-on:click="pagRe(cont)">Anterior </button>
<button class="border-2  border-black  bg-red-400 px-4  p-1 text-lg" @auxclick="pag(cont)">{{ cont }}</button>
<button class="border-2 border-rose-600  bg-blue-400 px-4 p-1 text-lg" v-on:click="pag(cont)">Siguiente </button>

</div>
    </div>
    
    <div class="bg-red-200">

     <h1 class="text-3xl py-3 text-center font-bold text--900 border-cyan-500">Información del personaje</h1>
     <h2 class="text-2xl text-center" > <strong>{{ infoUno.name}}</strong></h2>
      <!--carta de personaje-->
      <div v-if="mostrar">
        
        <div class="personaje">
          
            <div class=" object-center  m-16  px-32    ">
          
          <img class=" px-1 py-1  scale-125 bg-black" :src="infoUno.image" alt="">
          </div> 
       
            
         
          <div class="info-personaje">
            <h4  class="px-44 text-xl">-Estado:  {{ infoUno.status}}</h4>
            <h4 class="px-44 text-xl">-Especie: {{ infoUno.species}}</h4>
            <h4 class="px-44 text-xl">-Género: {{ infoUno.gender }}</h4>
            
          </div>
          
        </div>
        
      </div>
    </div>
  </div>
 
</template>