<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { useRoute } from 'vue-router';
import axios from 'axios';
import { useRuntimeConfig } from '#imports';

const config = useRuntimeConfig();
const route = useRoute();
const postId = route.params.id as string;

interface LatestPost {
    title: string;
    description: string;
    urlToImage: string;
    author: string;
    id: string;
    publishedAt: string;
}

const post = ref<LatestPost | null>(null);
const loading = ref(true);

onMounted(async () => {
    try {
        const response = await axios.get(`https://newsapi.org/v2/top-headlines?sources=bbc-news&apiKey=${config.public.apiKey}`);
        const posts = response.data.articles.map((article: any, index: number) => ({
            ...article,
            id: index.toString(),  // Generate unique ID
        }));
        const fetchedPost = posts.find((post: LatestPost) => post.id === postId);
        post.value = fetchedPost || null;
    } catch (error) {
        console.error('Error fetching post details:', error);
    } finally {
        loading.value = false;
    }
});

const year = computed(() => {
    return post.value ? post.value.publishedAt.substring(0, 4) : '';
});

</script>

<template>
    <div v-if="loading">
        <Skeleton class="h-[350px] w-[95%] mx-auto rounded-xl" />
    </div>
    <div v-else-if="post" class="flex items-center justify-center py-8 text-white">
        <div class="flex flex-col gap-10  ">
            <div class="flex flex-col gap-2">
                <h1 class="w-[75%]">{{ post.title }}</h1>
                <ul class="flex gap-2">

                    <li>
                        {{ post.author }}
                    </li>
                    <li>
                        {{ year }}
                    </li>
                </ul>
            </div>
            <img :src="post.urlToImage" alt="Post Image"  class="rounded-xl"/>
            <p>{{ post.description }}</p>

        </div>
    </div>

    <div v-else>
        <p>Post not found.</p>
    </div>
</template>
