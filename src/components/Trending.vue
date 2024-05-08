<template>
    <h1>Trending Anime</h1>
    <p v-if="!trendingAnime">Loading Anime...</p>
    <div v-for="(t, i) in trendingAnime" :key="i">
        <a :href="t.siteUrl" target="__blank">
            <img :src="t.coverImage.large"></img>
        </a>

    </div>
    <h1>Trending Manga</h1>
    <p v-if="!trendingManga">Loading Manga...</p>
    <div v-for="(t, i) in trendingManga" :key="i">

        <a :href="t.siteUrl" target="__blank">
            <img :src="t.coverImage.large"></img>
        </a>

    </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
const trendingQuery = `
query ($page: Int, $perPage: Int, $type:MediaType) {
    Page(page: $page, perPage: $perPage) {
        pageInfo {
            total
            perPage
        }
        media(type:$type, sort:TRENDING_DESC) {
            coverImage {
                large
            }
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
    perPage: 8,
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

async function getTrendingData() {
    await fetch(url, options).then(handleResponse)
        .then(handleData);
    variables.type = "MANGA";

    options.body = JSON.stringify({
        query: trendingQuery,
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
    if (variables.type == "ANIME") trendingAnime.value = JSON.parse(JSON.stringify(data.data.Page.media))
    else trendingManga.value = JSON.parse(JSON.stringify(data.data.Page.media))
}

onMounted(() => {
    trendingAnime.value = null;
    trendingManga.value = null;
    getTrendingData()
})

</script>