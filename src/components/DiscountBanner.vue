<template>
  <div class="discount">
    <h4>지금 결제하면 {{initPercent}}% 할인</h4>
  </div>
</template>

<script>
export default {
    name: 'discount-banner',
    props : {
      initPercent : Number
    }
}
</script>

<style>
    .discount {
        background: #eee;
        padding: 10px;
        margin: 20px;
    }
</style>