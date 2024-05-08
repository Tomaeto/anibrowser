<template>
  <div
   v-for="(t, i) in results" :key="i">

    <a :href="t.siteUrl" target="__blank"> 
      <img :src="t.coverImage.large"></img>
    </a>
  </div>
</template>



<script setup>
const props = defineProps({
  results: Array
})
</script>
