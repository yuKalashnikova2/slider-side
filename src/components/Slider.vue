<script setup>
import { ref } from 'vue'

const props = defineProps({
    images: {
        type: Array,
        default: () => [],
    },
})

const emit = defineEmits(['current'])

const currentImageIndex = ref(0)
const imagesToShow = ref([])
const imagesPerSlide = ref(3)

const lastImage = ref('')

function showImages() {
    const start = Math.max(0, currentImageIndex.value - 1)
    const end = Math.min(
        props.images.length - 1,
        start + imagesPerSlide.value - 1
    )
    if (end === props.images.length - 1) {
        imagesToShow.value = props.images.slice(start)

        lastImage.value = props.images[0]
        imagesToShow.value.push(lastImage.value)
    } else {
        imagesToShow.value = props.images.slice(start, end + 1)
    }
    console.log(imagesToShow.value, 'Картинки входящие в массив')
}
function prevImage() {
    if (currentImageIndex.value > 0) {
        currentImageIndex.value--
        showImages()
    }
    emit('current', currentImageIndex)
    console.log(emit, 'emit', currentImageIndex.value)
}

function nextImage() {
    currentImageIndex.value =
        (currentImageIndex.value + 1) % props.images.length
    showImages()
    emit('current', currentImageIndex)
    console.log(emit, 'emit',  currentImageIndex.value)
}
</script>

<template>
    <div class="slider">
        <div class="slider-images" v-if="imagesToShow.length > 0">
            <div
                v-for="(image, index) in imagesToShow"
                :key="index"
                :class="[
                    'slider-images-container',
                    currentImageIndex === index
                        ? 'slider-images-container_border'
                        : '',
                ]"
            >
                <img :src="image" alt="Product image" />
            </div>
        </div>
        <div class="slider-images" v-else>
            <div
                v-for="(image, index) in images.slice(0, 3)"
                :key="image"
                :class="[
                    'slider-images-container',
                    index === 0 ? 'slider-images-container_border' : '',
                ]"
            >
                <img :src="image" alt="" />
            </div>
        </div>

        <div class="slider__arrows">
            <div
                class="slider__arrows__item slider__arrows__item_white"
                @click="prevImage"
            >
                <img src="/arrows-slider-card/arrow-black.svg" alt="arrow" />
            </div>
            <div
                class="slider__arrows__item slider__arrows__item_black"
                @click="nextImage"
            >
                <img src="/arrows-slider-card/arrow-white.svg" alt="arrow" />
            </div>
        </div>
    </div>
</template>

<style lang="scss" scoped>
.slider {
    display: flex;
    flex-direction: column;
}

.slider-images-container {
    width: 68px;
    height: 68px;
    overflow: hidden;
    border-radius: 9px;
    & img {
        width: 100%;
        height: 100%;
        border-radius: 9px;
    }
    &_border {
        border: 1px solid #2e3232;
    }
}

.slider-images {
    display: flex;
    flex-direction: column;
    gap: 20px;
}

.slider__arrows {
    display: flex;
    flex-direction: column;
    gap: 12px;
    align-items: center;
    justify-content: center;
    margin-top: 24px;
    @media (max-width: 992px) {
        flex-direction: row;
    }
    &__item {
        cursor: pointer;
        width: 22px;
        height: 22px;
        border-radius: 50%;
        display: flex;
        justify-content: center;
        align-items: center;
        @media (max-width: 992px) {
            width: 44px;
            height: 44px;
        }
        &_white {
            background: white;
            &:hover {
                background: #c3c5c5;
            }
            @media (max-width: 992px) {
                transform: rotate(270deg);
            }
        }
        &_black {
            background: #3c4242;
            &:hover {
                opacity: 0.6;
            }
            @media (max-width: 992px) {
                transform: rotate(270deg);
            }
        }
    }
}
</style>
