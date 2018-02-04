<template>
    <div v-show="showFlag" class="food">
        <div class="image-header">
        </div>
    </div>
</template>
<script type="text/ecmascript-6">
    export default {
        props: {
            food: {
                type: Object
            }
        },
        data () {
            return {
                shoewFlag: false
            };
        },
        methods: {
            show() {
               this.shoewFlag = true; 
            }
        }
    }
</script>

<style lang="stylus" rel="stylesheet/stylus">
   
</style>
