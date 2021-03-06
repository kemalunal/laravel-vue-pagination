<template>
    <renderless-laravel-vue-pagination :data="data" :limit="limit" :pageNumberLimit="pageNumberLimit" v-on:pagination-change-page="onPaginationChangePage">
        <ul class="pagination" v-if="data.total > data.per_page" slot-scope="{ data, limit, pageRange, prevButtonEvents, nextButtonEvents, pageButtonEvents, firstButtonEvents, lastButtonEvents, getlastPage }">
            <li class="page-item pagination-prev-nav" v-if="data.prev_page_url && firstAndLast">
                <a class="page-link" href="#" aria-label="First" v-on="firstButtonEvents">
                    <slot name="first-nav">
                        <span aria-hidden="true">&larr;</span>
                        <span class="sr-only">First</span>
                    </slot>
                </a>
		    </li>
            <li class="page-item pagination-prev-nav" v-if="data.prev_page_url">
                <a class="page-link" href="#" aria-label="Previous" v-on="prevButtonEvents">
                    <slot name="prev-nav">
                        <span aria-hidden="true">&laquo;</span>
                        <span class="sr-only">Previous</span>
                    </slot>
                </a>
            </li>
            <li class="page-item pagination-page-nav" v-for="(page, key) in pageRange" :key="key" :class="{ 'active': page == data.current_page }">
                <a class="page-link" href="#" v-on="pageButtonEvents(page)">{{ page }}</a>
            </li>
            <li class="page-item pagination-next-nav" v-if="data.next_page_url && data.current_page != getlastPage()">
                <a class="page-link" href="#" aria-label="Next" v-on="nextButtonEvents">
                    <slot name="next-nav">
                        <span aria-hidden="true">&raquo;</span>
                        <span class="sr-only">Next</span>
                    </slot>
                </a>
            </li>
            <li class="page-item pagination-next-nav" v-if="data.next_page_url && firstAndLast && data.current_page != getlastPage()">
                <a class="page-link" href="#" aria-label="Last" v-on="lastButtonEvents">
                    <slot name="last-nav">
                        <span aria-hidden="true">&rarr;</span>
                        <span class="sr-only">Last</span>
                    </slot>
                </a>
		    </li>
        </ul>
    </renderless-laravel-vue-pagination>
</template>

<script>
import RenderlessLaravelVuePagination from './RenderlessLaravelVuePagination.vue';

export default {
    props: {
        data: {
            type: Object,
            default: () => {
                return {
                    current_page: 1,
                    data: [],
                    from: 1,
                    last_page: 1,
                    next_page_url: null,
                    per_page: 10,
                    prev_page_url: null,
                    to: 1,
                    total: 0
                }
            }
        },
        limit: {
            type: Number,
            default: 0
        },
         firstAndLast: {
            type: Boolean,
            default: false
        },
        pageNumberLimit: {
            type: Number,
            default: 0
        },
    },

    methods: {
        onPaginationChangePage (page) {
            this.$emit('pagination-change-page', page);
        }
    },

    components: {
        RenderlessLaravelVuePagination
    }
}
</script>
