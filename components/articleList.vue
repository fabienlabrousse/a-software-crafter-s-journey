<template>
    <section class="section has-background-light">
        <div class="container">

            <div class="columns">
                <div class="column is-one-third">
                    <h1 class="title">{{ lastArticlesTitle }}</h1>
                </div>
                <div class="column is-offset-one-third">
                    <div class="field has-addons">
                        <div class="control has-icons-left">
                            <input class="input" type="text" placeholder="Search..." v-model="searchInput">
                            <span class="icon is-small is-left">
                                <i class="fi-xnsuhl-search"/>
                            </span>
                        </div>
                        <div class="control">
                            <button class="button" v-on:click="clearSearch()">
                                Clear
                            </button>
                        </div>
                    </div>
                </div>
            </div>

            <section v-if="filteredArticles.length !== 0">
                <div class="columns is-centered" v-for="(article, i) in filteredArticles" :key="i" v-if="i % 2 == 0">
                    <div class="column">
                        <Article :article="article"/>
                    </div>
                    <div class="column" v-if="filteredArticles[i+1]">
                        <Article :article="filteredArticles[i+1]"/>
                    </div>
                </div>
            </section>
            <section v-else>
                <div class="container has-text-centered">
                    <h1 class="title">
                        No results
                    </h1>
                    <h2 class="subtitle">
                        Try to find with another keyword ?
                    </h2>
                </div>
            </section>

        </div>
    </section>
</template>

<script>
    import Article from '~/components/articleCard.vue'

    export default {
        props: ['articles'],
        components: {
            Article
        },
        data() {
            return {
                searchInput: "",
                lastArticlesTitle: 'Last articles'
            }
        },
        methods: {
            clearSearch: function () {
                this.searchInput = "";
            }
        },
        computed: {
            filteredArticles() {
                return this.articles.filter(article => {
                    return article.title.toLowerCase().includes(this.searchInput.toLowerCase())
                })
            }
        }
    }
</script>
