<template>
    <div id="app">

        <v-leftNav></v-leftNav>

        <div class="main">
            <router-view/>
        </div>
        

    </div>
</template>

<script>
    import LeftNav from '@/components/commom/LeftNav';
    export default {
        name: 'App',
        data () {
            return {

            }
        },
        components : {
            'v-leftNav' : LeftNav
        }
    }
</script>

<style>
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
    }
    .main{
        float: left;
        width: 95%;
        height: 100%;
        overflow: hidden;
        background: #EFF2F7;
    }
</style>
