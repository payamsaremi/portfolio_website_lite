<template>
  <div class="main">
  <div class="cards-container">
    <div v-for="post in posts" :key="post.slug">
        <card :link="`${post.dir.toLowerCase()}/${post.slug}`" :data-image="getThumbler(post.slug)">
          <h3 slot="category">{{ post.dir.toUpperCase().slice(1) }}</h3>
          <h1 slot="header">{{ post.title }}</h1>
          <p slot="content">{{ post.description }}</p>
        </card>
    </div>
  </div>
  <div class="nav-buttons">
      <NavigationButton back="true"/>
  </div>
  </div>
</template>
       
<script>
export default {
  //transition: 'slide-fade',
  data(){
    return {}
  },
  async asyncData({ $content }) {
    const posts = await $content("blog").fetch();
    return { posts }
  },
  mounted(){
    // this.getThumbler()
  },
  computed: {
    filterByCategories(){
      return this.data.filter( post => post.category.slug.toLowerCase() === this.slug.toLowerCase())
    }
  },
  methods: {
    getThumbler(id){
      for(let item in this.data){
        if(this.data[item].images.length && this.data[item].id === id){
          return this.data[item].images[0].image.medium_square_crop
        }
      }
    }
  }
}
</script>

<style scoped>
.nav-buttons {
  padding: 1rem 0 2rem 0;

  text-align: center;
}
</style>