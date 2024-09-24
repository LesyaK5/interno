<template>
    <div class="slider">
        <img v-for="item in projectImages" :key="item.id" :src="currentImg" alt="">
        <div class="imgBox">
            <img class="img" :src="currentImg" :class="{ resize: resizeFlag }" @dblclick="resizeFlag = !resizeFlag"
                @click="nextImage">
        </div>
    </div>
</template>

<script>
import { mapState, mapGetters } from 'vuex';

export default {
    data() {
        return {
            currentImg: require("@/assets/img/projects/Photo.png"),
            currentImgNumber: 0,
            resizeFlag: false,
            projectNumber: null,
        }
    },
    methods: {
        nextImage() {
            if (this.currentImgNumber === this.projectImages.length - 1)
                this.currentImgNumber = 0;
            else this.currentImgNumber++;
            this.currentImg = this.projectImages[this.currentImgNumber];
        },
        nextDot(index) {
            this.currentImgNumber = index;
            this.currentImg = this.projectImages[this.currentImgNumber];
        },
    },
    computed: {
        ...mapState({
            projectImages: (state) => state.projects[this.projectNumber - 1].img,
        }),
    },
    created() {
        const projectNumberParam = parseInt(this.$route.params.projectNumber);
        this.projectNumber = isNaN(projectNumberParam) || projectNumberParam;
    }
}
</script>

<style lang="scss" scoped>
@import '@/assets/scss/helpers/_vars.scss';

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

#prev,
#next {
    background-color: $headingColor;
    color: white;
    padding: 1rem 2rem;
    border-radius: 10px;
}
</style>