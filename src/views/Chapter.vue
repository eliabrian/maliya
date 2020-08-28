<template>
  <div id="chapter" class="container my-4">
    <div v-if="loading" class="text-center">
      <Loading></Loading>
    </div>
    <section class="chapter" v-else>
      <div class="text-center">
        <h4>{{ results.title }}</h4>
      </div>
      <div class="header mb-3">
        <b-button
          variant="primary"
          class="mr-2"
          v-if="results.prev"
          :href="'/chapter/' + results.prev"
        >Prev</b-button>
        <div v-else></div>
        <b-button
          variant="primary"
          class="mr-2"
          v-if="results.next"
          :href="'/chapter/' + results.next"
        >Next</b-button>
        <div v-else></div>
      </div>
      <div class="chapter-image my-3">
        <b-img-lazy
          v-bind="mainProps"
          v-for="image in results.images"
          :key="image.image"
          :src="image.image"
          alt="Image 1"
        ></b-img-lazy>
      </div>
      <div class="header">
        <b-button
          variant="primary"
          class="mr-2"
          v-if="results.prev"
          :href="'/chapter/' + results.prev"
        >Prev</b-button>
        <div v-else></div>
        <b-button
          variant="primary"
          class="mr-2"
          v-if="results.next"
          :href="'/chapter/' + results.next"
        >Next</b-button>
        <div v-else></div>
      </div>
    </section>
  </div>
</template>

<script>
import axios from "axios";
import Loading from "../components/Loading";

export default {
  props: ["slug"],
  components: {
    Loading,
  },
  created() {
    this.getChapters();
  },

  data() {
    return {
      loading: false,
      results: [],
      items: [
        {
          text: "Home",
          href: "/",
        },
        {
          text: "1-nen A-gumi no Monster",
          href: "/manga",
        },
        {
          text: "Chapter 1",
          active: true,
        },
      ],
      mainProps: {
        center: true,
        fluidGrow: true,
        blank: true,
        blankColor: "#bbb",
      },
    };
  },

  methods: {
    getChapters() {
      this.loading = true;
      axios
        .get(`https://pure-brook-81217.herokuapp.com/api/chapter/${this.slug}`)
        .then((response) => {
          this.results = response.data.data[0];
        })
        .catch((error) => {
          console.log(error);
        })
        .finally(() => {
          this.loading = false;
        });
    },
  },
};
</script>

<style lang="scss">
.header {
  display: flex;
  justify-content: space-between;
}

.chapter-image {
  max-width: 720px;
  margin-right: auto;
  margin-left: auto;
}
</style>