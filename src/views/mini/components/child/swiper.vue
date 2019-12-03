<template>
  <el-carousel arrow="never" height="200px" :interval="option.duration">
    <el-carousel-item v-for="(item, index) in option.list" :key="index">
      <el-image
        :src="item.image || url"
        style="height:100%;display:block;"
        fit="cover"
      />
    </el-carousel-item>
  </el-carousel>
</template>

<script>
export default {
  name: 'Swiper',
  props: {
    option: {
      type: Object,
      default: () => {}
    }
  },
  data() {
    return {
      url: require('@/assets/cover.png')
    }
  }
}
</script>
