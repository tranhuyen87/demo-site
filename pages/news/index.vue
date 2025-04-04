<template>
  <main>
  <div v-if="response" class="content">
    <div class="main-news">
    </div>
    <div class="news-box" v-if="news">
      <ul v-if="news.list.length > 0">
        <li v-for="news in news.list" :key="news.topics_id">
          <NuxtLink
            :to="`/news/detail/${news.topics_id}`"
            class="c-topics"
          >
            <div class="c-topics__title">
              {{ news.ext_1 }}
            </div>
            <time class="c-topics__date" :datetime="news.ymd">{{
              news.ymd
            }}</time>
            <span class="c-badge">
              {{ news.contents_type_nm }}
            </span>
            <p class="c-topics__title">
              {{ news.subject }}
            </p>
          </NuxtLink>
        </li>
      </ul>
      <div>
        <a href="#">
          <div class="img" v-for="img in response.details.ext_3" :key="img.id"><img :src="img.url" /></div>
          <div class="ttl"></div>
          <div class="date"></div>
        </a>
        <ul class="tag">
            <li></li>
          </ul>
      </div>
    </div>
  </div>
  </main>
</template>

<script setup>
const config = useRuntimeConfig();

import { ref, computed } from 'vue';

const news = ref(null); // Initialize a ref for the news

const getNews = async () => {
  try {
    // Fetch the data from the API
    const res = await $fetch("/rcms-api/1/news/list", {
      baseURL: config.public.apiBase,
      credentials: "include",
    });
    news.value = res; // Store the news
  } catch (e) {
    console.error(e); // Log the error if it occurs
  }
};

// Call the function to fetch data
await getNews();


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


@media screen and (max-width: 767px) {
}

@media screen and (min-width: 768px) {
  body {
      font-size: 2em;
  }

}
</style>
