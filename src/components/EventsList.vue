<template>
    <div id="events">
    </div>
</template>

<script>
    import Vue from 'vue';
    import moment from 'moment';
    
    Vue.prototype.moment = moment;

    export default {
        name: 'EventsList',
        data (){
            return {
                events: []
            }
        }
    }
</script>

<style lang="scss">
    #events {
        color: #333333;
    }
</style>