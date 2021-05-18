<template>
  <div class="carousel">
    <slot></slot>
    <button @click="next" class="carousel__next">次へ</button>
    <button @click="prev" class="carousel__prev">前へ</button>
  </div>
</template>

<script>
export default {
    props: [
    'clickedFilteredItemIndex',
  ],
  setup(props, context) {
    const next = () => {
      // クリックされたもののインデックス番号も渡す
      context.emit('next', props.clickedFilteredItemIndex)
    }
    const prev = () => {
      context.emit('prev', props.clickedFilteredItemIndex)
    }

    return {
      next,
      prev,
    }
  }
}
</script>

<style lang="scss" scoped>
.carousel {
  position: relative;
  width: 100%;
  height: 100%;
  overflow: hidden;

  & button {
    position: absolute;
    height: 40px;
    width: 50px;
    top: calc(50% - 20px);
    background-color: rgba(0,0,0,.8);
    border: none;
    color: #fff;

    &:focus,
    &:hover {
      outline: none;
      cursor: pointer;
    }

  }
  &__next {
    right: 0;
  }
  &__prev {
    left: 0;
  }

}
</style>