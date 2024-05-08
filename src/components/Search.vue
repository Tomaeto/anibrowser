<template>

    <input v-model="searchInput" placeholder="Input search term"></input>
    <input type="button" @click="getSearch" value="Click to search" :disabled="!searchResults"></input>

    <p v-if="!searchResults">Loading...</p>
    <h1 v-else>Search Results</h1>
    <ul>
        <ResultCard :results="searchResults" />
    </ul>
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
}
</script>