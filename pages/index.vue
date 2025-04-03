<template>
  <main>
    <h1>Kuroco News</h1>
    <div v-if="error">
      <p>Error fetching data: {{ error.message }}</p>
    </div>
    <div v-else-if="news">
      <h2>{{ news.details.ext_2 }}</h2>
      <ul>
          <li v-for="n in news.details.ext_3" :key="n.slag" class="works__item">
              <img  />
              <div class="works__item__text">
                  <h3>{{ n.ext_4 }}</h3>
                  <p>{{ n.ext_5 }}</p>
              </div>
          </li>
      </ul>
    </div>
    <div v-else>
      <p>Loading...</p>
    </div>
  </div>
  </main>
</template>

<script setup>
import { ref } from 'vue';

const news = ref(null);
const error = ref(null);

const fetchNews = async () => {
  try {
    const response = await fetch('https://ohk-test.g.kuroco.app/rcms-api/5/news/3');
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    news.value = await response.json();
  } catch (err) {
    error.value = err;
  }
};
console.log(news)

// Fetch news data when the component is mounted
fetchNews();
</script>

<style>
body {
  margin: 0;
  font-size: 1em;
  line-height: 1.5;
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