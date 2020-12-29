<template>
  <div class="modal">

    <div class="header" closeButton>
      <div class="close-container">
        <button @click="onClickClose">x</button>
      </div>
      <div class="pokemon-title">
        {{pokemonSelected.name}}
      </div>
    </div>

    <div class="body">
      
      <img :src="pokemonSelected.img" alt="pokemon"/>

      <div v-for="ability in abilities" :key="ability.name">
        <hr/> 
        <h6 class="ability-title">{{ability.name}}</h6>
        <p>{{ability.description}}</p>
      </div>

    </div>
    
  </div>
  <div class="overlay"></div>
</template>

<script>

export default {
  props: {
    pokemonSelected: {
      type: Object,
      default: function() {
        return {}
      }
    }, 
    handleClose: {
      type: Function,
      default: function () {}
    },
    language: {
        type: String,
        default: 'es'
    },
    showCardDetail: {
      type: Boolean,
      default: false
    },
    abilities: {
      type: Array,
      default: function() {
        return []
      }
    }
  },
  methods: {
    onClickClose() {
      this.$emit('handle-close-modal')
    },
    translate: function (options, language, propertie) {

      if(Array.isArray(options)) {
        for(let option of options){
          if(option.language.name === language){
            return option[propertie]
          }
        }
      }
    }
  }
}
</script>

<style scoped>
  .modal {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: #FFF;
    padding: 30px;
    z-index: 999;
    width: 80%;
    max-width: 600px;
  }

  img {
    display: block;
    margin: 0 auto;
  }

  .ability-title {
    color: #FF1E56;
  }

  .pokemon-title {
    font-size: 20px;
    text-transform: uppercase;
    text-align: center;
  }

  .close-container {
    text-align: right;
    margin-bottom: 20px;
  }

  .overlay {
    display: block;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.7);
    position: absolute;
    top: 0;
    left: 0;
    z-index: 900;
  }
</style>