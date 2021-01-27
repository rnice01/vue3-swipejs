<template>
  <div class="gallery__thumbnails">
    <div>
      <div v-for="(thumb, i) in thumbs" :key="'gallery-thumb-' + i" >
        <img
        :class="currentIndex === i ? 'active': ''"
        :src="thumb.src"
        @click="() => handleThumbnailClicked(i)" />
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  props: {
    thumbs: {
      type: Array.of(String),
      required: true
    },
    currentIndex: {
      type: Number,
      required: true
    }
  },
  methods: {
    handleThumbnailClicked (index: number) {
      this.$emit('thumbnail-clicked', index)
    }
  }
})
</script>

<style scoped lang="scss">
  .gallery__thumbnails {
    width: 100%;
    display: flex;
    justify-content: center;
    & > div {
      width: 75%;
      display: flex;
      justify-content: space-evenly;
      & > div > img {
        height: 50px;
        width: 50px;
      }
    }
  }
</style>
