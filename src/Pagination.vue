<template>
    <nav class="pagination" v-if="data.total > data.per_page">
        <a class="pagination-previous" v-if="data.prev_page_url" @click.prevent="selectPage(data.current_page - 1)">&laquo;</a>
        <a class="pagination-previous is-disabled" v-else>&laquo;</a>
        <a class="pagination-next" v-if="data.next_page_url" @click.prevent="selectPage(data.current_page + 1)">&raquo;</a>
        <a class="pagination-next is-disabled" v-else>&raquo;</a>
        <ul class="pagination-list">
            <li v-for="n in getPages()">
                <a class="pagination-link" :class="{ 'is-current': n == data.current_page }" @click.prevent="selectPage(n)">{{ n }}</a>
            </li>
        </ul>
    </nav>
</template>
<script>
'use strict';
import Vue from 'vue';
export default {
    props: {
		data: {
			type: Object,
			default() {
				return {
					current_page: 1,
					data: [],
					from: 1,
					last_page: 1,
					next_page_url: null,
					per_page: 10,
					prev_page_url: null,
					to: 1,
					total: 0,
				}
			}
		},
		limit: {
			type: Number,
			default: 0
		}
	},
    methods: {
		selectPage(page) {
			this.$emit('pagination-change-page', page);
		},
		getPages() {
			if (this.limit === -1) {
				return 0;
			}

			if (this.limit === 0) {
				return this.data.last_page;
			}

        	let start = this.data.current_page - this.limit;
            let end   = this.data.current_page + this.limit + 1;
        	let pages = [];
        	let index;

        	start = start < 1 ? 1 : start;
        	end   = end >= this.data.last_page ? this.data.last_page + 1 : end;

        	for (index = start; index < end; index++) {
        		pages.push(index);
        	}
        	return pages;
		}
	}
}
</script>
