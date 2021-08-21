<template>
  <div class="page-component">
    <hr />

    <!-- HEADER-IMAGE -->
    <div v-if="page.fields.image" class="header-wrapper">
      <div
        class="header"
        :style="{
          'background-image': 'url(' + page.fields.image.fields.file.url + ')',
        }"
      ></div>
    </div>

    <!-- HEADLINE HANDLER -->

    <div
      v-if="page.fields.content[0].sys.contentType.sys.id === 'headline'"
      class="headline"
    >
      <p class="title">{{ page.fields.content[0].fields.headline }}</p>

      <div class="paragraph">
        <p
          v-for="(paragraphItem, i) in page.fields.content[0].fields.description
            .content"
          :key="i"
        >
          {{
            page.fields.content[0].fields.description.content[i].content[0]
              .value
          }}
        </p>
      </div>
    </div>

    <h1>{{ page.fields.heading }}</h1>

    <!-- CONTENT -->

    <section class="content">
      <div v-for="(e, index) in page.fields.content" :key="index">
        <!-- WRAPPER -->
        <div
          v-if="page.fields.content[index].sys.contentType.sys.id === 'wrapper'"
          class="wrapper"
        >
          <div
            v-for="(e, i) in page.fields.content[index].fields.content"
            :key="i"
          >
            <!-- HEADLINE HANDLER -->
            <div
              v-if="
                page.fields.content[index].fields.content[i].sys.contentType.sys
                  .id === 'headline'
              "
            >
              <p class="title">
                {{
                  page.fields.content[index].fields.content[i].fields.headline
                }}
              </p>

              <div class="paragraph">
                <p
                  v-for="(paragraphItem, d) in page.fields.content[index].fields
                    .content[i].fields.description.content"
                  :key="d"
                >
                  {{
                    page.fields.content[index].fields.content[i].fields
                      .description.content[d].content[0].value
                  }}
                </p>
              </div>
            </div>

            <!-- IMAGE HANDLER -->
            <div
              v-if="
                page.fields.content[index].fields.content[i].sys.contentType.sys
                  .id === 'imageCard'
              "
            >
              <img
                :src="
                  page.fields.content[index].fields.content[i].fields.image
                    .fields.file.url
                "
              />
            </div>
          </div>
        </div>

        <!-- IMAGE-CARD-WRAPPER -->
        <div
          v-if="
            page.fields.content[index].sys.contentType.sys.id ===
            'imageCardWrapper'
          "
          class="imageCardWrapper"
        >
          <!-- IMAGE HANDLER -->
          <div
            v-for="(element, n) in page.fields.content[index].fields.imageCards"
            :key="n"
            class="imageCard"
          >
            <nuxt-link
              v-if="page.fields.content"
              :to="`/${page.fields.content[index].fields.imageCards[n].fields.link}`"
            >
              <img
                :src="
                  page.fields.content[index].fields.imageCards[n].fields.image
                    .fields.file.url
                "
              />
            </nuxt-link>
          </div>
        </div>
      </div>
    </section>

    <!-- IMAGE-CARDS -->

    <!-- <img
      v-if="page.fields.image"
      :src="page.fields.image.fields.file.url"
      :alt="page.fields.heading"
    /> -->

    <!-- PARAGRAPH -->

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
        console.log(
          page.items[0].fields.content[1].fields.content[0].fields.description
            .content[0].content
        );

        let index = null;

        page.items.forEach((e) => {
          if (e.fields.slug === undefined) {
            index = e;
          }
        });

        return {
          page: index,
          content: page.items[0].fields.content,
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

.page-component {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.wrapper {
  width: 95vw;
  margin: 1rem;
}

.wrapper img {
  width: 95vw;
  height: 80vh;
  object-fit: cover;
  margin-bottom: 1rem;
}

.imageCardWrapper {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.imageCard img {
  width: 300px;
  height: 400px;
  object-fit: cover;
  margin: 1rem;
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

.header-wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100vw;
  height: 100vh;
}

.header {
  /* Full height */
  height: 90vh;
  width: 95vw;

  /* Center and scale the image nicely */
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}
</style>
