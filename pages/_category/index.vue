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
      <div v-for="post in local_posts" :key="post.id">
        <card :link="post.link" data-image="">
          <h3 slot="category">{{ post.category.toUpperCase() }}</h3>
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
    return {
      local_posts: [
        {id:0,category:'art', title:'Perlin Flowers', description:'Experimentation with Perlin Noise and javascript', image:'',link:'art/noisy' }
      ]
    }
  },
  async asyncData({ $content }) {
    const posts = await $content("blog").fetch();
    return { posts }
  },
  mounted(){
    this.getThumbler()
  },
  computed: {
    filterByCategories(){
      return this.data.filter( post => post.category.slug.toLowerCase() === this.slug.toLowerCase())
    }
  },
  methods: {
    getThumbler(id){
      for(let item in this.posts){
        if(this.posts[item].media && this.posts[item].slug === id){
          let url = this.posts[item].media[0]
          let image = this.$cloudinary.image.url(
                      url,
                        {
                          gravity: 'auto:subject',
                          width: 400,
                          height: 400,
                          crop: 'fill',
                        }
                      )
          return image
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