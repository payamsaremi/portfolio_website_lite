<template>
  <div>
    <a @mouseenter.prevent="setLightBox" :href="fullSizeImage" class="glightbox">
        <figure>
        <!-- <img :src="image" class="image" /> -->
        <!-- {{image}} -->
        <div @mouseover="itsHover = !itsHover" @mouseout="itsHover = !itsHover">
            <cld-image v-if="itsHover" :publicId="image" height="400" width="400" crop="crop" class="image"/>
            <cld-image v-if="!itsHover" :publicId="image" height="400" width="400" crop="fill" class="image"/>
        </div>
        </figure>
        
    </a>
  </div>
</template>

<script>
import GLightbox from 'glightbox'
export default {
    props:['image'],
    data(){
        return{
            lightbox: '',
            itsHover: false
        }
    },
    methods: {
        setLightBox(){
            this.lightbox = GLightbox({
                touchNavigation: true,
                loop: false,
                openEffect: 'fade',
                closeEffect: 'fade',
                slideEffect: 'slide'
            })
        },
        toggleLightBox(){
            this.lightbox.open()
        }
  },
  computed: {
    fullSizeImage() {
      return this.$cloudinary.image.url(
        this.image,
        {
          gravity: 'auto:subject',
        //   width: 'auto',
          height: '1400',
          crop: 'fill',
        }
      )
    }
  }
}
</script>

<style>
.image {
    padding: 1rem;
    width: 300px;
    height: 300px;
    object-fit: cover;
}
@media only screen and (max-width: 750px) {
 .image {
    width: 100%;
    height: 100%;
    object-fit: cover;
}
}

/* overwriting the initial overflow */
/* .glightbox-open {
    overflow:hidden
} */
</style>