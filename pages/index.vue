<template>
  <div class="page-component">
    <hr />
    <!-- HEADER-IMAGE -->
    <div v-if="page.fields.image" class="header-image" :style="{ 'background-image': 'url(' + page.fields.image.fields.file.url + ')' }"></div>
    <h1>{{ page.fields.heading }}</h1>

    <img
      v-if="page.fields.image"
      :src="page.fields.image.fields.file.url"
      :alt="page.fields.heading"
    />
    <p
      v-for="(paragraphItem, index) in page.fields.fullText.content"
      :key="index"
    >
      {{ paragraphItem.content[0].value }}
    </p>
    <!-- <p>{{page.fields.fullText}}</p> -->
  </div>
</template>

<script>
import { createClient } from "../plugins/contentful.js";
const contentfulClient = createClient();
export default {
  name: "index",
  asyncData({ env, params }) {
    return contentfulClient
      .getEntries({
        content_type: "page",
        //"fields.slug": "",
      })
      .then((page) => {
        let index = null;

        page.items.forEach((e) => {
          if (e.fields.slug === undefined) {
            index = e;
          }
        });
        return {
          page: index,
        };
      })
      .catch(console.error);
  },
};
</script>

<style>
/* Sample `apply` at-rules with Tailwind CSS
.container {
@apply min-h-screen flex justify-center items-center text-center mx-auto;
}
*/
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont,
    "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}

.header-image {
  /* Full height */
  height: 100vh;
  widows: 100vw;

  /* Center and scale the image nicely */
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}
</style>
