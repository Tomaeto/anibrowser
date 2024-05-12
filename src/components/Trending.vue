<template>
    <h1 class="header">Trending Anime</h1>
    <p v-if="!trendingAnime">Loading Anime...</p>
    <div class="resBox">
        <div v-for="(t, i) in trendingAnime" :key="i">
            <a class="result" :href="t.siteUrl" target="__blank">

                <img :src="t.coverImage.extraLarge"></img>

                <p class="title">
                    {{ t.title.english === null ? t.title.romaji : t.title.english }}</p>

            </a>


        </div>
    </div>
    <h1 class="header">Trending Manga</h1>
    <p v-if="!trendingManga">Loading Manga...</p>
    <div class="resBox">
        <div v-for="(t, i) in trendingManga" :key="i">
            <a class="result" :href="t.siteUrl" target="__blank" title="Test">

                <img :src="t.coverImage.extraLarge"></img>
                <p class="title">
                    {{ t.title.english === null ? t.title.romaji : t.title.english }}</p>
            </a>
        </div>
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
                extraLarge
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
    perPage: 6,
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

const resBoxStyle = ref({

})

</script>

<style scoped>
img {
    border-radius: 10px; 
    width: inherit; 
    height: 22em;
    object-fit: cover; 
    aspect-ratio: auto; 
}

.header {
    margin-left: 20px;
}

.result {
    display: inline-flex;
    flex-flow: column;
    width: 14em;
    margin-inline: 2.5em;
    justify-content: center;
    color: inherit;
    text-decoration: inherit;
}

.result:hover {
    transform: scale(1.1);
    transition: ease-in-out;
    transition-duration: 500ms;
}

.title {
    white-space: nowrap; 
    max-width: 15em; 
    overflow: hidden; 
    text-overflow: ellipsis; 
    align-self: center; 
}

.resBox {
    display: inline-flex; 
    flex-wrap: wrap; 
    justify-content: normal;
}

</style>