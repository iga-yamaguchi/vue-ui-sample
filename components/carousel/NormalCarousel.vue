<template>
  <div class="wrap">
    <ul>
      <li
        v-for="(img, index) in imgList"
        :key="index"
        :style="{ left: left(index) }"
      >
        <img :src="img" />
      </li>
    </ul>
    <div @click="prev" class="prev">&lt;</div>
    <div @click="next" class="next">&gt;</div>
  </div>
</template>

<script>
export default {
  name: 'NormalCarousel',
  inject: ['carousel'],
  props: {
    displayCount: {
      type: Number,
      default: 1
    },
    imgList: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      initialWidth: 300,
      addWidth: 0,
      position: 0
    }
  },
  methods: {
    prev() {
      if (this.position <= this.imgList.length - 1) {
        this.position = 0
      } else {
        this.position -= 1
      }
      this.slidePrev()
    },
    next() {
      if (this.position >= this.imgList.length - 1) {
        this.position = 0
      } else {
        this.position += 1
      }
      this.slideNext()
    },
    slidePrev() {
      const intervalId = setInterval(() => {
        if (this.addWidth <= this.initialWidth * this.position) {
          clearInterval(intervalId)
        } else {
          this.addWidth -= 1
        }
      }, 1)
    },
    slideNext() {
      const intervalId = setInterval(() => {
        if (this.addWidth >= this.initialWidth * this.position) {
          clearInterval(intervalId)
        } else {
          this.addWidth += 1
        }
      }, 1)
    },
    left(index) {
      const left = index * this.displayCount * this.initialWidth - this.addWidth
      return `${left}px`
    }
  }
}
</script>

<style scoped>
.wrap {
  position: relative;
}

ul {
  position: relative;
  height: 200px;
  list-style: none;
  overflow: hidden;
}

li {
  width: 100%;
  position: absolute;
}

.prev,
.next {
  display: none;
  position: absolute;
  top: 0;
  width: 40px;
  height: 100%;
  background-color: gray;
  opacity: 0.5;
  user-select: none;
  cursor: pointer;
  font-size: 35px;
}
.wrap:hover .prev,
.wrap:hover .next {
  display: flex;
  justify-content: center;
  align-items: center;
  transition: all 300ms 0s ease;
}

.prev {
  left: 0;
}

.next {
  right: 0;
}
</style>
