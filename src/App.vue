<template>
  <div class="">
    <HelloWorld :images="images" :autoPlay="slideSettings.autoplay" :continuousScroll="slideSettings.continuousScroll" />
    <div class="">
      <div>
        <label for="autoplay-check">Autoplay Slides</label>
        <input id="autoplay-check" type="checkbox" @change="(e) => handleSettingsChange({autoplay: e.target.checked})" />
      </div>
      <div>
        <label for="loop-check">Continuously Loop Slides</label>
        <input id="loop-check" type="checkbox" @change="(e) => handleSettingsChange({continuousScroll: e.target.checked})" />
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive } from 'vue'
import HelloWorld from './components/Gallery.vue'
import { SliderSrc, SliderSrcType } from './@types/types'
import './styles/styles.scss'

export default defineComponent({
  name: 'App',
  setup () {
    const imageSrcs = [
      'https://place-hold.it/100',
      'https://place-hold.it/200',
      'https://place-hold.it/300',
      'https://place-hold.it/400'
    ]

    const images = imageSrcs.map((src): SliderSrc => {
      return {
        src,
        type: SliderSrcType.Image
      }
    })

    let slideSettings = reactive({
      autoplay: false,
      continuousScroll: false
    })

    const handleSettingsChange = (settingUpdate: Record<string, boolean>) => {
      slideSettings = {
        ...slideSettings,
        ...settingUpdate
      }
    }

    return {
      images,
      slideSettings,
      handleSettingsChange
    }
  },
  components: {
    HelloWorld
  }
})
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
