<template>
<div class="carousel-slide">

  <transition :name="direction" mode="in-out">
    <div v-show="visibleSlide === index" class="carousel__slide">
      <slot></slot>
    </div>
  </transition>
</div>
</template>

<script>
export default {
  props: [
    'visibleSlide',
    'index',
    'direction',
  ]
}
</script>

<style lang="scss" scoped>
.carousel{
  position: relative;
  &__slide {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
  }
  &__left {
    // 左から入ってくる
    &-enter-active {
      animation: leftIn 0.3s ease-in-out;
    }
    // 左へ出ていく
    &-leave-active {
      animation: leftOut 0.3s ease-in-out;
    }
  }
  &__right {
    // 右から入ってくる
    &-enter-active {
      animation: rightIn 0.3s ease-in-out;
    }
    // 右へ出ていく
    &-leave-active {
      animation: rightOut 0.3s ease-in-out;
    }
  }
}
@keyframes leftIn {
  from { transform: translateX(100%); }
  to { transform: translateX(0%); }
}
@keyframes leftOut {
  from { transform: translateX(0%); }
  to { transform: translateX(-100%); }
}
@keyframes rightIn {
  from { transform: translateX(-100%); }
  to { transform: translateX(0%); }
}
@keyframes rightOut {
  from { transform: translateX(0%); }
  to { transform: translateX(100%); }
}
</style>