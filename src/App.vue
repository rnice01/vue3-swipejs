<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <HelloWorld :images="images" />
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import HelloWorld from './components/HelloWorld.vue'
import './styles/styles.scss'

export default defineComponent({
  name: 'App',
  setup () {
    const images = [
      'https://place-hold.it/100',
      'https://place-hold.it/200',
      'https://place-hold.it/300',
      'https://place-hold.it/400'
    ]
    return {
      images
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
