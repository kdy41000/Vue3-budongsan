<template>

    <div @click="onClickItemHandler(item)">
        <img alt="not found" :src="item.image" />
        <h4>{{index + 1}}</h4>
        <h4>{{item.title}}</h4>
        <p>{{item.content}}</p>
        <p>{{item.price}} 원</p>
    </div>

</template>

<script>
export default {
    name: 'card-item',
    props : {
        item: Object,
        index: Number
    },
    methods : {
        onClickItemHandler:function(item) {
            this.emitter.emit('openModalEmitter',item);
        },
    }
}
</script>

<style>

</style>