<template>
  <div class="main-container">
    <ul>
      <AppCard v-for="image in images" v-bind:key="image.id" :image="image"/>
    </ul>
    <div class="toolbar">
      <button @click="getImages()">Generate</button>
    </div>
  </div>
</template>

<script>
import AppCard from "@/components/AppCard";

export default {
  name: 'AppGallery',
  components: {AppCard},
  data() {
    return {
      images: [],
      currentPage: 1,
      imagesPerPage: 6,
      get fetchNextUrl() {
        return `https://picsum.photos/v2/list?page=${this.currentPage}&limit=${this.imagesPerPage}`;
      },
    }
  },

  mounted() {
    this.getImages()
  },
  methods: {
    async getImages() {
      try {

        const blob = await fetch(this.fetchNextUrl);
        const images = await blob.json();
        const mappedImages = images.map(image => {
          return {
            url: `https://picsum.photos/id/${image.id}/600/600`,
            alt: image.author
          };
        })
        if (images) {
          this.images.length = 0
          this.images.push(...mappedImages)
          this.currentPage++;
        }
      } catch (err) {
        console.log(err);
      }

    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.main-container {
  max-width: 800px;
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 0 auto;
}

@media (min-width: 600px) {
  ul {
    grid-template-columns: 1fr 1fr 1fr;
    column-gap: 12px;
  }
}

ul {
  display: grid;
  row-gap: 12px;
  list-style: none;
  padding: 0;
  width: 100%;
}

.toolbar {
  margin: 70px auto;
  padding: 70px;
  border-top: 1px solid #dadada;
  width: auto;

}

button {
  padding: 10px 20px;
  border: transparent 1px solid;
  border-radius: 4px;
  background-color: #00aaee;
  color: white;
  box-shadow: inset 2px 2px 4px rgba(255, 255, 255, 0.4), inset -2px -2px 4px rgba(55, 55, 55, 0.4);
  transition: all 0.3s;
  cursor: pointer;
}

button:hover {
  background-color: #0066aa;
  color: white;
  box-shadow: inset 0 0 0 rgba(255, 255, 255, 0), inset 0 0 0 rgba(55, 55, 55, 0);
}

button:active {
  background-color: #004488;
  color: white;
  box-shadow: inset -2px -2px 4px rgba(255, 255, 255, 0.4), inset 2px 2px 4px rgba(55, 55, 55, 0.4);
}
</style>
