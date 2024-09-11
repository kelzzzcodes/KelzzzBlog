<script setup>
const { title, description, imageUrl, source, author, publishedAt } = defineProps({
    title: String,
    description: String,
    imageUrl: String,
    source: String,
    author: String,
    publishedAt: String,
});

const year = publishedAt.substring(0, 4);
</script>

<template>
    <div class="relative md:h-[550px] h-[400px] w-full ">
        <img :src="imageUrl" class="w-full" />
        <div class="absolute inset-0  bg-black/40 flex items-end p-4">
            <div class="text-white w-full max-w-4xl">
                <div class="flex flex-col gap-3 items-start">
                    <p class="text-sm md:text-base">{{ source }}</p>
                    <h2 class="text-lg md:text-xl font-semibold leading-tight text-left">{{ title }}</h2>
                    <ul class="flex gap-2 sm:gap-4">
                        <li class="text-sm">{{ author }}</li>
                        <li class="text-sm">{{ year }}</li>
                    </ul>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
/* Additional CSS if needed */
</style>
