<script setup lang="ts">
import { useLocalStorage } from "@vueuse/core"

import TextArea from "@/components/TextArea.vue";
import SearchTerm from "@/components/SearchTerm.vue";

const KEY = {
    AUDIENCE_NAMES: "AUDIENCE_NAMES",
    TERMS_OF_ART: "TERMS_OF_ART",
    PRODUCTS: "PRODUCTS",
    WATERING_HOLE_KEYWORDS: "WATERING_HOLE_KEYWORDS",
    TERMS: "TERMS",
    CLICKED_TERMS: "CLICKED_TERMS",
} as const;

const audienceNames = useLocalStorage(KEY.AUDIENCE_NAMES, "elixir developer, elixir dev");
const termsOfArt = useLocalStorage(KEY.TERMS_OF_ART, "TDD, MVC");
const products = useLocalStorage(KEY.PRODUCTS, "Phoenix, Phoenix LiveView");
const wateringHoleKeywords = useLocalStorage(KEY.WATERING_HOLE_KEYWORDS, "forum, email");

const terms = useLocalStorage<string[]>(KEY.TERMS, []);
const clickedTerms = useLocalStorage<string[]>(KEY.CLICKED_TERMS, []);

const transformStringToArray = (str: string) => {
    return str.split(",").map((item) => item.trim());
};

const getRandomItem = (arr: string[]) => {
    return arr[Math.floor(Math.random() * arr.length)];
};

const getItem = (val: string) => {
    return getRandomItem(transformStringToArray(val));
};

const getRandomSearchTerm = () => {
    const audienceName = getItem(audienceNames.value);
    const termOfArt = getItem(termsOfArt.value);
    const product = getItem(products.value);

    const item = getRandomItem([termOfArt, product]);
    const wateringHoleKeyword = getItem(wateringHoleKeywords.value);

    return `${audienceName} ${item} ${wateringHoleKeyword}`;
};

const generateTerms = () => {
    const count = 15;
    const generatedTerms: string[] = [];
    for (let i = 0; i < count; i++) {
        let newTerm = getRandomSearchTerm();
        console.log(newTerm);
        let counter = 0;
        while (generatedTerms.includes(newTerm) && counter < 100) {
            newTerm = getRandomSearchTerm();
            counter++;
        }

        generatedTerms.push(newTerm);
    }

    terms.value = generatedTerms;
};

const addClickedTerm = (term: string) => {
    clickedTerms.value.push(term);
};

generateTerms();
</script>

<template>
    <main class="min-h-screen bg-gray-50 p-8 space-y-6">
        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
            <TextArea id="audienceNames" name="Audience names:" v-model="audienceNames" />
            <TextArea id="termsOfArt" name="Terms of art:" v-model="termsOfArt" />
            <TextArea id="products" name="Products:" v-model="products" />
            <TextArea id="wateringHoleKeywords" name="Watering hole keywords:" v-model="wateringHoleKeywords" />
            <div class="md:col-span-2">
                <button type="button"
                    class="block text-center rounded-md border border-transparent bg-gray-600 px-4 py-2 text-base font-medium text-white shadow-sm hover:bg-gray-700 focus:outline-none focus:ring-2 focus:ring-gray-500 focus:ring-offset-2 w-full"
                    @click="generateTerms">
                    Generate terms
                </button>
            </div>
        </div>


        <div class="grid gap-2 bg-white p-3 border border-gray-600 shadow-md rounded-md">
            <div class=" text-center ">
                <h2 class="text-xl leading-tight ">Terms</h2>
                <p class="text-sm text-gray-500">Click to open in Google</p>
            </div>
            <hr class="border-black -mx-3">
            <SearchTerm v-for="term in terms" :key="term" :term="term" :clickedTerms="clickedTerms"
                @click="addClickedTerm" />
        </div>
    </main>
</template>
