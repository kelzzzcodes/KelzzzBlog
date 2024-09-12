<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { useRouter, useRoute } from 'vue-router';
import axios from 'axios';
import { useRuntimeConfig } from '#imports';

const config = useRuntimeConfig();
const router = useRouter();
const route = useRoute();
const searchQuery = ref(route.query.q as string || '');

const results = ref<any[]>([]);
const loading = ref(true);

onMounted(async () => {
    try {
        if (searchQuery.value) {
            const response = await axios.get(`https://newsapi.org/v2/everything?q=${encodeURIComponent(searchQuery.value)}&apiKey=${config.public.apiKey}`);
            results.value = response.data.articles.map((article: any, index: number) => ({
                ...article,
                id: index.toString(), // Generate a unique ID
            }));
        }
    } catch (error) {
        console.error('Error fetching search results:', error);
    } finally {
        loading.value = false;
    }
});
</script>


<template>
    <div v-if="loading" class="text-white py-4">
        <p>Loading...</p>
    </div>
    <div v-else>
        <h3 class="text-white mb-4 font-extrabold text-xl">Search Results:</h3>
        <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-8">
            <NuxtLink v-for="item in results" :key="item.url" 
                :to="{ name: 'details-id', params: { id: item.url }, query: { fromSearch: 'true' } }"
                class="text-white">
                <Card class="w-auto bg-[#181A2A] rounded-xl">
                    <CardHeader>
                        <img :src="item.urlToImage" class="rounded-md" />
                    </CardHeader>
                    <CardContent>
                        <div class="flex flex-col gap-1">
                            <span class="inline-block max-w-max bg-[#4B6BFB0D] px-2 py-1 rounded-md">{{ item.author
                                }}</span>
                            <p class="text-white line-clamp-2">{{ item.title }}</p>
                        </div>
                    </CardContent>
                </Card>
            </NuxtLink>
        </div>
    </div>
</template>

