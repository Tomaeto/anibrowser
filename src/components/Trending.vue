<template>
<h1>Trending Anime</h1>

<h1>Trending Manga</h1>
</template>

<script setup>
import {ref} from 'vue'
const trendingQuery = `
query ($page: Int, $perPage: Int, $type:MediaType) {
    Page(page: $page, perPage: $perPage) {
        pageInfo {
            total
            perPage
        }
        media(type:$type, sort:TRENDING_DESC) {
            id
            title {
                romaji
                english
            }
            isAdult
            genres
            siteUrl
        }
    }
}
`;
// Define our query variables and values that will be used in the query request
var variables = {
    search: "Default",
    page: 1,
    perPage: 5,
    type: "ANIME"
};


// Define the config we'll need for our Api request
var url = 'https://graphql.anilist.co',
    options = {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json',
            'Accept': 'application/json',
        },
        body: JSON.stringify({
            query: trendingQuery,
            variables: variables
        })
    };

const trendingAnime = ref([]);
const trendingManga = ref([]);


</script>