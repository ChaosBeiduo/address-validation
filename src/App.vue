<template>
  <h1>Address Validation 🕵️‍♂️</h1>
  <AddressParser ref="addressParser" @info-parsed="handleParsedAddress"/>
</template>

<script setup>
import AddressParser from './components/AddressParser.vue'
import {ref} from "vue";

const addressParser = ref(null);
const errorMessage = ref('');


function handleParsedAddress(result) {
  errorMessage.value = '';

  if (!result){
    errorMessage.value = "地址为空";
  }

  console.log(result)

}

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
