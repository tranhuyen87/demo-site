<template>
  <main>
  <div v-if="response" class="content">
    <div class="main-news">
      <div><img :src="response.details.ext_1.url" /></div>
      <h1>{{ response.details.ext_2 }}</h1>
    </div>
    <ul class="news-list">
      <li v-for="item in combinedData" :key="item.id">
        <img v-if="item.url" :src="item.url" :alt="item.desc" />
        <p v-if="item.desc">{{ item.desc }}</p>
        <p v-if="item.info">{{ item.info }}</p>
      </li>
    </ul>
  </div>
  </main>
</template>

<script setup>
const config = useRuntimeConfig();

import { ref, computed } from 'vue';

const response = ref(null); // Initialize a ref for the response

const getResponse = async () => {
  try {
    // Fetch the data from the API
    const res = await $fetch("/rcms-api/5/news/3", {
      baseURL: config.public.apiBase,
      credentials: "include",
    });
    response.value = res; // Store the response
  } catch (e) {
    console.error(e); // Log the error if it occurs
  }
};

// Call the function to fetch data
await getResponse();

// Combine the data into a single array after the response is received
const combinedData = computed(() => {
  if (!response.value || !response.value.details) return []; // Check for valid response

  const ext3 = response.value.details.ext_3 || [];
  const ext4 = response.value.details.ext_4 || [];
  const ext5 = response.value.details.ext_5 || [];

  return ext3.map((item, index) => ({
    id: item.id,
    url: item.url,
    desc: item.desc,
    info: ext5[index] || '', // Get corresponding info from ext_5
  })).concat(ext4.map((desc, index) => ({
    id: `ext_4_${index}`, // Unique ID for ext_4 items
    desc: desc,
  })));
});


</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: content-box;
}
body {
  margin: 0;
  font-size: 1em;
  line-height: 1.5;
}
.content {
  margin: 0 auto;
  max-width: 1000px;
}

ul {
  margin: 0;
  padding: 0;
  list-style: none;
}

img {
  max-width: 100%;
}

section {
  max-width: 1200px;
  margin: 3em auto;
  padding: 0 20px;
}

h1 {
  margin: 0.5em 0;
  font-size: 1.8em;
}

h2 {
  margin: 2em auto;
  font-size: 1.5em;
  text-align: center;
}

h3 {
  margin: 1em auto;
  font-size: 1.2em;
}

p {
  margin: 1em 0;
  font-size: 0.75em;
}

.header {
  position: relative;
  width: 100%;
  height: 300px;
  background-position: center center;
  background-size: cover;
}

.header__text {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100%;
  padding: 0 20px;
}

.works__item:not(:first-child) {
  margin-top: 3em;
}

@media screen and (max-width: 767px) {
  .header::before {
      width: 100%;
      height: 300px;
      position: absolute;
      top: 0;
      left: 0;
      background-color: rgba(0, 0, 0, 0.5);
      z-index: 1;
      content: '';
  }

  .header__text {
      position: absolute;
      top: 0;
      left: 0;
      color: #fff;
      z-index: 10;
  }
}

@media screen and (min-width: 768px) {
  body {
      font-size: 2em;
  }

  .header {
      height: 600px;
  }

  .header__text {
      width: 1200px;
      align-items: flex-end;
      margin: auto;
  }

  .header__text p {
      width: 600px;
  }

  .works__item {
      display: flex;
  }

  .works__item img {
      width: 400px;
      margin-right: 2em;
  }

  .about {
      text-align: center;
  }
}
</style>
