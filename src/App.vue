<script setup lang="ts">
import { useLocalStorage } from "@vueuse/core"

import TextArea from "@/components/TextArea.vue";
import SearchTerm from "@/components/SearchTerm.vue";
import ToggleButton from "@/components/ToggleButton.vue";
import RegenerateButton from "@/components/RegenerateButton.vue";
import ArrowPath from "@/components/ArrowPath.vue";

const KEY = {
    SHOW_INPUT: "SHOW_INPUT",
    AUDIENCE_NAMES: "AUDIENCE_NAMES",
    TERMS_OF_ART: "TERMS_OF_ART",
    PRODUCTS: "PRODUCTS",
    WATERING_HOLE_KEYWORDS: "WATERING_HOLE_KEYWORDS",
    TERMS: "TERMS",
    CLICKED_TERMS: "CLICKED_TERMS",
} as const;

const showInputs = useLocalStorage(KEY.SHOW_INPUT, true);
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
    <main class="min-h-screen bg-gray-50 p-2 md:p-8 space-y-6">
        <div v-if="showInputs" class="grid grid-cols-1 md:grid-cols-2 gap-4">
            <TextArea id="audienceNames" name="Audience names:" v-model="audienceNames" />
            <TextArea id="termsOfArt" name="Terms of art:" v-model="termsOfArt" />
            <TextArea id="products" name="Products:" v-model="products" />
            <TextArea id="wateringHoleKeywords" name="Watering hole keywords:" v-model="wateringHoleKeywords" />
            <div class="md:col-span-2 flex items-center justify-center gap-4">
                <ToggleButton v-model="showInputs" />
                <RegenerateButton @click="generateTerms" />
            </div>
        </div>


        <div class="grid gap-2 bg-white p-3 border border-gray-600 shadow-md rounded-md">
            <div class="flex items-center " :class="showInputs ? 'justify-center' : 'justify-between'">
                <div :class="showInputs ? 'text-center' : 'text-left'">
                    <h2 class="text-xl leading-tight ">Terms</h2>
                    <p class="text-sm text-gray-500">Click to open in Google</p>
                </div>
                <div v-if="!showInputs" class="flex items-center gap-2">
                    <ToggleButton v-model="showInputs" />
                    <RegenerateButton @click="generateTerms" />
                </div>
            </div>
            <hr class="border-black -mx-3">
            <SearchTerm v-for="term in terms" :key="term" :term="term" :clickedTerms="clickedTerms"
                @click="addClickedTerm" />
        </div>
    </main>
</template>
