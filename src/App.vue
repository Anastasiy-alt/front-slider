<script>
export default {
  data() {
    return {
      slides: [
        'https://images.unsplash.com/photo-1697403704196-e08fb51fc0cd?auto=format&fit=crop&q=80&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&w=2487.jpg',
        'https://images.unsplash.com/photo-1517784541475-54f13a036167?auto=format&fit=crop&q=80&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&w=2302.jpg',
        // 'https://images.unsplash.com/photo-1602575910330-f9807b05f69d?auto=format&fit=crop&q=80&w=2574&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D.jpg',
        // 'https://images.unsplash.com/photo-1547454876-9c75be28f80d?auto=format&fit=crop&q=80&w=2482&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D.jpg',
        'https://images.unsplash.com/photo-1520637438573-ee1ba80b2a7f?auto=format&fit=crop&q=80&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&w=2340.jpg'
      ],
      dots: [
        './assets/dots=activeFirstSlide.svg',
        './assets/dots=activeSecondSlide.svg',
        './assets/dots=activeThirdSlide.svg'
      ],
      currentIndex: 0,
      backCurrentIndex: 0,
      activeIndex: 0,
      showThumbnails: false,
      imageFullScreen: false,
      defaultSlider: true
    };
  },
  computed: {
    currentSlide() {
      return this.slides[this.currentIndex];
    },
    backCurrentSlide() {
      this.backCurrentIndex = this.currentIndex
      if (this.backCurrentIndex === this.slides.length - 1) {
        this.backCurrentIndex = 0;
      }
      return this.slides[this.backCurrentIndex + 1];
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
    openThumbnails() {
      this.showThumbnails = true;
      this.imageFullScreen = false;
      this.defaultSlider = false;
    },
    openFullScreenImage() {
      this.imageFullScreen = true;
      this.showThumbnails = false;
      this.defaultSlider = false;
    },
    openDefaultSlider() {
      this.imageFullScreen = false;
      this.showThumbnails = false;
      this.defaultSlider = true;
    },
    downloadImage() {
      const link = document.createElement('a');
      link.href = this.currentSlide;
      link.download = this.currentSlide;
      link.click();
    },
    mounted() {
      setInterval(() => { this.nextSlide(); }, 300000);
    },
    setCurrentSlide(index) {
      this.currentIndex = index;
      this.activeIndex = index;
    }
  }
}
</script>

<template>
  <div class="slider">
    <div class="slider__img-box" v-if="defaultSlider">
      <button @click="prevSlide" class="slider__button slider__button_type_prev"></button>
      <img :src="currentSlide" class="slider__img" :key="currentIndex" @click="openThumbnails" alt="Картинка.">
      <button @click="nextSlide" class="slider__button slider__button_type_next"></button>
    </div>
    <div class="slider__dots" v-if="defaultSlider">
      <span v-for="(slide, index) in slides" :key="index" class="slider__dot"
        :class="{ 'slider__dot_active': index === currentIndex }"></span>
    </div>

    <div class="slider__one-click" v-if="showThumbnails">
      <button class="slider__save" @click="downloadImage"></button>
      <button class="slider__close" @click="openDefaultSlider"></button>
      <button @click="prevSlide" class="slider__button slider__button_type_prev"></button>
      <img :src="backCurrentSlide" class="slider__img slider__img_size_medium slider__img_back" :key="backCurrentIndex"
        @dblclick="openFullScreenImage" alt="Следующая картинка.">

      <img :src="currentSlide" class="slider__img slider__img_size_medium" :key="currentIndex"
        @dblclick="openFullScreenImage"  alt="Картинка.">
      <button @click="nextSlide" class="slider__button slider__button_type_next"></button>
      <div class="slider__thumbnails">
        <img v-for="(slide, index) in slides" :key="index" :src="slide"
          :class="{ 'slider__thumbnail_active': index === activeIndex }" @click="setCurrentSlide(index)"
          class="slider__thumbnail-img" />
      </div>
    </div>

    <div class="slider__double-click" v-if="imageFullScreen">
      <button class="slider__save" @click="downloadImage"></button>
      <button class="slider__close" @click="openDefaultSlider"></button>
      <button @click="prevSlide" class="slider__button slider__button_type_prev"></button>
      <img :src="currentSlide" class="slider__img slider__img_size_full" :key="currentIndex" @click="openThumbnails">
      <button @click="nextSlide" class="slider__button slider__button_type_next"></button>
    </div>


  </div>
</template>

<style scoped>
/* *,*::before,*::after {outline: 1px solid red} */
</style>


<!-- :class="{ 'slider__img-bordered': imageBorder }" -->