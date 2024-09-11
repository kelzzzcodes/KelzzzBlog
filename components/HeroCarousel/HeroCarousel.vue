<script setup lang="ts">
import Autoplay from 'embla-carousel-autoplay'
import { Carousel, CarouselContent, CarouselItem } from '@/components/ui/carousel'
import HeroCard from '../ui/heroCard/heroCard.vue';
import axios from 'axios';
import { ref, onMounted } from 'vue';
import { Skeleton } from '../components/ui/skeleton'; // Import your Skeleton component

const config = useRuntimeConfig();

interface HeroItems {
  title: string;
  description: string;
  urlToImage: string;
  source: { name: string };
  author: string;
  publishedAt: string;
}

const heroItems = ref<HeroItems[]>([]);
// State to track loading status
const loading = ref(true);

// Function to get random heroItems
const getRandomheroItems = (heroItemsArray: any[], count: number) => {
  const shuffled = heroItemsArray.sort(() => 0.5 - Math.random());
  return shuffled.slice(0, count);
};

// Fetch heroItems on mounted
onMounted(async () => {
  try {
    const response = await axios.get(`https://newsapi.org/v2/everything?domains=techcrunch.com,thenextweb.com&apiKey=${config.public.apiKey}`);
    const allheroItems = response.data.articles;
    // Get 3 random heroItems from the fetched data
    heroItems.value = getRandomheroItems(allheroItems, 3);
  } catch (error) {
    console.error('Error fetching heroItems:', error);
  } finally {
    // Set loading to false once API call is done
    loading.value = false;
  }
});

const plugin = Autoplay({
  delay: 2000,
  stopOnMouseEnter: true,
  stopOnInteraction: false,
});
</script>

<template>
  <div>
    <!-- Show skeleton loader while loading is true -->
    <div v-if="loading" class="flex flex-col space-y-4">
      <!-- Skeleton for the carousel content -->
      <Skeleton class="h-[350px]  w-[95%]  items-center justify-center mx-auto rounded-xl" />

    </div>

    <!-- Show carousel content once data is loaded -->
    <div v-else>
      <Carousel :plugins="[plugin]" @mouseenter="plugin.stop"
        @mouseleave="[plugin.reset(), plugin.play(), console.log('Running')]" :opts="{ align: 'center', loop: true }"
        class="w-full h-full ">
        <CarouselContent class="w-full h-full">
          <CarouselItem v-for="(items, index) in heroItems" :key="index" class="text-center w-full flex">
            <div class="w-[95%] bg-stone-200 flex justify-center mx-auto rounded-2xl overflow-hidden">
              <!-- Pass heroItems data to HeroCard as props -->
              <HeroCard :title="items.title" :source="items.source.name" :author="items.author"
                :publishedAt="items.publishedAt" :imageUrl="items.urlToImage" />
            </div>
          </CarouselItem>
        </CarouselContent>
      </Carousel>
    </div>
  </div>
</template>
