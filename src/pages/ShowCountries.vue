<script setup lang="ts">
import { ref, onMounted } from 'vue'

type Country = {
    Code: string;
    Name: string;
};

type City = {
    ID: number;
    Name: string;
    CountryCode: string;
    District: string;
    Population: number;
};

const countryInfoes = ref<Country[]>([]);
const cityInfoes = ref<City[]>([]);
const countryToCityMap = ref<Map<string, City[]>>(new Map());

const selectedCountry = ref<string | null>(null);
const selectedCity = ref<string | null>(null);

onMounted(async () => {
    const countryRes = await fetch('/api/countries/all');
    if (countryRes.ok) {
        countryInfoes.value = await countryRes.json();
    }
    const cityRes = await fetch(`/api/cities/all`);
    if (cityRes.ok) {
        cityInfoes.value = await cityRes.json();
    }
    
    for (const city of cityInfoes.value) {
        if (countryToCityMap.value.has(city.CountryCode)) {
            countryToCityMap.value.get(city.CountryCode)?.push(city);
        } else {
            countryToCityMap.value.set(city.CountryCode, [city]);
        }
    }
});

const toggleCountry = (code: string) => {
    selectedCountry.value = selectedCountry.value === code ? null : code;
    selectedCity.value = null;
}

const toggleCity = (name: string) => {
    selectedCity.value = selectedCity.value === name ? null : name;
}
</script>

<template>
    <h1>国一覧</h1>
    <div v-for="country in countryInfoes">
        {{ country.Name }}
        <!-- <button @click="toggleCountry(country.Code)">
            {{ country.Name }}
        </button>
        <div v-show="selectedCountry === country.Code">
            <div v-for="city in countryToCityMap.get(country.Code)" :key="city.Name">
                <div>
                    <button @click="toggleCity(city.Name)">
                        {{ city.Name }}
                    </button>
                </div>
                <div v-show="selectedCity === city.Name">
                    {{ city }}
                </div>
            </div>
        </div> -->
    </div>
</template>

<style>
.countryName{
    display: inline-block;
    background-color: aqua;
    padding: 5px 10px;
    cursor: pointer;
}
.cityNames{
    display: inline-block;
    background-color: chartreuse;
    padding: 5px 10px;
    cursor: pointer;
}
.cityInfoes{
    background-color: chocolate;
}

</style>
