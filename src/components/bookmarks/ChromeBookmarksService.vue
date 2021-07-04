<script>

/**
 * Wrapper for the Chrome.bookmarks API.
 */
export default {
	// Gets the entire bookmark tree
	getTree() {
		return new Promise(resolve => {
			chrome.bookmarks.getTree(b => resolve(b))
		})
	},

	// Gest the bookmark tree starting at a specific node
	// @arg id String - The id of the root node
	getSubtree(id) {
		return new Promise(resolve => {
			chrome.bookmarks.getSubTree(id, b => resolve(b[0].children))
		})
	},

	// Gets chrome bookmarks
	// @arg rootId String - The id of the root node. If not provided, will all
	//                      bookmarks will be fetched.
	all(rootId = null) {
		return new Promise(resolve => {
			var bookmarks = []
			var fetch =  rootId ? this.getSubtree(rootId) : this.getTree()

			fetch.then(b => {
				bookmarks = b.map(i => this.remapFavorite(i))
				resolve(bookmarks)
			})
		})
	},

	// Remaps the Chrome bookmark node to a Jumpstart one
	// @arg favorite Object - The favorite object from Chrome
	remapFavorite(favorite) {
		var remapped = { id: favorite.id, title: favorite.title }

		if (favorite.url) 
		remapped = { ...remapped, type: 'bookmark', url: favorite.url }

		if (favorite.children) 
		remapped = { ...remapped, type: 'folder', 
			children: favorite.children.map(i => this.remapFavorite(i)) }

		return remapped;
	}
}
</script>