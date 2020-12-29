<template>
  <div class="card-container">
    <div>
            
      <div>
        <img :src="data.img" alt="pokemon"/>
      </div>

      <div>
        <div>
          <div>
            <p class="title">
              <b>{{data.name}}</b>
            </p>
          </div>
        </div>

        <div>
          <div>
            <p>
              {{(data.weight/10).toFixed(1)}} Kg - {{(data.height/10).toFixed(1)}}m 
            </p>
          </div>
        </div>

      </div>

        <div>
          <button @click="onClickPokemon(data.id)" >
              <img :src="viewIcon" alt="view-details"/>
          </button>
        </div>

      </div>
    </div>
</template>

<script>

// Assets
import ViewIcon from '../assets/lupa.png'

export default {
  props: {
    data: {
      default: function () {
        return {}
      },
      type: Object
    }, 
    id: {
      type: String,
      default: ''
    }
  },
  data() {
    return {
      viewIcon: ViewIcon
    }
  },
  methods: {

    /**
     * Function that is executed when a pokemon is seleted, returns pokemon selected data to app
     */
    onClickPokemon() {
      this.$emit('show-modal-card', this.id)
    }    
  }
}
</script>

<style scoped>
  .card-container {
      background-color: #F4EEFF;
      border-radius: 10px;
      padding: 25px 20px;
      width: 100%;
      max-width: 300px;
      margin: 20px auto;
      display: flex;
      justify-content: space-around;
      text-align: center;
  }

  .title {
    text-transform: capitalize;
    font-size: 20px;
  }

  button img {
      height: 30px;
      width: 25px;
  }
</style>