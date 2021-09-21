<template>
  <div class="hello">
    <h1>Calculator</h1>
    <div class="main-app">
      <h4 class="text-center">Enter the numbers</h4>
      <input type="number" placeholder="number 1" v-model="first_number">
      <input type="number" placeholder="number 2" v-model="second_number">
      <button class="text-center" @click="get_sum">Sum</button>

      <hr>

      <h4 class="text-center">Results</h4>
      <div class="answer">{{answer}}</div>

      <div class="history" v-if="sums">
        <h1>History</h1>
        <ul>
          <li v-for="(sum , index) in sums" :key="index">{{sum.first_number}} + {{sum.second_number}} = {{sum.answer}}</li>
        </ul>
      </div>
    </div>
    
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'Calculator',
  data: function() {
      return {
      first_number: '',
      second_number: '',
      answer: '',
      sums: ''
    }
  },
  methods: {
    async get_sum(){
      const {data}  = await axios.post('http://localhost:8000/api/get-sum' , {'first_number' :this.first_number , 'second_number': this.second_number})
      this.answer = data.sum
      this.sums.push({
        first_number : this.first_number,
        second_number : this.second_number,
        answer: data.sum
        });
      this.first_number = '';
      this.second_number = '';
    },
    async fetchData(){
      const {data}  = await axios.get('http://localhost:8000/api/all-sum' );
      this.sums = data
    }
  },
  created(){
      this.fetchData();

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

h1 {
    color: white;
    background: red;
    padding:50px 0;
    font-weight: 300;
    max-width: 700px;
    margin: 0 auto;
}

h4 {
    font-weight: 300;
}


.main-app  {
    max-width: 700px;
    margin: 0 auto;
    color: gray;
    border: 1px solid gray;
    -webkit-box-shadow: 2px 2px 16px 2px rgb(0 0 0 / 53%);
    box-shadow: 2px 2px 16px 2px rgb(0 0 0 / 53%);
}

input[type="number"] {
    width: 50%;
    display: block;
    margin: 0 auto;
    padding-bottom: 20px;
    margin-bottom: 30px;
}

button.text-center {
    background: red;
    border: none;
    color: white;
    font-size: 20px;
    padding: 10px 100px;
    cursor: pointer;
}

body{
  padding: 0;
  margin: 0;
}

li {
  display: block;
  width:100%;
}
</style>
