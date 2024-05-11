<template>
  <div class="resultBox">
    <div v-for="(t, i) in results" :key="i">
      <div :href="t.siteUrl" target="__blank" :style="{
        display: 'inline-flex', 'flex-flow': 'row', height: 'auto', width: '90em', border: 'solid',
        color: 'inherit', textDecoration: 'inherit', justifyContent: 'left', backgroundColor: 'whitesmoke'
      }">

        <img :src="t.coverImage.extraLarge"></img>
        <div :style="{ display: 'inline-flex', flexFlow: 'column' }">
          <a :href="t.siteUrl" target="__blank" :style="{ textDecoration: 'inherit' }">
            {{ t.title.english === null ?
              t.title.romaji :
              t.title.english }}</a>
          <p>{{ t.title.native }}</p>
          <p v-if="t.description">{{ t.description.replace(descRegex, "") }}</p>
          <p v-else>No description provided.</p>

          <div>
            <p> {{ titleize(t.type) }}</p>
            <p>Status: {{ titleize(t.status) }}</p>

            <p v-if="t.type === 'ANIME' && t.episodes"> Episodes: {{ t.episodes }}</p>
            <p v-else-if="t.type === 'MANGA' && t.chapters">Chapters: {{ t.chapters }}</p>
            <p v-if="t.startDate.day">Start Date: {{ t.startDate.month }}/{{ t.startDate.day }}/{{ t.startDate.year }}</p>
            <p v-if="t.endDate.day">End Date: {{ t.endDate.month }}/{{ t.endDate.day }}/{{ t.endDate.year }}</p>

            <span v-if="t.averageScore" :style="avgScore">Average Score: {{ t.averageScore }}%</span>
            <p>
            <div v-for="genre in t.genres" :style="{ display: 'inline-flex', backgroundColor: 'pink', marginRight: '1em' }">
              <span> {{ genre }}</span>
            </div>
            </p>
          </div>
        </div>

      </div>

    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const resultBox = ref({

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

<style scoped>
img {
  width: 13em;
  height: auto;
  object-fit: cover;
  margin-right: 1em;
}

.resultBox {
  display: inline-flex;
  flex-wrap: wrap;
  justify-content: left;
  margin-bottom: 3em;
  margin-left: 3em;
}
</style>
