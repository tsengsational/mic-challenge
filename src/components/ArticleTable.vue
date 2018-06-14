<template>
    <div class="article-table" >
        <table>
            <tr class="table-header">
                <th><div class="header-title">Unpublished Articles ({{articles.length}})</div></th>
                <th><div class="header-author">Author</div></th>
                <th><div class="header-words"> Words <span class="sort-icon" @click="$emit('sortChange', 'words')"><font-awesome-icon :icon="sort" /></span></div></th>
                <th><div class="header-submitted">Submitted <span class="sort-icon" @click="$emit('sortChange', 'submitted')"><font-awesome-icon :icon="sort" /></span></div></th>
            </tr>
            <div class="table-content">
                <article-row v-for="(article, key) in articles" :key="key" :index="key" :article="article" ></article-row>
            </div>
        </table>    
    </div>
</template>

<script>
import ArticleRow from './ArticleRow'
import FontAwesomeIcon from '@fortawesome/vue-fontawesome'
import faSort from '@fortawesome/fontawesome-free-solid/faSort'

export default {
    components: {
        ArticleRow,
        FontAwesomeIcon
    },
    props: ["articles"],
    computed: {
        sort: function() {
            return faSort
        }
    }
}
</script>

<style lang="scss">
    .article-table {
        width: 80vw;
        left: 10vw;
        position: relative;
        table {
            border-collapse: collapse;
            th {
                text-align: left;
                padding: 20px 10px;
                background: linear-gradient(#c21e46, #8b1130);
                color: #fff;
                text-transform: uppercase;
                font-weight: 900;
                border: 1px #8b1130 solid;
                z-index: 1;
                background-clip:padding-box;
                white-space: nowrap;
                box-sizing: border-box;
            }

            .table-content {
                position: relative;
                top: 59px;
            }


            .table-header {
                position: fixed;
                top: 0;
                z-index: 1;
                background: linear-gradient(#c21e46, #8b1130);
                width: 80vw;
                .header-title {
                    min-width: 50vw;
                }
                .header-author, .header-words, .header-submitted {
                    min-width: 8vw;
                }
            }
        }
    }
</style>
