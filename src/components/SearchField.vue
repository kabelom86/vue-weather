<script setup>
import { reactive } from 'vue';

const emit = defineEmits(['place-data']);

const searchTerm = reactive({
    query: '',
    timeout: null,
    results: null,
});

const handleSearch = () => {
    clearTimeout(searchTerm.timeout);
    searchTerm.timeout = setTimeout(async () => {
        if (searchTerm.query !== '') {
            const response = await fetch(`http://api.weatherapi.com/v1/search.json?key=157c0f0cf591453794660316241010&q=${searchTerm.query}`);

            const data = await response.json();
            searchTerm.results = data;
            console.log(searchTerm.results);
        } else {
            searchTerm.results = null;
        }
    }, 500)
};

const getWeather = (async (id) => {
    const response = await fetch(`http://api.weatherapi.com/v1/forecast.json?key=157c0f0cf591453794660316241010&q=id:${id}&days=3&aqi=no&alerts=no`);

    const data = await response.json();

    // emit the event of this data
    emit('place-data', data);

    // clear the search field
    searchTerm.query = '';
    searchTerm.results = null;
});
</script>

<template>
    <div>
        <!-- search field -->
        <form>
            <div class="bg-white border border-indigo-600/30 rounded-lg shadow-lg flex items-center">
                <i class="fa-solid fa-magnifying-glass p-2 text-indigo-600"></i>
                <input v-model="searchTerm.query" @input="handleSearch" type="text" placeholder="Search for a place"
                    class="rounded-r-lg p-2 border-0 outline-0 focus:ring-2 focus:ring-indigo-600 ring-inset w-full" />
            </div>
        </form>
        <!-- search suggestions -->
        <div class="bg-white my-2 rounded-lg shadow-lg">
            <div v-if="searchTerm.results !== null">
                <div v-for="place in searchTerm.results" :key="place.id">
                    <button @click="getWeather(place.id)"
                        class="px-3 my-2 hover:text-indigo-600 hover:font-bold w-full text-left">
                        {{ place.name }}, {{ place.region }}, {{ place.country }}
                    </button>
                </div>
            </div>

        </div>
    </div>
</template>