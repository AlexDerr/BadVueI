<template>
  <div class="parent-div">
    Please select your phone number: {{selectedPhoneNumber}}

    <div v-for="(row, index) in phoneNumbers" :key="index">
      <button v-for="phoneNumber in row"
        class="number-button"
        @click="selectedPhoneNumber = phoneNumber"
      >
        {{phoneNumber}}
      </button>
    </div>
  </div>
</template>

<script setup lang="ts">
  import { onMounted } from 'vue';

  let rows = ref(10);
  let cols = ref(10);
  let phoneNumbers = ref([]);
  let selectedPhoneNumber = ref("");

  onMounted(() => {
    PopulatePhoneNumbers();
    PersistentUpdate();
  })

  function GenerateRandomPhoneNumber(){
    let number = String(Math.floor(Math.random() * (9999999999 - 1000000000 + 1)) + 1000000000);
    number = "(" + number.substring(0, 3) + ") " + number.substring(3, 6) + "-" + number.substring(6, 10);
    return number;
  }

  function PopulatePhoneNumbers(){
    phoneNumbers.value = []
    for(let i = 0; i < rows.value; i++){
      phoneNumbers.value.push([])
      for(let k = 0; k < cols.value; k++){
        phoneNumbers.value[i].push(GenerateRandomPhoneNumber());
      }
    }
  }

  async function PersistentUpdate(){
    while(true){
      let x = 0
      let y = 0;

      for(x; x < rows.value - 1; x++){
        await new Promise(f => setTimeout(f, 500));
        UpdateDiagonal(x, y)
      }

      for(y; y < cols.value; y++){
        await new Promise(f => setTimeout(f, 500));
        UpdateDiagonal(x, y)
      }
    }
  }

  function UpdateDiagonal(x: number, y: number){
    while(x >= 0 && y < cols.value){
      phoneNumbers.value[x][y] = GenerateRandomPhoneNumber();
      x--;
      y++;
    }
  }
</script>

<style>
  .parent-div{
    text-align: center;
  }

  .number-button{
    margin: 10px;
  }
</style>