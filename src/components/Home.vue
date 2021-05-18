<template>
  <section class="home section bd-container">
    <span class="section-subtitle">sub title</span>
    <h2 class="section-title">home</h2>

    <!-- ナブバー -->
    <div class="home__nav" ref="home">
      <span class="home__item" @click="showAll">All</span>
      <span class="home__item" @click="FilterCatOne" >カテゴリー1</span>
      <span class="home__item" @click="FilterCatTwo">カテゴリー2</span>
    </div>

    <!-- ナブアイテム -->
    <transition-group
      class="home__container bd-grid"
      tag="div"
      appear
      name="home__list"
      @before-enter="beforeEnter"
      @enter="enter"
    >

      <div
        class="home__content"
        v-for="(item, filteredItemIndex) in filteredItems"
        :key="item"
        :class="{'is-hidden' : item.isHidden}">
        
        <Carousel
            @next="next"
            @prev="prev"
            :clickedFilteredItemIndex="filteredItemIndex"
          >
            <CarouselSlide
              v-for="(slide, index) in item.img"
              :key="slide"
              :index="index"
              :visibleSlide="visibleSlide"
              :direction="direction"
              >
              <img :src="require(`@/assets/img/${slide}.jpg`)" class="home__img" :alt="slide">
            </CarouselSlide>
          </Carousel>
        <div class="home__data">
          <span class="home__subtitle">{{ item.label }}</span>
          <a href="#"><h2 class="home__title">{{ item.title }}</h2></a>
          <p class="home__skills">{{ item.notes }}</p>
          <a href="#" class="button button-link">詳しく見てみる</a>
        </div>
      </div>

    </transition-group>

  </section>
</template>

<script>
import Carousel from '@/components/home/Carousel.vue'
import CarouselSlide from '@/components/home/CarouselSlide.vue'

import { ref } from '@vue/reactivity'
import { onMounted } from 'vue'
import gsap from 'gsap'

export default {
  components: {
    Carousel,
    CarouselSlide,
  },
  setup() {

    // データ
    const homeItems = ref([
      {
       id: 1,
       label: 'カテゴリー1',
       title: 'タイトル',
       notes: 'ノート',
       img: [
         '0-01',
         '0-02',
         '0-03',
         '0-04',
       ],
       isHidden: false},
      {
       id: 2,
       label: 'カテゴリー2',
       title: 'タイトル',
       notes: 'ノート',
       img: [
         '0-01',
         '0-02',
         '0-03',
         '0-04',
       ],
       isHidden: false},
      {
       id: 3,
       label: 'カテゴリー1',
       title: 'タイトル',
       notes: 'ノート',
       img: [
         '0-01',
         '0-02',
         '0-03',
         '0-04',
       ],
       isHidden: false},
      {
       id: 4,
       label: 'カテゴリー2',
       title: 'タイトル',
       notes: 'ノート',
       img: [
         '0-05',
         '0-06',
         '0-07',
         '0-08',
       ],
       isHidden: false},
      {
       id: 5,
       label: 'カテゴリー1',
       title: 'タイトル',
       notes: 'ノート',
       img: [
         '0-01',
         '0-02',
         '0-03',
         '0-04',
       ],
       isHidden: false},
      {
       id: 6,
       label: 'カテゴリー2',
       title: 'タイトル',
       notes: 'ノート',
       img: [
         '0-05',
         '0-06',
         '0-07',
         '0-08',
       ],
       isHidden: false},

    ])

    // データの表示用
    const filteredItems = ref(null)

    // ナブアイテム
    const home = ref(null)

    // クリックされたナブアイテムをアクティブにする
    const activeNav = (el) => {
      home.value.childNodes.forEach(e => {
        e.className = 'home__item'
      })
      el.currentTarget.className = 'home__item home__nav-active'
    }


  // カテゴリー1を優先表示
  const FilterCatOne = (el) => {
    homeItems.value.filter(item => {
      if(item.label !== 'カテゴリー1') {
        item.isHidden = true
      } else {
        item.isHidden = false
      }
    })
    // 並び替えtrue,falseの順
    filteredItems.value = [...homeItems.value].sort((a,b) => a.isHidden - b.isHidden)

    activeNav(el)

  }
  // カテゴリー2を優先表示
  const FilterCatTwo = (el) => {
    homeItems.value.filter(item => {
      if(item.label !== 'カテゴリー2') {
        item.isHidden = true
      } else {
        item.isHidden = false
      }
    })
    // 並び替えtrue,falseの順
    filteredItems.value = [...homeItems.value].sort((a,b) => a.isHidden - b.isHidden)
    
    activeNav(el)
  }

    // すべてを表示(id順)
    const showAll = (el) => {
      homeItems.value.filter(item => {
        item.isHidden = false
      })
      // id順に並び替え
      filteredItems.value = [...homeItems.value].sort((a,b) => a.id - b.id)

      activeNav(el)
    }

    // 交差後のすぐに発火するアニメーション
    const beforeEnter = (el) => {
      el.style.opacity = 0
    }
    // 交差後のすぐに発火するアニメーション
    const enter = (el,done) => {
      gsap.to(el, {
        opacity: 1,
        duration:0.5,
        ease: 'circ.out',
        onComplete: done,
      })
    }


    // indexとイコールの変数を作成する
    const visibleSlide = ref(0)

    // indexには、Carousel.vueでクリックされたボタンのindexが渡る
    const slidesLength = (clickedFilteredItemIndex) => {
     return filteredItems.value[clickedFilteredItemIndex].img.length
    }

    // ボタン、left,rightを入れ替えるための変数
    const direction = ref('left')

    // Carouselから受け取ったnext
    const next = (clickedFilteredItemIndex) => {
      if(visibleSlide.value >= slidesLength(clickedFilteredItemIndex) - 1) {
        visibleSlide.value = 0
      } else {
        visibleSlide.value++
      }
      
      direction.value = 'carousel__left'
    }

    // Carouselから受け取ったprev
    const prev = (clickedFilteredItemIndex) => {
      if(visibleSlide.value <= 0) {
        visibleSlide.value = slidesLength(clickedFilteredItemIndex) - 1
      } else {
        visibleSlide.value--
      }

      direction.value = 'carousel__right'
    }

    onMounted(() => {
      
      let options = {
        rootMargin: '0px',
        threshold: 1.0
      }

      const observer = new IntersectionObserver(function(entries) {
        entries.forEach(entry => {
          if (entry.isIntersecting) {
           filteredItems.value = homeItems.value
           observer.disconnect()
          }
        })
      }, options)
      observer.observe(home.value)
    })

    return {
      homeItems,
      home,
      FilterCatOne,
      FilterCatTwo,
      filteredItems,
      showAll,
      beforeEnter,
      enter,
      activeNav,

      // slides,
      visibleSlide,
      direction,
      next,
      prev,
    }
  }
}
</script>

<style lang="scss" scoped>
.is-hidden {
  opacity: 0.5 !important;
  pointer-events: none !important;
}

.home {
  &__nav {
    text-align: center;
    margin-bottom: var(--mb-3);

    &-active {
      color: var(--first-color-light);
    }
  }

  &__item {
    margin: 0 var(--mb-2);
    cursor: pointer;
    display: inline-block;

    &:hover,
    &:active {
      color: var(--first-color-light);
    }
  }

  &__content {
    transition: all ease-in-out .5s;
    position: relative;
    min-height: 500px;
    display: flex;
    flex-direction: column;

    background-color: #FFF;
    border-radius: .5rem;
    overflow: hidden;
    box-shadow: 0 4px 6px rgba(185, 119, 97,.3);

    &:hover {
      box-shadow: 0 6px 8px rgba(185, 119, 97, 0.4);

      & .home__img {
        transform: scale(1.02)
      }
    }
  }

  &__img {
    width: 100%;
    height: auto;
    transition: .4s;
  }

  &__data {
    width: 100%;
    padding: 1.5rem;
    z-index: 100;
  }

  &__subtitle,
  &__skills {
    font-size: var(--small-font-size);
    color: var(--first-color-light);
  }

  &__title {
    font-size: var(--h3-font-size);
    color: var(--first-color);
    margin: var(--mb-2) 0;
  }

}
@media screen and (min-width: 576px) {
  .home {
    &__container {
      grid-template-columns: repeat(2,1fr);
    }
  }
}
@media screen and (min-width: 768px) {
  .home {
    &__container {
      grid-template-columns: repeat(3,1fr);
    }
  }
}
</style>