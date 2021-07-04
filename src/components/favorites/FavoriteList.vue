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
			const activeElement = document.activeElement

			if (activeElement.className != 'fav-item') {
				this.arrowNavigateFirstItem()
				return
			}

			switch(key) {
				case 'ArrowUp':
					this.arrowNavigateUp(activeElement)
					break
				case 'ArrowDown':
					this.arrowNavigateDown(activeElement)
					break
				case 'ArrowLeft':
					this.arrowNavigateLeft(activeElement)
					break
				case 'ArrowRight':
					this.arrowNavigateRight(activeElement)
			}
		},

		// Navigates to the first available fav item
		arrowNavigateFirstItem() {
			document
				.querySelector('.favorite-list .favorite-section')
				.querySelector('.favorite-list .favorite-column')
				.querySelector('.fav-item')
				.focus()
		},
		
		arrowNavigateUp(activeElement) {
			const previousSibling = activeElement.previousSibling
			if(previousSibling) previousSibling.focus()
			else this.focusLastItemPreviousSection(activeElement)
		},
		arrowNavigateDown(activeElement) {
			const nextSibling = activeElement.nextSibling
			if(nextSibling) nextSibling.focus()
			else this.focusFirstItemNextSection(activeElement)
		},
		arrowNavigateLeft(activeElement) {
			const previousColumn = activeElement.parentElement.previousSibling
			if(!previousColumn) return

			const siblings = Array.from(activeElement.parentElement.children)
			const currentIndex = siblings.indexOf(activeElement)

			const previousColumnItems = Array.from(previousColumn.children)
			previousColumnItems[currentIndex].focus()
		},
		arrowNavigateRight(activeElement) {
			const nextColumn = activeElement.parentElement.nextSibling
			if(!nextColumn) return

			const siblings = Array.from(activeElement.parentElement.children)
			const currentIndex = siblings.indexOf(activeElement)

			const nextColumnItems = Array.from(nextColumn.children)
			const nextItem = nextColumnItems[currentIndex]
			
			if(nextItem) 
				nextItem.focus()
			else
				nextColumnItems[currentIndex-1].focus()
		},

		focusFirstItemNextSection(element) {
			const parentSection = element.closest('.favorite-section')
			if(!parentSection.nextSibling) return
			
			const currentIndex = this.elementIndexOnList(element)
			const nextList = parentSection
				.nextSibling
				.querySelector('.favorite-list')
				.children[currentIndex]
			
			if(nextList) 
				nextList.children[0].focus()
			else
				parentSection // TODO: Fix this case
					.nextSibling 
					.querySelector('.favorite-list .favorite-column')
					.children[0]
					.focus()
		},
		focusLastItemPreviousSection(element) {
			const parentSection = element.closest('.favorite-section')
			if (!parentSection.previousSibling) return

			const currentIndex = this.elementIndexOnList(element)
			const previousList = parentSection
				.previousSibling
				.querySelector('.favorite-list')
				.children[currentIndex]

			if(previousList)
				previousList
					.children[previousList.children.length - 1]
					.focus()
			else
				parentSection // TODO: Fix this case
					.previousSibling
					.querySelector('.favorite-list .favorite-column')
					.children[0]
					.focus
		},

		elementIndexOnList(element) {
			const parentColumn = element.closest('.favorite-column')
			const parentList = element.closest('.favorite-list')
			return Array
				.from(parentList.children)
				.indexOf(parentColumn)

		}
	}
}
</script>


<style scoped>

</style>
