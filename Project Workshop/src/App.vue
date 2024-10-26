<script setup>
import PocketBase from 'pocketbase';
import { ref, onMounted } from 'vue';

const db = new PocketBase('http://127.0.0.1:8090');

const titleVal = ref("");
const descriptionVal = ref(""); // Define descriptionVal
const data = ref([]);

async function submit() {
  const recordData = {
    Title: titleVal.value,
    Description: descriptionVal.value,
  };
  const result = await db.collection("posts").create(recordData);
  
  // Add the new record to the data list directly
  data.value.push(result);

  // Clear input fields after submission
  titleVal.value = "";
  descriptionVal.value = "";
}

onMounted(async () => {
  const result = await db.collection("posts").getFullList(); // Added missing parentheses
  data.value = result;
});

</script>

<template>
  <input v-model="titleVal" placeholder="Title">
  <input v-model="descriptionVal" placeholder="Description">
  <button class="bg-blue-300 transition duration-1000 hover:bg-blue-500" @click="submit">Submit</button>

  <ul>
    <li v-for="item in data" :key="item.id">
      <p>---------------------</p>
      <p>Title: {{ item.Title }}</p>
      <p>Decription: {{ item.Description }}</p>
    </li>
  </ul>
</template>

<style scoped>
</style>