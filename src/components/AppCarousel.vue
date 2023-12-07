<template>
    <section class="carousel">
        <div class="carousel__container" v-if="checkData">
                <transition-group name="fade" tag="div" class="carousel__wrapper">
                    <img
                        v-for="image in visibleImages"
                        :key="image.id"
                        :src="image.download_url"
                        alt="img"
                        class="carousel__img"
                        :class="{ selected: isSelected(image) }"
                        @click="toggleSelection(image)"
                    >
                </transition-group>
        <button class="btn btn-prev" @click="scrollImages(-1)">
            <img class="btn__arrow" src="../assets/img/arrow-left.svg" alt="arrow icon">
        </button>
        <button class="btn btn-next" @click="scrollImages(1)">
            <img class="btn__arrow" src="../assets/img/arrow-right.svg" alt="arrow icon">
        </button>
        </div>
        <transition-group name="block-fade">
            <div v-if="selectedImages.length > 0" class="selected-images">
                <h2 class="selected-images__title">Selected Images</h2>
                <transition-group name="list-fade" tag="ul" class="selected-images__list">
                    <li 
                        class="selected-images__item" 
                        v-for="(image, index) in selectedImages" 
                        :key="index"
                        @click="scrollToImage(index)"
                    >
                        {{ image.download_url }}
                    </li>
                </transition-group>
            </div>
        </transition-group>
    </section>
</template>

<script setup>
    import { ref, computed, watch, defineProps, onMounted, onBeforeUnmount } from 'vue';
    
    const props = defineProps({
        images: Array,
        default: () => []

    });

    const currentIndex = ref(0);
    const visibleImages = ref([]);
    const selectedImages = ref([]);
    
    const checkData = computed(() => {
        return props.images && props.images.length > 0;

    });

    const getVisibleImageCount = () => {
        const screenWidth = window.innerWidth;
        if (screenWidth >= 1200) {
            return 5;
        } else if (screenWidth >= 900) {
            return 4;
        } else if (screenWidth >= 600) {
            return 3;
        } else {
            return 1;
        } 

};  

    const updateVisibleImages = () => {
        const totalImages = props.images.length;
        const startIndex = currentIndex.value;
        const endIndex = startIndex + getVisibleImageCount() - 1;

        visibleImages.value = props.images.slice(startIndex, endIndex + 1).concat(
            endIndex >= totalImages ? props.images.slice(0, endIndex - totalImages + 1) : []

        );

    };

    watch(() => {
        updateVisibleImages();

    });

    const scrollImages = (step) => {
        currentIndex.value = (currentIndex.value + step + props.images.length) % props.images.length;

        if (currentIndex.value >= props.images.length) {
            currentIndex.value = props.images.length;
        } else if (currentIndex.value < 0) {
            currentIndex.value = props.images.length - 1;
        }

    };

    const isSelected = (image) => {
        return selectedImages.value.some((selectedImage) => selectedImage.id === image.id);

    };

    const toggleSelection = (image) => {
        const index = selectedImages.value.findIndex((selectedImage) => selectedImage.id === image.id);

        if (index !== -1) {
            selectedImages.value.splice(index, 1);

        } else {
            selectedImages.value.push(image);

        }

    };

    onMounted(updateVisibleImages);

    const handleResize = () => {
        location.reload();

    };

    onMounted(() => {
        window.addEventListener('resize', handleResize);

    });

    onBeforeUnmount(() => {
        window.removeEventListener('resize', handleResize);

    });

    const scrollToImage = (index) => {
        currentIndex.value = index;
        
    };

</script>
