<template>
    <div class="blog-banner">
        <img :src="blogDetailsBanner" alt="баннер">
    </div>
    <section class="blog_flex center">
        <div class="blog-details">
            <div v-for="article in getFilteredArticles" :key="article.id" class="blog-details__article">
                <div class="blog-details__text">
                    <h2 class="blog-details__heading">{{ article.heading }}</h2>
                </div>
                <div class="blog-details__img">
                    <img :src="article.img" alt="статья">
                </div>
                <div class="blog-details__block">
                    <p class="blog-details__date">{{ article.date }}</p>
                    <div class="blog-details__date">Interior / Home / Decore</div>
                </div>
                <div class="blog-details__textBox">
                    <p class="blog-details__text"> {{ article.text }}
                    </p>
                </div>
            </div>
        </div>
        <BlogCategoriesComponent />
    </section>
</template>

<script>
import { mapActions, mapState, mapGetters } from 'vuex';
import BlogCategoriesComponent from "../components/BlogCategoriesComponent.vue";


    export default {
        components: {
            BlogCategoriesComponent,
        },
        data() {
            return {
                blogDetailsBanner: require('@/assets/img/blog/Banner_blog_details.jpg'),
                articleNumber: null,
            }
        },
        methods: {
            ...mapActions(['load_article_tag']),
            getArticleNumber() {
                // получаем id статьи из адресной строки
                const articleNumberParam = parseInt(this.$route.params.articleNumber) - 1;
                this.articleNumber = isNaN(articleNumberParam) || articleNumberParam;
            },
            changeArticleCategoryTag() {
                // получили id статьи
                this.getArticleNumber();
                // по id статьи вычисляем его tag
                const articleTag = this.getAllArticles[this.articleNumber].tag;
                // меняем активный tag
                this.load_article_tag(articleTag);
            }
        },
        computed: {
            ...mapState(['articleTags']),

            ...mapGetters(['getAllArticles', 'getFilteredArticles']),
        },

        created() {
            this.changeArticleCategoryTag();
        },
    }
</script>

<style lang="scss" scoped>
@import '@/assets/scss/style.scss';

.blog-banner {
    margin-bottom: 200px;

    img {
        width: 100%;
        background: #CDA274;
    }
}

.blog_flex {
    display: flex;
    margin-bottom: 96px;
}

.blog-details {
    width: 65%;

    &__img img {
        border-radius: 50px;
        width: 100%;
        margin-bottom: 46px;
    }

    &__heading {
        font-family: DM Serif Display;
        font-size: 50px;
        font-weight: 400;
        line-height: 63px;
        letter-spacing: 0.02em;
        text-align: left;
        color: $headingColor;
        margin-bottom: 16px;
    }

    &__block {
        display: flex;
        justify-content: space-between;
        align-items: center;
    }

    &__text,
    &__date {
        white-space: pre-line;
        font-family: Jost;
        font-size: 22px;
        font-weight: 400;
        line-height: 33px;
        letter-spacing: 0.01em;
        text-align: left;
        margin-bottom: 48px;
        color: $textColor;
    }

    &__text {
        text-align: justify;

        &_span {
            font-family: DM Serif Display;
            font-size: 20px;
            font-weight: 400;
            line-height: 25px;
            letter-spacing: 0.02em;
            text-align: left;
            color: #CDA274;
        }
    }

    &__item {
        display: flex;
        gap: 11px;
    }
}

.sidebar {
    position: sticky;
    top: 20%;
    width: 30%;
    margin-left: 4%;
    margin-bottom: 10%;

    &__heading {
        font-family: DM Serif Display;
        font-size: 25px;
        font-weight: 400;
        line-height: 31px;
        letter-spacing: 0.02em;
        text-align: left;
        color: $headingColor;
        margin-bottom: 24px;
    }

    &__tags {
        display: flex;
        gap: 10px;
        flex-wrap: wrap;
    }
}

.tag {
    border-radius: 10px;
    padding: 1rem 2rem;
    background: #F4F0EC;
    color: $headingColor;
    font-family: Jost;
    font-size: 18px;
    font-weight: 400;
    line-height: 23px;
    letter-spacing: 0.02em;
    text-align: center;

    &:hover,
    .active {
        background: $headingColor;
        color: #FFF;
    }
}

.active {
    background: $headingColor;
    color: #FFF;
}
</style>