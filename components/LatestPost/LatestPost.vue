<script setup lang="ts">
import {
    Card,
    CardContent,
    CardDescription,
    CardFooter,
    CardHeader,
    CardTitle,
} from '@/components/ui/card'
</script>

<template>
    <section>
        <h3 class="text-white mb-4  font-extrabold text-xl">Latest Post:</h3>
        <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-8 items-center">
            <Card class="w-auto bg-[#181A2A] rounded-xl">
                <CardHeader>
                    <img src="/images/heroCardImage.jpeg " class="rounded-md" />
                </CardHeader>
                <CardContent>
                    <div class="flex flex-col gap-1">
                        <span class="inline-block max-w-max bg-[#4B6BFB0D] px-2 py-1 rounded-md  ">Technology</span>
                        <p class="text-white">The Impact of Technology on the Workplace: How Technology is Changing</p>
                    </div>
                </CardContent>
         
            </Card>


        </div>
    </section>
</template>