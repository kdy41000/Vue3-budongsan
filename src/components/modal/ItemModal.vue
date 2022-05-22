<template>
  <div class="black-bg" v-if="modalStatus">
    <div class="white-bg">
      <div v-if="initPercent > 0">할인적용됨 : {{initPercent}}%</div>
      <img alt="not found" :src="selectedItem.image" />
      <h4>{{selectedItem.title}}</h4>
      <p>{{selectedItem.content}}</p>
      <input type="text" v-model="month" />
      <p>{{month}}개월 선택함 : 
        <span :class="{'item-discounted': discountStatus}">{{selectedItem.price * month}}</span>&nbsp; 
        <span v-if="discountStatus" style="color:red;">{{selectedItem.discountedPrice * month}}</span>원
      </p>
      <button @click="onClickCloseHandler()">닫기</button>
    </div>
  </div>
</template>

<script>

export default {
    name: 'item-modal',
    props: {
        modalStatus : Boolean,
        selectedItem : Object,
        initPercent : Number
    },
    data() {
        return {
            month : 1,
            discountStatus : false
        }
    },
    watch : {
        month(a, b) {  // 왼쪽(a) : 변경 후 데이터, 오른쪽(b) : 변경 전 데이터
            if(isNaN(a)) {
                alert("숫자 입력 불가능");
                this.month = b;
            }
            if(a > 13) {
                alert("13보다 크게 입력 불가능 합니다.");
                this.month = b;
            }
            if(a == 2) {
                alert("현재 2개월은 제공되지 않습니다.");
                this.month = b;
            }
        }
    },
    methods: {
        onClickCloseHandler:function() {
            this.discountStatus = false;
            this.emitter.emit('closeModalEmitter');
        }
    },
    updated() {
        if(this.initPercent != 0 && this.modalStatus && !this.discountStatus) {
            this.discountStatus = true;
            this.emitter.emit('savedDiscountEmitter',this.initPercent);
        }
    }
}
</script>

<style>
.black-bg {
  width: 100%; 
  height:100%;
  background: rgba(0,0,0,0.5);
  position: fixed; padding: 20px;
  z-index: 10;
  top: 0;
}
.white-bg {
  width: 50%;
  background: white;
  border-radius: 8px;
  padding: 20px;
  margin-top: 20%;
  margin-left: 25%;
}
.item-discounted {
    text-decoration:line-through
}
</style>