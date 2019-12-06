<template>
  <div class="home">
    <nav class="navbar is-fixed-top" role="navigation" aria-label="main navigation">
      <div class="navbar-brand">
        <a class="navbar-item has-text-white">
          <h6>Modelling HR Attrition</h6> 
        </a>
      </div>
    </nav>
    <div class="columns">
      <div class="is-input">
        <inputs @prediction="updatePredictions"></inputs>
      </div>
      <div class="column"></div>
      <div class="column is-two-thirds is-output">
        <outputs :predictedClass="predictedClass" :predictedProbability="predictedProbability"></outputs>
      </div>
    </div>
    
  </div>
</template>

<script>
import axios from 'axios'
import Inputs from '@/components/Inputs.vue'
import Outputs from '@/components/Outputs.vue'

export default {
  components: {
    Inputs,
    Outputs
  },
  data() {
    return {
      predictedClass: '',
      predictedProbability: ''
    }
  },
  mounted() {
    axios.get('http://127.0.0.1:5000/').then((response) => console.log(response.data))
  },
  methods: {
    updatePredictions(value) {
      this.predictedClass = value.prediction
      console.log("New class: ", this.predictedClass)
      this.predictedProbability = value.probability
      console.log("New proba: ", this.predictedProbability)
    }
  }
}
</script>

<style lang="scss">
nav.navbar {
  background-color: #0B0D3E;
}
.is-input {
  margin-top: 4rem;
  background-color: #E9E9F5;
  border-right: 1px solid black;
  position: fixed;
  width: 33%;
}
.is-output {
  margin-top: 4rem;
  padding-top: 1rem;
  background-color: #F2ECEC;
}
</style>
