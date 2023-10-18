<script>
export default {
  data() {
    return {
      slides: [
        'https://images.unsplash.com/photo-1697403704196-e08fb51fc0cd?auto=format&fit=crop&q=80&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&w=2487',
        'https://images.unsplash.com/photo-1517784541475-54f13a036167?auto=format&fit=crop&q=80&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&w=2302',
        'https://images.unsplash.com/photo-1520637438573-ee1ba80b2a7f?auto=format&fit=crop&q=80&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&w=2340'
      ],
      dots: [
        './assets/dots=activeFirstSlide.svg',
        './assets/dots=activeSecondSlide.svg',
        './assets/dots=activeThirdSlide.svg'
      ],
      currentIndex: 0,
      activeIndex: 0,
      showThumbnails: true,
      imageFullScreen: false
    };
  },
  computed: {
    currentSlide() {
      return this.slides[this.currentIndex];
    }
  },
  methods: {
    prevSlide() {
      if (this.currentIndex === 0) {
        this.currentIndex = this.slides.length - 1;
      } else {
        this.currentIndex--;
      }
      this.activeIndex = this.currentIndex;
    },
    nextSlide() {
      if (this.currentIndex === this.slides.length - 1) {
        this.currentIndex = 0;
      } else {
        this.currentIndex++;
      }
      this.activeIndex = this.currentIndex;
    },
    toggleThumbnails() {
      // this.showThumbnails = !this.showThumbnails;
    },
    openFullScreenImage() {
        this.imageFullScreen = true;
    }
  },
  mounted() {
    setInterval(() => { this.nextSlide(); }, 30000);
  },
  setCurrentSlide(index) {
    this.currentIndex = index;
    this.activeIndex = index;
  }
}
</script>

<template>
  <div class="slider">
    <!-- <div class="slider__img-box" v-if="!showThumbnails" >
      <button @click="prevSlide" class="slider__button slider__button_type_prev"></button>
      <img :src="currentSlide" class="slider__img" :key="currentIndex" @click="toggleThumbnails"
        @dblclick="addImageBorder">
      <button @click="nextSlide" class="slider__button slider__button_type_next"></button>
    </div>
    <div class="slider__dots" v-if="!showThumbnails" >
      <span v-for="(slide, index) in slides" :key="index" class="slider__dot"
        :class="{ 'slider__dot_active': index === currentIndex }"></span>
    </div> -->

    <div class="slider__one-click">
      <button @click="prevSlide" class="slider__button slider__button_type_prev"></button>
      <img :src="currentSlide" class="slider__img slider__img_size_medium" :key="currentIndex"
      @dblclick="openFullScreenImage" :class="{ 'slider__img_size_full': imageFullScreen }">
      <button @click="nextSlide" class="slider__button slider__button_type_next"></button>
      <div v-if="showThumbnails" class="slider__thumbnails">
        <img v-for="(slide, index) in slides" :key="index" :src="slide"
          :class="{ 'slider__thumbnail_active': index === activeIndex }" @click="setCurrentSlide(index)"
          class="slider__thumbnail-img" />
      </div>
    </div>


  </div>
</template>

<style scoped>
/* *,*::before,*::after {outline: 1px solid red} */
</style>


<!-- :class="{ 'slider__img-bordered': imageBorder }" -->