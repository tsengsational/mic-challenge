<template>
    <div class="article-table" >
        <table>
            <tr>
                <th>Unpublished Articles ({{articles.length}})</th>
                <th>Author</th>
                <th>Words</th>
                <th>Submitted</th>
            </tr>
            <article-row v-for="(article, key) in articles" :key="key" :article="article" ></article-row>
        </table>    
    </div>
</template>

<script>
import ArticleRow from './ArticleRow'

export default {
    components: {
        ArticleRow
    },
    props: ["articles"]
}
</script>

<style lang="scss">
    .article-table {
        table {
            width: 100%;
        }
    }
</style>
