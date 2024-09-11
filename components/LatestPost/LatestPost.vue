<script setup lang="ts">
import axios from 'axios';
import { ref, onMounted } from 'vue';

const config = useRuntimeConfig();

interface LatestPost {
    title: string;
    urlToImage: string;
    author: string;
    id: string;

}

const allLatestPost = ref<LatestPost[]>([]);
// State to track loading status
const loading = ref(true);


onMounted(async () => {
    try {
        const response = await axios.get(`https://newsapi.org/v2/top-headlines?sources=bbc-news&apiKey=${config.public.apiKey}`);
        allLatestPost.value = response.data.articles.map((article: LatestPost, index: number) => ({
            ...article, // Spreading all properties of the current article object
            id: index.toString(),  // Using the index to generate a unique ID for each post
        }));
        console.log(allLatestPost);

    } catch (error) {
        console.error('Error fetching heroItems:', error);
    } finally {
        // Set loading to false once API call is done
        loading.value = false;
    }
});

</script>

<template>
    <section>
        <h3 class="text-white mb-4  font-extrabold text-xl">Latest Post:</h3>
        <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-8 items-center">

            <Skeleton v-if="loading" v-for="index in 8" :key="index" class="h-[350px]  rounded-xl" />

            <NuxtLink v-else v-for="(item) in allLatestPost" :key="item.id"
                :to="{ name: 'details-id', params: { id: item.id } }">

                <Card class="w-auto bg-[#181A2A] rounded-xl">
                    <CardHeader>
                        <img :src="item.urlToImage" class="rounded-md" />
                    </CardHeader>
                    <CardContent>
                        <div class="flex flex-col gap-1">
                            <span class="inline-block max-w-max bg-[#4B6BFB0D] px-2 py-1 rounded-md  ">{{ item.author
                                }}</span>
                            <p class="text-white line-clamp-2">{{ item.title }}</p>
                        </div>
                    </CardContent>

                </Card>
            </NuxtLink>



        </div>
    </section>
</template>
