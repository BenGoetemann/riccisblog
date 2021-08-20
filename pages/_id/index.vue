<template>
  <div class="page-component">
    <!-- <a @click="$router.go(-1)">Go back to overview</a> -->
    <hr />
    <h1>{{ page.fields.heading }}</h1>
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
import { createClient } from "../../plugins/contentful";
const contentfulClient = createClient();

export default {
  name: "index",
  asyncData({ env, params }) {
    return contentfulClient
      .getEntries({
        content_type: "page",
        "fields.slug": params.id,
      })
      .then((page) => {
            console.log(params.id)
        return {
          page: page.items[0],
        };
      })
      .catch(console.error);

  }
};
</script>