<template>
	<div class="favorite-list">
		<favorite-section
			v-for="bookmark in bookmarks"
			:key="bookmark.id"
			:title="bookmark.title"
			:children="bookmark.children"
			:cols="2" />
	</div>
</template>


<script>
import { EventBus } from '../../main'
import BookmarkService from '../bookmarks/BookmarksService.vue'
import FavoriteListKeyboardNavigationService from '../services/FavoriteListKeyboardNavigationService.vue'
import FavoriteSection from './FavoriteSection.vue'

export default {
	components: { FavoriteSection },
	name: 'NewTab',

	data: () => { 
		return {
			bookmarks: []
		}
	},

	created: function() {
		BookmarkService.all("1").then(b => { this.bookmarks = b })
		EventBus.$on('arrow-navigation', key => this.arrowNavigation(key))
	},

	methods: {
		arrowNavigation(key) {
			FavoriteListKeyboardNavigationService.arrowNavigation(key)
		}
	}
}
</script>


<style scoped>

</style>
