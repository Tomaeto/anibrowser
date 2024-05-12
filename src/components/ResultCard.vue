<template>
  <div class="resultBox">
    <div v-for="(t, i) in results" :key="i">
      <div class="result" :href="t.siteUrl" target="__blank">

        <img :src="t.coverImage.extraLarge"></img>
        <div :style="{ display: 'inline-flex', flexFlow: 'column', marginTop: '1em' }">
          <a :href="t.siteUrl" target="__blank">
            {{ t.title.english === null ?
              t.title.romaji :
              t.title.english }}</a>
          <p class="nativeTitle normal">{{ t.title.native }}</p>
          <p class="descriptionText normal" v-if="t.description">{{ t.description.replace(descRegex, "") }}</p>
          <p v-else>No description provided.</p>

          <div class="infoBox">
            <p> Type:  
              <span v-if="t.format === 'MANGA' || t.type === 'ANIME'">{{ titleize(t.type) }}</span>
              <span v-else>{{ titleize(t.format) }}</span>
            </p>
            <p v-if="t.type === 'ANIME' && t.format"> Format: 
              <span v-if="!['TV', 'ONA', 'OVA'].includes(t.format)">
                {{titleize(t.format)}}
              </span>
              <span v-else>
                {{t.format}}
              </span>
            </p>
            <p>Status: 
              <span>{{ titleize(t.status) }}
              </span>
            </p>

            <p v-if="t.type === 'ANIME' && t.episodes"> Episodes:  
              <span>{{ t.episodes }}</span>
            </p>
            <p v-else-if="t.type === 'MANGA' && t.chapters">Chapters:
              <span> {{ t.chapters }}
              </span>
            </p>
            <p  v-if="t.startDate.day">Start Date: 
              <span>{{ t.startDate.month }}/{{ t.startDate.day }}/{{ t.startDate.year }}
              </span> 
            </p>
            <p v-if="t.endDate.day">End Date: 
              <span>{{ t.endDate.month }}/{{ t.endDate.day }}/{{ t.endDate.year }}
              </span>
            </p>

            <span v-if="t.averageScore">Average Score:
              <span>{{ t.averageScore }}%</span>
            </span>
            <p>
            <div class="genreBox" v-for="genre in t.genres">
              <span> {{ genre }} </span>
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

a {
  color: black;
  text-decoration: inherit;
  margin-right: auto;
  font-size: 1.3em;
}

a:hover {
  color: green;
  transition: ease-in-out;
  transition-duration: 100ms;
}

.resultBox {
  display: inline-flex;
  flex-wrap: wrap;
  justify-content: left;
  margin-bottom: 3em;
  margin-left: 3em;
  color: darkslategrey;
}

.result {
  display: inline-flex;
  flex-flow: row;
  height: auto;
  width: 100em;
  border: solid;
  color: inherit;
  text-decoration: inherit;
  justify-content: left;
  background-color: white;
  margin-bottom: 1em;
}

.nativeTitle {
  font-style: italic;
  color: slategrey;
  margin-top: auto;
  margin-bottom: auto;
}

.descriptionText {
  line-height: 1.5;
  margin-top: 1em;
  margin-bottom: 0.5em;
}

.infoBox {
  font-weight: 600;
  font-size: 0.85em
}

.genreBox {
  display: inline-flex;
  font-weight: 550;
  color: black;
  background: lightgreen;
  margin-right: 0.5em;
  padding: 0.5em;
  justify-content: center;
  border-radius: 20px;
}
</style>
