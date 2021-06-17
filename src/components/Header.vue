<template>
    <header>
        <h1>{{title}}</h1>
        <Button 
            v-show="homepage"
            @toggle-add-btn="$emit('toggleBtn')" 
            :text="showAddForm ? 'Close':'Add Task'" 
            :color="showAddForm ? 'black':'red'"/>
    </header>
</template>

<script>
import Button from './Button.vue';
    export default {
        name:"Header",
        data() {
            return {
                text:"Add Task",
                color:"red"
            }
        },
        props:{
            title:String,
            showAddForm:Boolean            
        },
        components:{
            Button
        },
        computed: {
            homepage() {
                return this.$route.path === '/' ? true : false;
            }
        }
    }
</script>

<style scoped>
header {
    display: flex;
    justify-content: space-between;
    text-align: center;
}
</style>