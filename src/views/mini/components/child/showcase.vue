<template>
  <div class="showcase" :style="{backgroundColor: option.backgroundColor, padding:`${option.padlf}px ${option.padtb}px`}">
    <div v-if="option.layout.type !== 4" :class="{'three' : option.layout.type === 2, 'four': option.layout.type === 3}">
      <el-image v-for="(item, index) in option.list" :key="index" class="case" :src="item.image | img(index)" />
    </div>
    <div v-else class="show">
      <div class="left">
        <el-image class="case" :src="require('@/assets/01.jpg')" fit="cover" />
      </div>
      <div class="right">
        <div v-if="option.list[1] !== void 0" class="top-img"><el-image class="case" :src="option.list[1].image || require('@/assets/02.jpg')" fit="cover" /></div>
        <div class="bot-img">
          <el-image v-if="option.list[2] !== void 0" class="case" :src="option.list[2].image || require('@/assets/03.jpg')" fit="cover" />
          <el-image v-if="option.list[3] !== void 0" class="case" :src="option.list[3].image || require('@/assets/04.jpg')" fit="cover" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Showcase',
  filters: {
    img(image, index) {
      const idx = index < 4 ? index + 1 : !(index % 4) ? 1 : index % 4 + 1
      return image || require(`@/assets/0${idx}.jpg`)
    }
  },
  props: {
    option: {
      type: Object,
      required: true
    }
  }
}
</script>

<style lang="scss">
.showcase{
    & > div {
        display: flex;
        flex-wrap: wrap;
        .case {
            flex-basis: 50%;
            display: block;
        }
        &.three {
            .case {
                flex-basis: 33.33333%;
            }
        }
        &.four {
            .case {
                flex-basis: 25%;
            }
        }
        &.show {
            flex-wrap: nowrap;
            position: relative;
            .left {flex-basis: 50%}
            .right {
                width: 50%;
                right: 0;
                display: flex;
                flex-direction: column;
                position: absolute;
                height: 100%;
                overflow: hidden;
                & > div {
                    flex: 1;
                    &.top-img{
                        height: 50%;
                        .case {
                            height: 100%;
                        }
                    }
                    &.bot-img {
                        display: flex;
                    }
                }
            }
        }
    }
}
</style>
