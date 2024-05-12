<template>

    <div :style="{display: 'inline-flex', marginTop:'auto'}">
    <input class="searchInput" type="form" v-model="searchInput" placeholder="Search Anime & Manga" @keyup.enter="getSearch" :disabled="!searchResults"></input>
    <input class="searchButton" type="button" @click="getSearch" value="Search" :disabled="!searchResults"></input>
</div>
    <p v-if="!searchResults">Loading...</p>
    <h1 v-else-if="searchComplete" :style="{ display: 'flex', justifyContent: 'left' }">Total Results: {{ searchResults.length }}</h1>
    <ResultCard :results="searchResults" />
</template>

<script setup>
import { ref, watch } from 'vue'
import ResultCard from "./ResultCard.vue"
const searchQuery = `
query($search: String, $perPage: Int) {
    Page(perPage:$perPage) {
        pageInfo {
            total
            perPage
        }
        media(search: $search, isAdult:false, sort:POPULARITY_DESC) {
            title {
                english
                romaji
                native
            }
            status
            format
            genres
            siteUrl
            coverImage {
                large
                extraLarge
            }
            description
            startDate {
        year
        month
        day
      }
      endDate {
        year
        month
        day
      }
      averageScore
      type
      format
      episodes
      chapters
        }
    }
}
`;
var variables = {
    search: "Default",
    page: 1,
    perPage: 25,
};
var url = 'https://graphql.anilist.co',
    options = {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json',
            'Accept': 'application/json',
        },
        body: JSON.stringify({
            query: searchQuery,
            variables: variables
        })
    };

const searchInput = ref('');
const searchComplete = ref(false);
const searchResults = ref([]);
async function getSearch() {
    searchResults.value = null;
    variables.search = searchInput.value;
    options.body = JSON.stringify({
        query: searchQuery,
        variables: variables
    });
    await fetch(url, options).then(handleResponse)
        .then(handleData);
}
function handleResponse(response) {
    return response.json().then((json) => {
        return response.ok ? json : Promise.reject(json);
    });
}

function handleData(data) {
    searchResults.value = JSON.parse(JSON.stringify(data.data.Page.media))
    searchComplete.value = true;
}
</script>

<style scoped>
.searchInput {
    font-family:Arial, Helvetica, sans-serif;
    width: 12em;
    height: 2.25em;
    margin-right: 1em;
    margin-left: 1em;
    padding-right:100px;
    font-size:16px;
    border-style:solid;
    border-radius: 10px;
    background-color: white;
}

.searchInput:focus {
    outline: none;
    border-color:forestgreen;
}

.searchButton {
    font-family:Arial, Helvetica, sans-serif;
    width: 5em;
    border-radius: 10px;
    font-size:16px;
    font-weight:550;
    color: white;
    border-style: solid;
    background-color: forestgreen;
}

.searchButton:hover {
    background-color: green;
    transition-duration: 100ms;
}
.searchButton:focus {
 background-color: forestgreen;
}
</style>