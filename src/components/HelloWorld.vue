<template>
  <v-container>
    <v-row class="my-8">
      <v-col>
        <h2 class="my-4">{{ title }}</h2>
        <form @submit.prevent="fetchGifs">
          <v-text-field
            required
            v-model="search"
            class="pb-4"
            :loading="loading"
            density="compact"
            variant="solo"
            label="Search Gif"
            append-inner-icon="mdi-magnify"
            single-line
            hide-details
            @click:append-inner="fetchGifs"
          ></v-text-field>
          <v-btn type="submit" class="mr-4" color="error">Search</v-btn>
        </form>
      </v-col>
    </v-row>

    <div class="my-4 text-center" v-if="this.loaded">
      <v-pagination v-model="page" :length="6"></v-pagination>
    </div>

    <div style="column-count: 3" v-if="this.loaded">
      <v-row v-for="(image, index) in images" :key="index++">
        <img @click="goToImageUrl" class="pa-2" :src="image" alt="" />
      </v-row>
    </div>
  </v-container>
</template>

<script>
export default {
  data: () => ({
    title: 'Giphy Search App',
    search: '',
    loaded: false,
    loading: false,
    images: [],
    page: 1,
  }),

  methods: {
    async fetchGifs() {
      this.images = [];

      this.loading = true;

      setTimeout(() => {
        this.loading = false;
        this.loaded = true;
      }, 2000);

      try {
        console.log(process.env.VUE_APP_VARIABLE);
        const response = await fetch(
          `https://api.giphy.com/v1/gifs/search?api_key=${process.env.VUE_APP_VARIABLE}&q=${this.search}`
        );
        const {data} = await response.json();
        data.forEach(d => {
          this.images.push(d.images.original.url);
        });
      } catch (err) {
        console.log(err);
      }
    },
    goToImageUrl() {
      console.log('Go image');
    },
  },
};
</script>

<style>
img {
  width: 100%;
}
</style>
