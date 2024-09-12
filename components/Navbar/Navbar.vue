<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import { Search } from 'lucide-vue-next';
import { Input } from '@/components/ui/input';
import { useRouter } from 'vue-router';

const items = ref([
    { title: 'Home', path: '/' },
    { title: 'Contact', path: '/' }
]);

const isScrolled = ref(false);
const searchQuery = ref('');
const router = useRouter();

const handleScroll = () => {
    isScrolled.value = window.scrollY > 30;
};

const handleSearch = () => {
    if (searchQuery.value.trim()) {
        router.push({ name: 'search', query: { q: searchQuery.value } });
        searchQuery.value = ''; // Clear the search input after navigation
    }
};

onMounted(() => {
    window.addEventListener('scroll', handleScroll);
});

onUnmounted(() => {
    window.removeEventListener('scroll', handleScroll);
});
</script>

<template>
    <nav
        :class="['flex items-center justify-between py-5 text-white border-b-2 border-stone-300 px-8 fixed top-0 w-full transition-all duration-300 z-50', { 'bg-stone-300 text-black': isScrolled }]">
        <NuxtLink href="/" class="text-xl font-extrabold">Kelzzz<span>Blog</span></NuxtLink>
        <div class="flex-grow flex items-center justify-center mx-8">
            <div class="flex gap-12">
                <NuxtLink :href='item.path' v-for="(item, index) in items" :key='index'>
                    <span>{{ item.title }}</span>
                </NuxtLink>
            </div>
        </div>
        <div class="relative max-w-sm flex-1">
            <Input v-model="searchQuery" id="search" type="text" placeholder="Search..."
                class="pl-2 outline-none py-1 bg-[#A1A1AA] border-none w-full" />
            <button @click="handleSearch" class="absolute right-0 inset-y-0 flex items-center justify-center px-2">
                <Search class="size-6" />
            </button>
        </div>
    </nav>
</template>
