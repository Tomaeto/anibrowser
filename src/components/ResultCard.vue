<template>
  <div :style="{display:'inline-flex', 'flex-wrap':'wrap', justifyContent:'center', marginBottom:'20px'}"
   v-for="(t, i) in results" :key="i">

   <div :style="{display:'flex', 'flex-direction':'column', margin:'auto', width:'350px'}">
    <a :href="t.siteUrl" target="__blank" :style="{margin:'auto'}"> 
      <img :src="t.coverImage.large"></img>
    </a>
    <p1 :style="{display:'flex', justifyContent:'center'}">{{ t.title.english === null ? t.title.romaji : t.title.english }}</p1>
  </div>
  </div>
</template>

<script setup>
const props = defineProps({
  results: Array
});
</script>
