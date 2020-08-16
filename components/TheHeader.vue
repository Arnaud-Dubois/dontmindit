<template>
    <header class="header text-white text-center pt-16 pb-32">

        <!-- Version -->
        <div class="absolute top-0 right-0 m-3 text-color">
          v2.0.2
        </div>
        <!-- / Version -->
        
        <!-- Header -->
        <TheLogo/>
        <h1 class="text-5xl leading-none">DontMindIT</h1>
        <h2 class="text-2xl text-color font-light">Clear your mind</h2>
        <!-- / Header -->

        <!-- Date -->
        <div class="w-40 mx-auto mt-8">
          <div class="grid grid-cols-2 grid-rows-2">
            <div class="row-span-2 mt-1 text-6xl text-right mr-2 leading-10">{{ new Date().getDate() }}</div>
            <div class="text-2xl font-light text-left leading-none">{{ new Date() | moment('MMMM') }}</div>
            <div class="text-xl mt-1 font-bold text-left leading-none">{{ new Date().getFullYear() }}</div>  
          </div>
        </div>
        <!-- / Date -->

    </header>
</template>

<script>
import moment from 'moment'

export default {
    name: 'the-header',
    filters: {
        moment: function (date, format) {
        return moment(date).format(format)
        }
    },
}
</script>

<style lang="scss">
    .header {
        background: rgb(31,28,227);
        background: linear-gradient(58deg, rgba(31,28,227,1) 0%, rgba(54,9,251,1) 42%, rgba(29,132,255,1) 100%);
    }
</style>
