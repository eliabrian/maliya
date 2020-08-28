<template>
  <div id="dicover" class="container my-4">
    <section class="drawer">
      <div class="heading">
        <h2>Popular Isekai Manga</h2>
        <hr />
      </div>

      <div class="content">
        <b-row v-if="loading">
          <b-col cols="12">
            <div class="text-center">
              <Loading></Loading>
            </div>
          </b-col>
        </b-row>
        <b-row v-else>
          <b-col cols="6" md="2" v-for="manga in popularIsekai" :key="manga.id">
            <div class="manga-container">
              <div class="manga-image">
                <b-link :to="`/manga/${manga.slug}`">
                  <b-badge variant="success" class="position-absolute">{{ manga.type }}</b-badge>
                  <b-img-lazy
                    v-bind="mainProps"
                    rounded
                    :src="manga.thumbnail"
                    :alt="manga.title"
                    fluid
                  ></b-img-lazy>
                </b-link>
              </div>
              <div class="manga-summary mt-3">
                <div class="manga-title">
                  <h6 class="m-0">
                    <a :href="`/manga`">
                      <div class="text-truncate">{{ manga.title }}</div>
                    </a>
                  </h6>
                  <b-badge pill variant="warning">{{ manga.newest }}</b-badge>
                </div>
                <div class="manga-rating">
                  <b-form-rating
                    id="rating-inline"
                    class="p-0"
                    inline
                    no-border
                    v-model="manga.rating"
                    readonly
                    size="sm"
                    variant="primary"
                  ></b-form-rating>
                  <label for="rating-inline">{{ manga.value }}</label>
                </div>
              </div>
            </div>
          </b-col>
        </b-row>
      </div>
    </section>

    <section class="drawer">
      <div class="heading">
        <h2>Popular Action Manga</h2>
        <hr />
      </div>

      <div class="content">
        <b-row v-if="loading">
          <b-col cols="12">
            <div class="text-center">
              <Loading></Loading>
            </div>
          </b-col>
        </b-row>
        <b-row v-else>
          <b-col cols="6" md="2" v-for="manga in popularAction" :key="manga.id">
            <div class="manga-container">
              <div class="manga-image">
                <b-link :to="`/manga/${manga.slug}`">
                  <b-badge variant="success" class="position-absolute">{{ manga.type }}</b-badge>
                  <b-img-lazy
                    v-bind="mainProps"
                    rounded
                    :src="manga.thumbnail"
                    :alt="manga.title"
                    fluid
                  ></b-img-lazy>
                </b-link>
              </div>
              <div class="manga-summary mt-3">
                <div class="manga-title">
                  <h6 class="m-0">
                    <a :href="`/manga`">
                      <div class="text-truncate">{{ manga.title }}</div>
                    </a>
                  </h6>
                  <b-badge pill variant="warning">{{ manga.newest }}</b-badge>
                </div>
                <div class="manga-rating">
                  <b-form-rating
                    id="rating-inline"
                    class="p-0"
                    inline
                    no-border
                    v-model="manga.rating"
                    readonly
                    size="sm"
                    variant="primary"
                  ></b-form-rating>
                  <label for="rating-inline">{{ manga.value }}</label>
                </div>
              </div>
            </div>
          </b-col>
        </b-row>
      </div>
    </section>

    <section class="drawer">
      <div class="heading">
        <h2>Popular Mystery Manga</h2>
        <hr />
      </div>

      <div class="content">
        <b-row v-if="loading">
          <b-col cols="12">
            <div class="text-center">
              <Loading></Loading>
            </div>
          </b-col>
        </b-row>
        <b-row v-else>
          <b-col cols="6" md="2" v-for="manga in popularMystery" :key="manga.id">
            <div class="manga-container">
              <div class="manga-image">
                <b-link :to="`/manga/${manga.slug}`">
                  <b-badge variant="success" class="position-absolute">{{ manga.type }}</b-badge>
                  <b-img-lazy
                    v-bind="mainProps"
                    rounded
                    :src="manga.thumbnail"
                    :alt="manga.title"
                    fluid
                  ></b-img-lazy>
                </b-link>
              </div>
              <div class="manga-summary mt-3">
                <div class="manga-title">
                  <h6 class="m-0">
                    <a :href="`/manga`">
                      <div class="text-truncate">{{ manga.title }}</div>
                    </a>
                  </h6>
                  <b-badge pill variant="warning">{{ manga.newest }}</b-badge>
                </div>
                <div class="manga-rating">
                  <b-form-rating
                    id="rating-inline"
                    class="p-0"
                    inline
                    no-border
                    v-model="manga.rating"
                    readonly
                    size="sm"
                    variant="primary"
                  ></b-form-rating>
                  <label for="rating-inline">{{ manga.value }}</label>
                </div>
              </div>
            </div>
          </b-col>
        </b-row>
      </div>
    </section>
  </div>
</template>

<script>
import axios from "axios";
import Loading from "../components/Loading";

export default {
  created() {
    this.getManga(this.order.popular, this.genre.isekai).then((data) => {
      this.popularIsekai = data;
    });
    this.getManga(this.order.popular, this.genre.action).then((data) => {
      this.popularAction = data;
    });
    this.getManga(this.order.popular, this.genre.mystery).then((data) => {
      this.popularMystery = data;
    });
  },

  components: {
    Loading,
  },

  data() {
    return {
      loading: false,
      mainProps: {
        center: true,
        fluidGrow: true,
        blank: true,
        blankColor: "#bbb",
      },
      genre: {
        isekai: "isekai",
        action: "action",
        mystery: "mystery",
      },
      order: {
        popular: "popular",
        update: "update",
        latest: "latest",
      },

      popularIsekai: [],
      popularAction: [],
      popularMystery: [],
    };
  },

  methods: {
    getManga(order, genre) {
      this.loading = true;
      return axios
        .get(
          `https://pure-brook-81217.herokuapp.com/api/genres/${genre}/1/${order}`
        )
        .then((response) => {
          this.loading = false;
          return response.data.data.slice(0, 12);
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
};
</script>


<style lang="scss">
@import "../assets/scss/main.scss";

.content {
  margin-top: 1rem;
}

.manga-container {
  margin-bottom: 1.5rem;

  .manga-image {
    transition: transform 0.25s ease-in-out;
  }

  .manga-image:hover {
    transform: scale(1.1);
  }
}

.manga-summary {
  width: 100%;

  .manga-title {
    a {
      color: black;
      transition: color 0.2s ease-in-out;
    }
  }

  .manga-title {
    a:hover {
      text-decoration: none;
      color: $success;
      cursor: pointer;
    }
  }
}
</style>
