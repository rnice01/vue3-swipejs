<template>
  <div className="gallery">
      <div className="gallery__slider mh-500">
        <div id="slider" class="swipe">
          <div class="swipe-wrap">
            <div v-for="(image, i) in state.sources" :key="'gallery-swiper-' + i"><img :src="image.src"/></div>
          </div>
        </div>
        <button
          class="slider__arrow-right"
          type="button"
          @click="() => arrowClicked(1)"
        >Right</button>
        <button
          class="slider__arrow-left"
          type="button"
          @click="() => arrowClicked(-1)"
        >Left</button>
      </div>
      <BottomThumbnails :thumbs="state.sources" :currentIndex="state.currentIndex" @thumbnailClicked="thumbnailClicked" />
    </div>
</template>

<script lang="ts">
import Swipe from 'swipejs'
import { defineComponent, onMounted, ref, reactive } from 'vue'
import { SliderSrc } from '../@types/types'
import BottomThumbnails from './BottomThumbnails.vue'

type GalleryState = {
  currentIndex: number;
  sources: SliderSrc[];
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
      currentIndex: 0,
      sources: props.images || []
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
    const slideTo = (index: number) => {
      if (sliderRef.value) {
        state.currentIndex = index
        sliderRef.value.slide(index, 500)
      }
    }
    const thumbnailClicked = (index: number) => {
      slideTo(index)
    }

    const arrowClicked = (value: number) => {
      const indexTo = value + state.currentIndex
      if (indexTo > state.sources.length - 1) {
        slideTo(0)
      } else if (indexTo < 0) {
        slideTo(state.sources.length - 1)
      } else {
        slideTo(indexTo)
      }
    }

    return {
      allImages: props.images,
      arrowClicked,
      thumbnailClicked,
      state
    }
  },
  components: {
    BottomThumbnails
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

  @mixin slider-arrow {
    top: 50%;
    position: absolute;
  }

  .gallery {
    &__slider {
      position:relative;
      .slider__arrow-left {
        @include slider-arrow;
        left: 0;
      }
      .slider__arrow-right {
        @include slider-arrow;
        right: 0;
      }
    }
  }
</style>
