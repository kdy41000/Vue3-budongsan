<template>
  <h1 :style="mainTitle">원룸샵</h1>
  <menu-nav :menuArr="menuArr" />
  <discount-banner v-if="showDiscount" :initPercent="initPercent" />
  
 <section>
   <div>
      <span>정렬</span>
      <button @click="sortHandler('asc')">▲</button>
      <button @click="sortHandler('desc')">▼</button>
      <button @click="sortHandler('back')">되돌리기</button>
  </div>
    <card-item 
      v-for="(item,index) in itemList" 
      :key="item"
      :item="item" 
      :index="index" 
    />
  </section>

  <!-- vue에서 제공하는 애니메이션 기능(name설정 후 css작업 필요) -->
  <transition name="fade">
    <item-modal :modalStatus="modalStatus" :selectedItem="this.selectedItem" :initPercent="initPercent" />
  </transition>
</template>

<script>
//import eventBus from './eventBus';
import { menuArr, itemArr } from './assets/data';
import DiscountBanner from './components/DiscountBanner.vue';
import ItemModal from './components/modal/ItemModal.vue';
import CardItem from './components/CardItem.vue';
import MenuNav from './components/MenuNav.vue';

export default {
  name: 'App',
  data(){
    return {
      mainTitle : 'color: blue',
      menuArr,
      itemList : itemArr,
      itemOriginal : [...itemArr],
      modalStatus : false,
      selectedItem : {},
      showDiscount: true,
      initPercent : 20,
      timer : null
    }
  }, 
  components: {
    DiscountBanner, ItemModal, CardItem, MenuNav
  },
  methods : {
    closeModalHandler:function() {
      this.selectedItem = {};
      this.modalStatus = false;
    },
    openModalHandler:function(param) {
      this.selectedItem = JSON.parse(JSON.stringify(param));
      this.modalStatus = true;
      clearInterval(this.timer);
    },
    sortHandler:function(flag) {
      if(flag === "asc") {
        this.itemList.sort((a, b) => a.price - b.price);
      } else if(flag === "desc") {
        this.itemList.sort((a, b) => b.price - a.price);
      } else if(flag === "back") {
        this.itemList = [...this.itemOriginal];
      }
    },
    savedDiscountHandler:function(rate) {
      console.log("할인적용",rate);
      const savedPrice = this.selectedItem.price * (rate / 100);
      const resultPrice = this.selectedItem.price - savedPrice;

      console.log("원래가격:",this.selectedItem.price);
      console.log("할인가격:",resultPrice);
      this.selectedItem.discountedPrice = resultPrice;
    }
  },
  mounted(){
    console.log("=== mounted ===");
    this.timer = setInterval(() => {
      this.initPercent--;
      if(this.initPercent === 0) clearInterval(this.timer);
    },1000);

    this.emitter.on('closeModalEmitter',this.closeModalHandler);
    this.emitter.on('openModalEmitter',this.openModalHandler);
    this.emitter.on('savedDiscountEmitter',this.savedDiscountHandler);
  },
  created() {
    console.log("=== created ===");
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.item {
  margin-bottom: 50px;
}

img { width:50%; }

/* 등장 */
.fade-enter-from {
  opacity: 0;
}
.fade-enter-active {
  transition: all 1s;
}
.fade-enter-to {
  opacity: 1;
}

/* 퇴장 */
.fade-leave-from {
  opacity: 1;
}
.fade-leave-active {
  transition: all 1s;
}
.fade-leave-to {
  opacity: 0;
}
</style>
