<template>
  <div>
    <h3>How to Use</h3>
    <p>Input cellphone numbers only</p>
    <p>Numbers must be 10 digit format starting with 9 and no spaces or other special characters</p>
    <p>Can get up to 50 numbers at once</p>
    <p>seperate each number with a comma(,) and no space</p>
    <p>this form has no input validation. Please make sure your input vales and format is correct</p>
  </div>
  <div>
    <textarea name="" id="" cols="60" rows="5" v-model="numbers" maxlength="600" placeholder="9170001122,9183334455,9060..."></textarea>
  </div>
  <br>
  <div>
    <button :disabled="gettingscores" @click="getscores">Get Score</button>
    <span v-if="gettingscores"> Getting Scores....</span>
  </div>
  <br>
  <table v-if="results.length > 0" :style="{width: '200px'}">
    <thead>
      <tr>
        <td>Number</td>
        <td>Score</td>
      </tr>
    </thead>
    <tbody>
      <tr v-for="obj in results" :key="obj.number">
        <td>{{ obj.number }}</td>
        <td>{{ obj.score }}</td>
      </tr>
    </tbody>
  </table>
</template>

<script>
const axios = require('axios');

export default {
  name: 'App',
  data(){
    return {
      results: [],
      gettingscores: false,
      numbers: "",
      errormsg:""
    }
  },
  methods:{
    getscores(){
      this.results = []
      this.gettingscores = true
      axios.post("https://utils.polka.ph/inquiro/inquiro.php", {
        numbers: this.numbers,
      })
      .then((res) => {
        if(res.data.status == "success"){
          this.results = res.data.response
        }
      })
      .catch(function (error) {
        console.log(error);
      })
      .finally(() => {
        this.gettingscores = false
      })
    }
  }
}
</script>

<style>

</style>
