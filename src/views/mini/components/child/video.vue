<template>
  <div class="video" :style="{padding: `${option.padtb}px ${option.padlf}px`}">
    <video :src="require('@/assets/mov_bbb.mp4')" :autoplay="option.video.auto" controls :loop="option.video.loop" />
  </div>
</template>

<script>
export default {
  name: 'Video',
  props: {
    option: {
      type: Object,
      required: true
    }
  }
}
</script>

<style lang="scss">
.video {
    video {
        width: 100%;
        display: block;
    }
}
</style>
