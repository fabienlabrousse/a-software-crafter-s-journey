<template>
    <ArticleList :articles="articles"/>
</template>

<script>
    import ArticleList from '~/components/articleList.vue'
    import fakeArticles from '../static/mocks/article-list'

    export default {
        async asyncData({params}) {
            let articles = [];

            if (process.env.NODE_ENV === 'development') {
                articles = fakeArticles;

                return {
                    articles: articles
                }
            }

            const files = require.context('@/articles', true, /.*\.md$/);
            const articleFiles = files.keys();

            for (const articleFile of articleFiles) {
                let fileName = articleFile.substr(2, articleFile.length);

                const fileContent = await import(`~/articles/${fileName}`);

                articles.push({
                    title: fileContent.attributes.title,
                    author: fileContent.attributes.author,
                    published_date: fileContent.attributes.published_date,
                    description: fileContent.attributes.description,
                    tags: fileContent.attributes.tags.split(','),
                    file_name: fileName,
                    route: `/${fileName.substr(0, fileName.lastIndexOf('.'))}`
                });
            }

            return {
                articles: articles
            }
        },
        components: {
            ArticleList
        }
    }
</script>
