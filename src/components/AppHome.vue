<template>
    <section class="header">
        <img class="header__logo" src="../assets/img/carousel-svgrepo-com.svg" alt="logo img">
        <h1 class="header__title">Image carousel</h1>
    </section>
    <div class="wrapper">
        <AppCarousel :images="imagesData"/>
    </div>
</template>

<script setup>
    import axios from 'axios';

    import { ref, onMounted } from 'vue';

    import AppCarousel from './AppCarousel.vue';

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

