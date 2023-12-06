<template>
    <div>
        <ImageCarousel :images="imagesData"/>
    </div>
</template>

<script setup>
import axios from 'axios';

import { ref, onMounted } from 'vue';

import ImageCarousel from './ImageCarousel.vue';

const apiUrl = "https://picsum.photos/v2/list";
const imagesData = ref([]);

const getImagesData = async () => {
    try {
        const response = await axios.get(apiUrl);
        imagesData.value = response.data;
        console.log(imagesData.value);
    } catch (error) {
        console.error('Error fetching images:', error);
    }
};

onMounted(() => {
    getImagesData();

});

</script>

<script setupe>

</script>

