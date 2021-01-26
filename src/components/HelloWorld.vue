<template>
  <div className="row">
      <div className="col mh-500">
        <swiper
          :space-between="0"
          :slides-per-view="1"
          @slideChange="(e) => console.log(e)"
          @onSwiper="(swiper) => console.log(swiper)"
        >
          <swiper-slide v-for="(image, i) in allImages" :key="'gallery-swiper-' + i"><img :src="image"/></swiper-slide>
        </swiper>
      </div>
      <div class="row d-flex flex-wrap">
        <img v-for="(image, i) in allImages" :src="image" :key="'gallery-thumb-' + i" @click="() => thumbnailClicked(i)" />
      </div>
    </div>
</template>

<script lang="ts">
// @ts-ignore
import { Swiper, SwiperSlide } from 'swiper/vue'
import { defineComponent } from 'vue'
import 'swiper/swiper.scss'

type Image = {
  src: string;
}

export default defineComponent({
  name: 'Gallery',
  props: {
    images: Object as () => Image[]
  },
  data () {
    return {
      allImages: this.images
    }
  },
  methods: {
    thumbnailClicked (index: number) {
      console.log(index)
    }
  },
  onMounted () {
    console.log(this.$props)
  },
  components: {
    Swiper,
    SwiperSlide
  }
})
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
