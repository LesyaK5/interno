<template>

    <div class="project__banner">
        <img :src="projectDetailsBanner" alt="баннер">
    </div>
    <div class="project center">
        <div class="project__about">
            <h2 class="project__heading">{{ projects[projectNumber].title }}</h2>
            <p class="project__text">{{ projects[projectNumber].text }}</p>
        </div>
        <div class="slider">
            <div class="imgBox">
                <img class="img" :src="projects[projectNumber].img[currentImgNumber]" :class="{ resize: resizeFlag }"
                    @dblclick="resizeFlag = !resizeFlag" @click="nextImage">
            </div>
            <div class="dots">
                <div v-for="(item, index) in projects[projectNumber].img">
                    <div v-if="index === currentImgNumber" class="dot dot-current"></div>
                    <div v-else class="dot" @click="nextDot(index)"></div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { mapState } from 'vuex';
import SliderComponent from './SliderComponent.vue';

export default {
    components: { SliderComponent },
    data() {
        return {
            currentImgNumber: 0,

            projectDetailsBanner: require('@/assets/img/projects/Banner_project_details.jpg'),
            projectNumber: null,
        }
    },
    methods: {
        // получаем id проекта из адресной строки
        getProjectNumber() {
            const projectNumberParam = parseInt(this.$route.params.projectNumber) - 1;
            this.projectNumber = isNaN(projectNumberParam) || projectNumberParam;
        },

        nextImage() {
            if (this.currentImgNumber === this.projects[this.projectNumber].img.length - 1)
                this.currentImgNumber = 0;
            else this.currentImgNumber++;
            this.currentImg = this.projects[this.projectNumber].img[this.currentImgNumber];
        },
        nextDot(index) {
            this.currentImgNumber = index;
            this.currentImg = this.projects[this.projectNumber].img[this.currentImgNumber];
        },
    },
    computed: {
        ...mapState(['projects']),
    },
    created() {
        this.getProjectNumber();
    },
}
</script>

<style lang="scss" scoped>
@import '@/assets/scss/helpers/_vars.scss';

.project {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;

    &__banner {
        margin-bottom: 200px;

        img {
            width: 100%;
            background: #CDA274;
        }
    }

    &__about {
        width: 55%;
        margin-bottom: 100px;
    }

    &__heading {
        font-family: DM Serif Display;
        font-size: 50px;
        font-weight: 400;
        line-height: 62.5px;
        letter-spacing: 0.02em;
        text-align: left;
        color: $headingColor;
        margin-bottom: 12px;
    }

    &__text {
        font-family: Jost;
        font-size: 22px;
        font-weight: 400;
        line-height: 33px;
        letter-spacing: 0.01em;
        text-align: left;
        color: $textColor;
    }
}

.slider {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 8px;
    margin-bottom: 200px;
}

.img {
    border-radius: 50px;
    margin-bottom: 16px;
}

.resize {
    width: 100vw;
}

.dots {
    display: flex;
    justify-content: center;
}

.dot {
    border-radius: 50%;
    width: 16px;
    height: 16px;
    border: solid 1px $headingColor;
    margin: 4px;
    cursor: pointer;
}

.dot-current {
    background-color: $headingColor;
}
</style>