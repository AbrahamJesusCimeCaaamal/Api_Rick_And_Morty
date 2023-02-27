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
      resulta:[],
      resultaID:false,
      resultaNombre:false,

      id: 0,
      infoUno:[],
      personaje: [],
      mostrar: false,
      inicio:true,
      moscarta:false,
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
    pagRe() {
      axios.get(this.info.prev)
      .then((response) => {
        this.info = response.data.info;
        this.personajes = response.data.results;
      })
      if (this.cont < 3){
        this.cont == this.cont

      }else{
         this.cont = this.cont - 1
      }
      
      
    },
    //metodo para pagina siguiente
    pag() {
       axios.get(this.info.next)
      .then((response) => {
        
        this.info = response.data.info;
        this.personajes = response.data.results;
      })
      if (this.cont > 41){
        this.cont == this.cont

      }else{
        this.cont = this.cont + 1
      }
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
      this.inicio=false
      console.log(this.mostrar)
    },

    buscar(search)  { 

    if(!isNaN (search) === true) {
      API_URL='https://rickandmortyapi.com/api/character/'+search
      axios.get(API_URL) 
      .then((response) => {
        console.log(response.data)
        this.resulta = response.data;
        console.log(this.resulta)
      })
      this.resultaID = true
      this.resultaNombre = false
    }
    else{              
      API_URL='https://rickandmortyapi.com/api/character/?name='+search
      axios.get(API_URL)
      .then((response) => {
        this.resulta = response.data.results;
        console.log(this.resultados)
      })
      this.resultaNombre  = true
      this.resultaID = false
    } 
      
  },
  },

  }

  



</script>

<template>
  <div >
  <h2 class="text-2xl py-3">Hay <strong>{{ info.count }} </strong> personajes en el programa de Rick & Morty</h2>
  <!--buscador por nombre por pagina-->
  <div class="flex justify-align-center mx-auto  my-5 ">
      
      <h3 class="text-2xl text-center  mx-5 sm:py-2"> <strong>Buscar  </strong></h3>
      <input class=" rounded-full py-2 px-4" type="text" v-model="busqueda" @input= buscar(busqueda) placeholder= "Buscar por nombre o id">
    </div>




</div>
<!--Resultados Buscador-->

  <!--si recibe true en la variable mostrarBuscadosID
      ES QUE BUSCO UN PERSONAJE POR SU ID-->
      <div v-if="resultaID" class="container flex flex-col items-center mx-auto md:flex md:gap-4" >
    <!--Recorre la nueva lista y por cada personaje que encuentra-->    
    <div class="buscado por id ">
      <!--muestra en pantalla la carta de personaje-->
      <img :src="resulta.image" alt="">
      <div class="info-buscados">
        <h2> Id {{ resulta.id}}</h2>
        <h2> Nombre {{ resulta.name}}</h2>
        <h4> Especie {{ resulta.species}}</h4>
        <h4> Estado {{ resulta.status}}</h4>
      </div>
    </div>      
  </div>

  <!--si no recibe true en la variable mostrarBuscados
      ENCONTRO VARIOS PERSONAJES POR SU NOMBRE-->
  <div v-if="resultaNombre">
    <!--Recorre la nueva lista y por cada personaje que encuentra-->    
    <div class="buscados" v-for="resul in resulta">
      <!--muestra en pantalla la carta de personaje-->
      <img :src="resul.image" alt="">
      <div class="info-buscados">
        <h2> Id {{ resul.id}}</h2>
        <h2> Nombre {{ resul.name}}</h2>
        <h4> Especie {{ resul.species}}</h4>
        <h4> Estado {{ resul.status}}</h4>
      </div>
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
<!-- Información de inicio de carga-->

<div v-if="inicio">
        
        <div class="personaje">
          
            <div class=" object-center  m-16  px-32 py-36   ">
          
          <img class=" px-1 py-1  scale-150   bg-black" src="./rich.gif" width="1500" height="1500" alt="Funny image">
          </div> 
          
        </div>
        
      </div>




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