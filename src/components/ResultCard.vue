<template>
  <div :style="{ display: 'inline-flex', 'flex-wrap': 'wrap', justifyContent: 'center', marginBottom: '3em' }">
    <div v-for="(t, i) in results" :key="i">
      <a :href="t.siteUrl" target="__blank"
        :style="{ display: 'inline-flex', 'flex-flow': 'column', width: '15em', border: 'solid',
        color: 'inherit', textDecoration: 'inherit' }">
        <img :src="t.coverImage.extraLarge"
        :style="{width: 'inherit', height: '20em', objectFit: 'cover', aspectRatio: 'auto' }"></img>
        <p
          :style="{ whiteSpace: 'nowrap', 'max-width': '15em', overflow: 'hidden', 'textOverflow': 'ellipsis', alignSelf: 'center' }">
          {{ t.title.english === null ?
            t.title.romaji :
            t.title.english }}</p>
      </a>

    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  results: Array
});
</script>
