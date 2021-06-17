<template>
    <button @click="buttonClicked" :style="{background: color}" class="btn">{{text}}</button>
</template>

<script>
    export default {
        name:'Button', 
        props: {
            text:String,
            color:String
        },
        methods:{
            buttonClicked() {
                this.$emit('toggle-add-btn');
            }
        }
    }
</script>

<style scoped>

</style>