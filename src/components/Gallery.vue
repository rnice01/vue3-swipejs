<template>
  <div className="gallery">
      <div className="gallery__slider mh-500">
        <div id="slider" class="swipe">
          <div class="swipe-wrap">
            <div v-for="(image, i) in allImages" :key="'gallery-swiper-' + i"><img :src="image.src"/></div>
          </div>
        </div>
      </div>
      <div class="gallery__thumbnails">
        <div v-for="(image, i) in allImages" :key="'gallery-thumb-' + i" >
          <img
          :class="state.currentIndex === i ? 'active': ''"
          :src="image.src"
          @click="() => thumbnailClicked(i)" />
        </div>
      </div>
    </div>
</template>

<script lang="ts">
import Swipe from 'swipejs'
import { defineComponent, onMounted, ref, reactive } from 'vue'
import { SliderSrc } from '../@types/types'

type GalleryState = {
  currentIndex: number;
}

export default defineComponent({
  name: 'Gallery',
  props: {
    images: Object as () => SliderSrc[],
    autoplay: {
      type: Boolean,
      default: false
    },
    continuousScroll: {
      type: Boolean,
      default: false
    }
  },
  setup (props) {
    const sliderRef = ref<Swipe>()
    const state = reactive<GalleryState>({
      currentIndex: 0
    })
    onMounted(() => {
      const sliderEl = document.getElementById('slider') as HTMLElement
      if (sliderEl) {
        sliderRef.value = new Swipe(sliderEl, {
          startSlide: 0,
          auto: props.autoplay ? 2000 : 0,
          draggable: true,
          autoRestart: false,
          continuous: props.continuousScroll,
          disableScroll: true,
          stopPropagation: true,
          callback: (index, element) => {

          },
          transitionEnd: (index, element) => {
            state.currentIndex = index
          }
        })
      }
    })
    const thumbnailClicked = (index: number) => {
      console.log(index)
    }
    return {
      allImages: props.images,
      thumbnailClicked,
      state
    }
  }
})
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  .swipe {
    overflow: hidden;
    visibility: hidden;
    position: relative;
  }
  .swipe-wrap {
    overflow: hidden;
    position: relative;
  }
  .swipe-wrap > div {
    float: left;
    width: 100%;
    position: relative;
  }

  .gallery {
    &__thumbnails {
      display: flex;
      justify-content: space-between;
      & > div > img {
        height: 150px;
        width: 150px;
      }
    }
  }
</style>
