<template>
  <div id="manga" class="container my-4">
    <div class="row">
      <b-col cols="12" md="3">
        <div class="p-2 bg-light shadow-sm rounded">
          <div v-if="loading" class="text-center">
            <Loading></Loading>
          </div>
          <div class="manga-summary mb-3" v-else>
            <b-img-lazy
              v-bind="mainProps"
              rounded
              :src="details.thumbnail"
              :alt="details.title"
              fluid
            ></b-img-lazy>
            <div class="manga-title my-3">
              <h6 class="text-success text-center">{{details.title}}</h6>
              <p class="small text-center">{{details.alternatives}}</p>
            </div>
            <b-button
              variant="primary"
              class="mr-2"
              block
              :to="'/chapter/' + details.chapters[details.chapters.length - 1].slug"
            >Read first</b-button>
            <b-button
              block
              variant="secondary"
              :to="'/chapter/' + details.chapters[0].slug"
            >Read last</b-button>
          </div>
        </div>

        <div class="p-2 mt-2 bg-light shadow-sm my-3 rounded">
          <div v-if="loading" class="text-center">
            <Loading></Loading>
          </div>
          <div class="manga-genres" v-else>
            <h6>Genres</h6>
            <b-badge
              class="mr-2"
              variant="warning"
              v-for="genre in details.genres"
              :key="genre.slug"
            >{{ genre.genre }}</b-badge>
          </div>
        </div>

        <div class="p-2 mt-2 bg-light shadow-sm my-3 rounded">
          <div v-if="loading" class="text-center">
            <Loading></Loading>
          </div>
          <div class="manga-details" v-else>
            <h6>Details</h6>
            <div class="clearfix small mb-1">
              <strong class="float-left">Status:</strong>
              <span class="float-right">{{details.status}}</span>
            </div>
            <div class="clearfix small mb-1">
              <p class="float-left">Updated On:</p>
              <span class="float-right">{{details.updated}}</span>
            </div>
          </div>
        </div>
      </b-col>
      <b-col cols="12" md="9">
        <div class="p-2 bg-light shadow-sm rounded">
          <div v-if="loading" class="text-center">
            <Loading></Loading>
          </div>
          <div class="manga-desc" v-else>
            <h6>Description</h6>
            <p class="text-muted m-0">{{ details.description }}</p>
          </div>
        </div>
        <div class="p-2 mt-2 bg-light shadow-sm my-3 rounded">
          <div v-if="loading" class="text-center">
            <Loading></Loading>
          </div>
          <div class="manga-chapters" v-else>
            <h6>Chapters</h6>
            <div class="chapters-table overflow-auto" style="max-height:720px">
              <table class="table table-hover">
                <thead>
                  <tr>
                    <th scope="col">Name</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="chapter in details.chapters" :key="chapter.id">
                    <th scope="row">
                      <a :href="`/chapter/${chapter.slug}`">{{chapter.name}}</a>
                    </th>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>
      </b-col>
    </div>

    <!-- <div v-if="loading" class="text-center">
      <Loading></Loading>
    </div>-->
    <!-- <div v-else>
      <section class="manga-summary">
        <div class="manga-title">
          <h3 class="text-success">{{details.title}}</h3>
        </div>
        <div class="manga-details pt-4">
          <div class="row">
            <b-col cols="12" md="3">
              <b-img-lazy
                v-bind="mainProps"
                rounded
                :src="details.thumbnail"
                :alt="details.title"
                fluid
              ></b-img-lazy>
            </b-col>
            <b-col cols="12" md="9">
              <dl>
                <dt>Alternatives</dt>
                <dd>{{ details.alternatives }}</dd>
                <dt>Author</dt>
                <dd>{{ details.author }}</dd>
                <dt>Updated On</dt>
                <dd>{{ details.updated }}</dd>
                <dt>Status</dt>
                <dd>{{ details.status }}</dd>
                <dt>Genres</dt>
                <dd>
                  <span v-for="genre in details.genres" :key="genre.slug">{{ genre.genre }},</span>
                </dd>
              </dl>
              <b-button
                variant="primary"
                class="mr-2"
                :to="'/chapter/' + details.chapters[details.chapters.length - 1].slug"
              >Read first</b-button>
              <b-button variant="secondary" :to="'/chapter/' + details.chapters[0].slug">Read last</b-button>
            </b-col>
          </div>
        </div>
      </section>
      <section class="manga-description py-4">
        <h5>Description</h5>
        <hr />
        <p class="text-muted">{{ details.description }}</p>
      </section>
      <section class="manga-chapters">
        <h5>Chapters</h5>
        <div class="chapters-table overflow-auto" style="max-height:638px">
          <table class="table table-hover">
            <thead>
              <tr>
                <th scope="col">Name</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="chapter in details.chapters" :key="chapter.id">
                <th scope="row">
                  <a :href="`/chapter/${chapter.slug}`">{{chapter.name}}</a>
                </th>
              </tr>
            </tbody>
          </table>
        </div>
      </section>
    </div>-->
  </div>
</template>

<script>
import axios from "axios";
import Loading from "../components/Loading";

export default {
  props: ["slug"],

  created() {
    this.getDetails();
  },

  components: {
    Loading,
  },

  data() {
    return {
      loading: false,
      details: "",
      mainProps: {
        center: true,
        fluidGrow: true,
        blank: true,
        blankColor: "#bbb",
      },
    };
  },

  methods: {
    getDetails() {
      this.loading = true;
      axios
        .get(`https://pure-brook-81217.herokuapp.com/api/manga/${this.slug}`)
        .then((response) => {
          this.details = response.data.data[0];
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
@import "../assets/scss/main.scss";
</style>