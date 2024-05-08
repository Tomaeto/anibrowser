<template>
  <div :style="{ display: 'inline-flex', 'flex-wrap': 'wrap', justifyContent: 'center', marginBottom:'3em' }">
    <div v-for="(t, i) in results" :key="i"
      :style="{ display: 'inline-flex', 'flex-flow': 'column', width: '20em', border: 'solid' }">
      <a :href="t.siteUrl" target="__blank" :style="{ margin: 'auto' }">
        <img :src="t.coverImage.large"></img>
      </a>
      <p :style="{ display: 'webkit-box', textAlign: 'center', margin: 'auto' }">{{ t.title.english === null ?
        t.title.romaji :
        t.title.english }}</p>
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  results: Array
});
</script>
