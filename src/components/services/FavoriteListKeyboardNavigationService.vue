<script>

/**
 * Service to enable keyboard navigation for the favorites list
 */
export default {

	/**
	 * Favorite list strucuture:
	 * 
	 * <div class="favorite-section">
	 *     <h2>Section title</h2>
	 * 
	 *     <div class="favorite-list">
	 *         <div class="favorite-column"> <!-- Many -->
	 *             <div class="fav-item"> <!-- Many... -->
	 *         </div>
	 *     </div>
	 * </div>
	 */

	// Starts the arrow key navigation
	// @arg key String - The arrow key pressed
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

	// Navigates to the first available fav-item. Usually trigerred
	// when there is nothing active on the page yet
	arrowNavigateFirstItem() {
		document
			.querySelector('.favorite-list .favorite-section')
			.querySelector('.favorite-list .favorite-column')
			.querySelector('.fav-item')
			.focus()
	},
	
	// Navigates up 
	// @arg activeElement HTMLObject - The currently active element,
	//                                 from which to navigate
	arrowNavigateUp(activeElement) {
		const previousSibling = activeElement.previousSibling
		if(previousSibling) previousSibling.focus()
		else this.focusLastItemPreviousSection(activeElement)
	},

	// Navigates down
	// @arg activeElement HTMLObject - The currently active element,
	//                                 from which to navigate
	arrowNavigateDown(activeElement) {
		const nextSibling = activeElement.nextSibling
		if(nextSibling) nextSibling.focus()
		else this.focusFirstItemNextSection(activeElement)
	},

	// Navigates left
	// @arg activeElement HTMLObject - The currently active element,
	//                                 from which to navigate
	arrowNavigateLeft(activeElement) {
		const previousColumn = activeElement.parentElement.previousSibling
		if(!previousColumn) return

		const siblings = Array.from(activeElement.parentElement.children)
		const currentIndex = siblings.indexOf(activeElement)

		const previousColumnItems = Array.from(previousColumn.children)
		previousColumnItems[currentIndex].focus()
	},

	// Navigates right
	// @arg activeElement HTMLObject - The currently active element,
	//                                 from which to navigate
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

	// Focus the first item in the next section. It also takes into account
	// which column the current element is in, and matches it to the next
	// section.
	// @arg element HTMLObject - The element from which to find the
	//                           next section
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

	// Focus the last item in the previous section. It also takes into account
	// which column the current element is in, and matches it to the previous
	// section.
	// @arg element HTMLObject - The element from which to find the
	//                           previous section
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

	// Gets the index of the current element in its parent list. Helpful to
	// help transitiona laterally between columns and keep the same position
	// focused.
	// @arg element HTLMObject - The element from which the index will be calculated  
	elementIndexOnList(element) {
		const parentColumn = element.closest('.favorite-column')
		const parentList = element.closest('.favorite-list')
		return Array
			.from(parentList.children)
			.indexOf(parentColumn)

	}
}
</script>
