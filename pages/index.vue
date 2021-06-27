<template>
  <div class="page-component">
    <!-- <a @click="$router.go(-1)">Go back to overview</a> -->
    <hr />
    <h1>{{ page.fields.heading }}</h1>
    <p> {{ process.env.CTF_CDA_ACCESS_TOKEN }}</p>
    <img
      :src="page.fields.image.fields.file.url"
      :alt="page.fields.heading"
      v-if="page.fields.image"
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
    asyncData({ env, params }) {
    return contentfulClient
      .getEntries({
        content_type: "page",
        "fields.slug": "",
      })
      .then((page) => {
        return {
          page: page.items[0],
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
</style>
