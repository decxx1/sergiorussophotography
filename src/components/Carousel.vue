<template>
    <div class="slider" >
        <TransitionGroup name="fade">
            <img :key="1" v-show="slidePos === 0" class="w-100 full-height superponer" src="../images/author-2.jpg" alt="slide 1"/>
            <img :key="2" v-show="slidePos === 1" class="w-100 full-height superponer" src="../images/slider.jpg" alt="slide 2"/>
        </TransitionGroup>
        <div class="slider__dots" :data-pos="slidePos">
          <a class="slider__indicator" :class="indicatorClass"></a>
          <a @click="handleClickDots(0)" class="slider__dot" data-pos="0"></a>
          <a @click="handleClickDots(1)" class="slider__dot" data-pos="1"></a>
        </div>
    </div>
</template>

<script>
export default{
    data(){
        return{
          slides: 2,
          slidePos: 0,
          delay: 5000,
          indicatorClass: 'slider__indicator--right',
          manualSlide: false
        }
    },
    methods:{
        playSlide(){
            setInterval(() => {
              if(!this.manualSlide){
                this.slidePos++;
                if(this.slidePos > this.slides - 1){
                  this.slidePos = 0;
                }
              }
              this.manualSlide = false;
            }, this.delay);
        },
        handleClickDots(elementPos){
          this.manualSlide = true;
          let currentPos = parseInt(this.slidePos);
          let newPos = parseInt(elementPos);

          let newDirection = newPos > currentPos ? 'right' : 'left';

          this.indicatorClass = `slider__indicator--${newDirection}`;
          this.slidePos = newPos;
        }
    },
    mounted(){
      this.playSlide();
    }
}
</script>

<style scoped>
.zIndex1{
  z-index: 1;
}
.superponer{
  position: absolute;
}
.slider {
  width:50%;
  position:relative;
  bottom: 50%;
}
@media (max-width: 767.98px) {
  .slider {
    width: 30%;
  }
  /* .slider__dots {
    left: 25% !important;
  } */
}
.full-height{
    height: 100vh;
    object-fit: cover;
}
/* 1. declare transition */
.fade-move,
.fade-enter-active,
.fade-leave-active {
  transition: opacity 1.8s ease-in;
  animation: fadeIn 1.8s;
  transform: scale(0.95);
  -webkit-animation: fadeIn 1.8s;
  -moz-animation: fadeIn 1.8s;
  -o-animation: fadeIn 1.8s;
  -ms-animation: fadeIn 1.8s;
}
/* 2. declare enter from and leave to state */
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  
}
/* 3. ensure leaving items are taken out of layout flow so that moving
      animations can be calculated correctly. */
.fade-leave-active {
  position: absolute;
  animation: fadeOut 1s;
  transform: scale(1);
  -webkit-animation: fadeOut 1s;
  -moz-animation: fadeOut 1s;
  -o-animation: fadeOut 1s;
  -ms-animation: fadeOut 1s;
}
@keyframes fadeIn {
  0% { 
    opacity: 0; 
    transform: scale(0.98);
  }
  100% { 
    opacity: 1;
    transform: scale(1); 
  }
}

@-moz-keyframes fadeIn {
  0% { 
    opacity: 0; 
    transform: scale(0.98);
  }
  100% { 
    opacity: 1;
    transform: scale(1); 
  }
}

@-webkit-keyframes fadeIn {
  0% { 
    opacity: 0; 
    transform: scale(0.98);
  }
  100% { 
    opacity: 1;
    transform: scale(1); 
  }
}

@-o-keyframes fadeIn {
  0% { 
    opacity: 0; 
    transform: scale(0.98);
  }
  100% { 
    opacity: 1;
    transform: scale(1); 
  }
}

@-ms-keyframes fadeIn {
  0% { 
    opacity: 0; 
    transform: scale(0.98);
  }
  100% { 
    opacity: 1;
    transform: scale(1); 
  }
}
@keyframes fadeOut {
  0% { 
    opacity: 1; 
  }
  100% { 
    opacity: 0;
  }
}

@-moz-keyframes fadeOut {
  0% { 
    opacity: 1;
  }
  100% { 
    opacity: 0;
  }
}

@-webkit-keyframes fadeOut {
  0% { 
    opacity: 1;
  }
  100% { 
    opacity: 0; 
  }
}

@-o-keyframes fadeOut {
  0% { 
    opacity: 1; 
  }
  100% { 
    opacity: 0;
  }
}

@-ms-keyframes fadeOut {
  0% { 
    opacity: 1; 
  }
  100% { 
    opacity: 0;
  }
}
.slider__dots {
  display: flex;
  position: absolute;
  z-index: 3;
  bottom: -97vh;
  left: 50%;
  transform: translateX(-50%);
}
.slider__dot, .slider__indicator {
  cursor: pointer;
  display: block;
  margin: 0 0.5em;
  width: 1em;
  height: 1em;
  background: rgba(255, 255, 255, 0.5);
  border-radius: 100px;
}
.slider__indicator {
  position: absolute;
  background: white;
  width: 1rem;
}
.slider__indicator--left {
  transition: left 0.3s cubic-bezier(0.51, 0.92, 0.24, 1.15), right 0.3s 0.1s cubic-bezier(0.51, 0.92, 0.24, 1.15);
}
.slider__indicator--right {
  transition: left 0.3s 0.1s cubic-bezier(0.51, 0.92, 0.24, 1.15), right 0.3s cubic-bezier(0.51, 0.92, 0.24, 1.15);
}
.slider__dots[data-pos="0"] .slider__indicator {
  left: 0em;
  right: 6em;
}
.slider__dots[data-pos="1"] .slider__indicator {
  left: 2em;
  right: 4em;
}
.slider__dots[data-pos="2"] .slider__indicator {
  left: 4em;
  right: 2em;
}
</style>