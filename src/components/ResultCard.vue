<template>
  <div :style="resultBox">
    <div v-for="(t, i) in results" :key="i">
      <div :href="t.siteUrl" target="__blank" :style="{
        display: 'inline-flex', 'flex-flow': 'row', height: 'auto', width: '90em', border: 'solid',
        color: 'inherit', textDecoration: 'inherit', justifyContent: 'left', backgroundColor: 'whitesmoke'
      }">

        <img :src="t.coverImage.extraLarge" :style="imgStyle"></img>
        <div :style="{ display: 'inline-flex', flexFlow: 'column'}">
          <a :href="t.siteUrl" target="__blank" :style="{ textDecoration: 'inherit' }">
            {{ t.title.english === null ?
              t.title.romaji :
              t.title.english }}</a>
          <p>{{ t.title.native }}</p>
          <p v-if="t.description">{{ t.description.replace(descRegex, "") }}</p>
          <p v-else>No description provided.</p>

          <div>
            <span v-if="t.averageScore" :style="avgScore">Average Score: {{ t.averageScore }}%</span>

            <p>Status: {{titleize(t.status)}}</p>
            <p> Series Type</p>
            <p>Episodes/Chapters</p>
            <p>Start Date</p>
            <p>End Date</p>
            <p>Genres</p>
          </div>
        </div>

      </div>

    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const resultBox = ref({
  display: 'inline-flex',
  flexWrap: 'wrap',
  justifyContent: 'left',
  marginBottom: '3em',
  marginLeft: '3em'
})

const imgStyle = ref({
  width: '13em',
  height: 'auto',
  objectFit: 'cover',
  marginRight: '1em'
})

const avgScore = ref({
  display: 'inline'
})

const props = defineProps({
  results: Array
});


function titleize(value) {
  value = value.replaceAll('_', ' ').toLowerCase();
  return value.replace(/(?:^|\s|-)\S/g, x => x.toUpperCase());
}

const descRegex = ref(/(<([^>]+)>)/gi)
</script>
