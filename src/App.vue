<template>
  <div id="pokeapp">
    <header>
      <div class="container-logo">
        <img :src="Logo" alt="logo" />
      </div>
    </header>
    <language @handle-click-language="handleLanguageSelected"></language>
    <div>
      <loader v-if="showLoader"></loader>
      <card :id="pokemon.id" v-for="pokemon in pokemonList" v-bind:key="pokemon.id" :data="pokemon" @show-modal-card="showModalCard"></card>
    </div>
     <card-data :pokemonSelected="pokemonSelected" v-if="showCardData" :language="languageSelected" :cardDetailData="cardDetailData" :abilities="abilities" @handle-close-modal="handleClose" :showCardDetail="showCardData"></card-data>
     <paginator @handle-click-next="handleNext" @handle-click-previous="handlePrevious"></paginator>
  </div>
</template>

<script>

// Assets
import Logo from './assets/logo.png'

// Components
import Loader from './components/Loader.vue';
import Card from './components/Card.vue';
import CardData from './components/CardData.vue';
import Paginator from './components/Paginator.vue';
import Language from './components/Language.vue';

// Dependencies
import axios from 'axios'

export default {
  name: 'App',
  data() {
    return {
      showLoader: true,
      results : [],
      next: 0,
      previous: 0,
      pokemonList: [],
      showCardData: false,
      cardDetailData: {},
      pokemonSelected: null,
      initialGet: 'https://pokeapi.co/api/v2/pokemon?limit=5',
      languageSelected: 'en',
      Logo,
      abilities: []
    }
  },
  components: {
    Loader,
    Card,
    CardData,
    Paginator,
    Language
  },
  methods: {

    /**
     * Function that shows or hides loader
     * 
     * @param { Boolean } value
     */
    changeShowLoder(value) {
      this.showLoader = value
    },

    /**
     * Function that changes the language
     * 
     * @param { String } language
     */
    handleLanguageSelected(languageSelected) {
      this.languageSelected = languageSelected
    },

    /**
     * Function that is executed when next button is selected
     */
    handleNext() {
      this.initialGet = this.next
    },

    /**
     * Function that is executed when previous button is selected
     */
    handlePrevious() {
      this.initialGet = this.previous
    },

    /**
     * Function that is executed when the app is mounted and gets the pokemon's list
     */
    getPokemonList() {

      this.showLoader = true;
      this.pokemonList = []

      axios
        .get(this.initialGet)
        .then(response => {
          this.results = response.data.results;
          this.next = response.data.next;
          this.previous = response.data.previous
        })
        .catch(error => {
          console.log(error)
        })
        .finally(() => {
          this.showLoader  = false;
        })

    },

    /**
     * Function that gets details of each pokemon in the list
     */
    async getEachPokemonData() {
      
      await Promise.all(this.results.map(async (element) => {

      axios
      .get(element.url)
      .then(response => {
       
        this.pokemonList.push({
          id: response.data.id,
          name: response.data.name,
          abilities: response.data.abilities,
          img: response.data.sprites.front_default,
          weight: response.data.weight,
          height: response.data.height
        })
      
      })
      .catch(error => {
        console.log(error)
      })
      .finally(() => {
        this.showLoader  = false;
      })
    }))

    },

    /**
     * Function that is executed every time a pokemon is selected to show it's details
     * 
     * @param { String } id
     */
    showModalCard(id){
      let pokemonSelected = this.pokemonList.find((pokemon) => pokemon.id === id)   
      this.pokemonSelected = pokemonSelected
    },

    /**
     * Function that is executed when a close button is selected, hides modal
     */
    handleClose(){
      this.showCardData = false;
      this.pokemonSelected = null;
    },

    /**
     * Function that selects the correct option for the language selected
     * 
     * @param { Array } options
     * @param { String } language
     * @param { String } propertie  
     */
    translate (options, language, propertie) {

        for(let option of options){
          if(option.language.name === language){
              return option[propertie]
          }
        }

    },
                            
    /**
     * Function that gets the data for each ability
     */
    async getAbilities () {

      this.abilities = []
      
      if(this.pokemonSelected !== null && Array.isArray(this.pokemonSelected.abilities)){
                
        await Promise.all(this.pokemonSelected.abilities.map(async (abilityItem) => {

          axios
          .get(abilityItem.ability.url)
          .then(response => {
            this.abilities.push({
              name: this.translate(response.data.names, this.languageSelected, 'name'), 
              description: this.translate(response.data.flavor_text_entries, this.languageSelected, 'flavor_text')
            })

            this.showCardData = true
 
            return  {
              name: this.translate(response.data.names, this.languageSelected, 'name'), 
              description: this.translate(response.data.flavor_text_entries, this.languageSelected, 'flavor_text')
            }
    
          })
          .catch(error => {
            console.log(error)
          })
          .finally(() => {
            this.showLoader  = false;
          })
                                        
      }))}
    } 
  },
  mounted() {
    this.getPokemonList()
  },
  watch: {
    results: {
      handler (newValue) {
        this.getEachPokemonData(newValue)
      }
    }, 
    pokemonSelected : {
      handler (newPokemonSelected) {
        this.getAbilities(newPokemonSelected)
      }
    },
    initialGet : {
      handler () {
        this.getPokemonList()
      }
    }
  }
}

</script>

<style>

  #pokeapp {
    font-family: Helvetica, sans-serif;
    position: relative;
  }

  header {
    background-color: #FF1E56;
  }

  .container-logo {
      width: 150px;
      margin: 0 auto;
  }

  .container-logo img {
    width: 100%;
  }

  .container {
    margin-top: 50px;
  }

  #pokeapp {
    color: #313131;
  }

  button {
    border: 0;
    background-color: transparent;
  }

  button:focus {
    outline: 0px;
  }

</style>
