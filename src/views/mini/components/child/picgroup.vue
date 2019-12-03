<template>
  <div class="pic-group" :style="{padding: `${option.padtb}px ${option.padlf}px`, backgroundColor: option.backgroundColor}">
    <div v-for="(item, index) in option.list" :key="index">
      <el-image class="pic" :src="item.image || url" fit="cover" />
    </div>
  </div>
</template>

<script>
export default {
  name: 'Picgroup',
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

<style lang="scss">
.pic-group {
    .pic {
        height: 200px;width: 100%;display:block;
    }
}
</style>
