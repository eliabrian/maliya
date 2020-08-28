<template>
  <div id="app">
    <header id="nav" class="shadow-sm sticky-top">
      <div class="container">
        <b-navbar toggleable="lg" type="light">
          <b-navbar-brand href="/">Malya</b-navbar-brand>

          <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

          <b-collapse id="nav-collapse" is-nav>
            <!-- Right aligned nav items -->
            <b-navbar-nav class="ml-auto">
              <b-navbar-nav>
                <b-nav-item to="/">Home</b-nav-item>
                <b-nav-item to="/discover">Discover</b-nav-item>
                <b-nav-item to="/list">List</b-nav-item>
                <b-nav-item v-b-modal.modal-prevent-closing>Search</b-nav-item>
                <b-modal
                  id="modal-prevent-closing"
                  ref="modal"
                  title="Submit Your Name"
                  @show="resetModal"
                  @hidden="resetModal"
                  @ok="handleOk"
                >
                  <form ref="form" @submit.stop.prevent="handleSubmit">
                    <b-form-group
                      :state="nameState"
                      label="Name"
                      label-for="name-input"
                      invalid-feedback="Name is required"
                    >
                      <b-form-input id="name-input" v-model="name" :state="nameState" required></b-form-input>
                    </b-form-group>
                  </form>
                </b-modal>
              </b-navbar-nav>
            </b-navbar-nav>
          </b-collapse>
        </b-navbar>
      </div>
    </header>
    <router-view />
  </div>
</template>

<style lang="scss">
@import "./assets/scss/main.scss";

body {
  background-color: $bg-color;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

#nav {
  padding: 1rem;
  background-color: $light;
  a {
    font-weight: bold;
    color: $dark;

    &.router-link-exact-active {
      color: $primary;
    }
  }
}

.navbar {
  padding: 0;
}
</style>


<script>
export default {
  data() {
    return {
      name: "",
      nameState: null,
      submittedNames: [],
    };
  },
  methods: {
    checkFormValidity() {
      const valid = this.$refs.form.checkValidity();
      this.nameState = valid;
      return valid;
    },
    resetModal() {
      this.name = "";
      this.nameState = null;
    },
    handleOk(bvModalEvt) {
      // Prevent modal from closing
      bvModalEvt.preventDefault();
      // Trigger submit handler
      this.handleSubmit();
    },
    handleSubmit() {
      // Exit when the form isn't valid
      if (!this.checkFormValidity()) {
        return;
      }
      // Push the name to submitted names
      this.submittedNames.push(this.name);
      // Hide the modal manually
      this.$nextTick(() => {
        this.$bvModal.hide("modal-prevent-closing");
      });
    },
  },
};
</script>