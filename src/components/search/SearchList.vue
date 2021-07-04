<template>
	<div>
		<h2 class="section-title">Search results</h2>
		
		<div class="fav-list">
			<favorite-item
				v-for="result in results" 
				:key="result.item.id"
				:title="result.item.title"
				:url="result.item.url"/>
		</div>
	</div>
</template>

<script>
import BookmarkService from '../bookmarks/BookmarksService.vue'
import SearchService from './SearchService.vue'
import FavoriteItem from '../favorites/FavoriteItem.vue'

export default {
	components: { FavoriteItem },

	props: {
		query: String
	},

	data: function() {
		return {
			results: [],
			flatBookmarks: []
		}
	},

	watch: {
		query: function(newVal) {
			this.results = SearchService.find(this.flatBookmarks, newVal)
			console.log(this.results)
		}
	},

	mounted: function() {
		BookmarkService.flatList("1").then(b =>
			this.flatBookmarks = b)
	}
}
</script>

<style scoped>
</style>