<template>
  <div class="container">
        <div class="input">
            <input @keypress.enter="getdata" type="text" v-model="input" placeholder="enter a country" id="inp">
            <button @click="getdata" id="btn">search</button>
        </div>
        <div class="result" v-if="data">
          <div class="main-word">
        <img :src="data.flags.png" alt="">
        <h2>{{data.name.common}}</h2>
    </div>
    <div class="info">
        <div class="inf">
            <h3>captial: <span>{{data.capital[0]}}</span></h3>
        </div>
        <div class="inf">
            <h3>containent: <span>{{data.continents[0]}}</span></h3>
        </div>
        <div class="inf">
            <h3>population: <span>{{data.population}}</span></h3>
        </div>
        <div class="inf">
            <h3>currency: <span>{{currency}}</span></h3>
        </div>
        <div class="inf">
            <h3>common languages: <span>{{ thelang }}</span></h3>
        </div>
    </div>
            </div>

            <h3 v-else id="errorCountry">{{ errormessage }}</h3>

          
    </div>
</template>

<script>
import axios from 'axios';
import {ref} from 'vue'

export default {
  setup(){
  
    let input = ref(null);
    let data = ref(null);
    let thelang = ref(null);
    let currency = ref(null);
    let errormessage = ref(null);

    const getdata = async ()=>{
      try{
        let result = await axios.get(`https://restcountries.com/v3.1/name/${input.value}?fullText=true`);
        if(result.status == 200){
        console.log(result.data[0]);
        data.value = result.data[0];
        let lang =  Object.values(data.value.languages);
        if(lang.length > 1){
         thelang.value = lang.join(' | ') 
          console.log(lang.join(' | '))
        } else{
         thelang.value = lang[0] 
          console.log(lang[0])
        }
         currency.value = Object.values(data.value.currencies)[0].name
      } else{
        console.log('error');
      }
      } catch(e){
        errormessage.value = `error there's no  country named ${input.value}`
      }
      
      
     
    }

    return {input, getdata , data , thelang , currency , errormessage}
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
body{
  background-color: skyblue;
}

.container{
      background-color: #fff;
      padding: 40px;
      width: 80%;
      margin: 50px auto;
      border-radius: 30px;
    display: flex;
    flex-direction: column;
    gap: 20px;
    .input{
    display: flex;
    align-items: center;
    justify-content: space-between;
    width: 100%;
    input{
    border: none;
    width: 70%;
    // border-bottom: 2px solid skyblue;
    outline: none;
    background-color: transparent;
    padding: 10px;
    border-radius: 10px;
    font-size: 20px;
}
button{
    border: none;
    padding: 10px 20px;
    color: #fff;
    background-color: skyblue;
    border-radius: 10px;
    margin: 0px 5px;
    cursor: pointer;
}
}
.result{
    display: flex;
    flex-direction: column;
    gap: 20px;
    .main-word{
  display: flex;
  align-items: center;
  flex-direction: column;
  i{
    color: skyblue;
    font-size: 25px;
    cursor: pointer;
}
h2{
    font-size: 35px;
    color: #333;
}
}
.result .example{
    position: relative;
    color: #ccc;
    &::before{
      content: '';
    width: 2px;
    height: 100%;
    position: absolute;
    background-color: skyblue;
    left: -10px;
    }
}
}
.info{
    display: flex;
    flex-direction: column;
    .inf h3{
    color: #333;
}
.inf h3 span{
    font-size: 15px;
     color: grey;
}
}
}

#errorCountry{
    color: crimson;
}



</style>
