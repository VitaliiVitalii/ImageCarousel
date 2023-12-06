<template>
    <div class="image-carousel">
        <div class="carousel-container" v-if="checkData">
            <img 
                :src="imageUrl(currentIndex)" 
                alt="img"
            >
        </div>
        <button @click="scrollImages(-1)">Prev</button>
        <button @click="scrollImages(1)">Next</button>
    </div>
</template>

<script setup>
    import { ref, computed, defineProps } from 'vue';

    const props = defineProps({
        images: Array,
        default: () => []

    });

    const currentIndex = ref(0);

    const checkData = computed(() => {
        return props.images && props.images.length > 0;

    });

    const imageUrl = (index) => {
        return props.images[index].download_url;
        
    };

    const scrollImages = (step) => {
        currentIndex.value = (currentIndex.value + step + props.images.length) % props.images.length;
        console.log(currentIndex.value);

    };

</script>

<style lang="scss">
    img {
        width: 200px;
    }
</style>