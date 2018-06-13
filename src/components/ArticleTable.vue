<template>
    <div class="article-table" >
        <table>
            <tr>
                <th>Unpublished Articles ({{articles.length}})</th>
                <th>Author</th>
                <th>Words <button @click="$emit('sortChange', 'words')">V</button></th>
                <th>Submitted <button @click="$emit('sortChange', 'submitted')">V </button> </th>
            </tr>
            <article-row v-for="(article, key) in articles" :key="key" :index="key" :article="article" ></article-row>
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
            border-collapse: collapse;
            th {
                text-align: left;
                padding: 20px 10px;
                background: linear-gradient(#c21e46, #8b1130);
                color: #fff;
                text-transform: uppercase;
                font-weight: 900;
                border: 1px #8b1130 solid;
            }
        }
    }
</style>
