<template>
  <div class="address-parser">
    <textarea placeholder="Paste or input text to automatically identify names, phone numbers, and addresses." id="info-input" v-model="userInput"/>
    <button type="button" @click="parseAddressInfo" :class="{ 'has-value': userInput !== '' }">Validate</button>
  </div>
</template>

<script setup>

import {ref} from "vue";

const userInput = ref('IbunnChou 15057474575 上海市静安区江场三路238号');
const separators = [", ", " ", ","]
// const userInfo = ref({
//   name: '',
//   tel: '',
//   province: '',
//   city: '',
//   district: '',
//   postalCode: '',
//   detail: ''
// });

// eslint-disable-next-line no-undef
// const emit = defineEmits(['info-parsed']);

// eslint-disable-next-line no-undef
defineExpose({
  clearInput
});

function clearInput(){
  userInput.value = "";
}

async function parseAddressInfo() {
  let input = userInput.value;
  let basicInfo = {
    name: "",
    phone: "",
    address: ""
  }

  // Identify the phone number first.
  let separator = separators.find(separator => input.split(separator).length > 1);

  // Ideal: Information is divided by separators!
  if (separator){
    // This ensures that multiple separators are treated as one and makes the input clean for further processing.
    let separatorRegex = new RegExp(`[${separators.join('')}]+`, 'g');
    let normalizedInput = input.replace(separatorRegex, ' ');

    input = normalizedInput.split(' ').filter(part => part.trim() !== '');

    // Extract the phone
    basicInfo.phone = input.find(part => /^\d+$/.test(part));
    console.log(input.splice(basicInfo.phone));

    // Unfortunately, there are no separators.
  }else{
    // Here, I use the Chinese mobile number prefix for detection
    const telRegex = /(\+?86)?(1[3-9]\d{9})/;
    const telMatch = input.match(telRegex);

    if(telMatch){
      // If a phone number is detected, save it and then replace it with a blank
      basicInfo.phone = telMatch[0];
      input = input.replace(basicInfo.phone, " ").trim().split(" ");
      console.log(input)
    }
  }

  //
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.address-parser {
  position: relative;

  margin: 0 auto;

  width: 50vw;

  textarea {
    display: block;
    box-sizing: border-box;
    resize: none;

    padding: 24px;

    width: 100%;

    color: #ababab;
    font-size: 18px;
    font-weight: bold;
    background: rgb(243, 243, 243);

    border: none;
    border-radius: 18px;

    overflow-y: hidden;
  }

  textarea:focus{
    outline: none;
    color: gray;
  }

  button {
    position: absolute;
    z-index: 100;
    right: 6px;
    bottom: 6px;

    padding: 6px 12px;

    border: none;
    border-radius: 18px;
    background-color: color-mix(in srgb, #dcdcdc 50%, transparent);
    color: #ababab;
    font-weight: bold;
    font-size: 16px;

    cursor: context-menu;

    transition: all 0.2s ease;
  }

  button.has-value {
    color: white;
    background-color: #ababab;

    cursor: pointer;
  }

  button.has-value:hover, button.has-value:active {
    opacity: 0.8;
  }
}

</style>
