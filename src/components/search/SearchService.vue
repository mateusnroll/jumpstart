<script>
import Fuse from 'fuse.js'

/**
 * Service to find favorites
 */
export default {
	// Queries the title of a bookmark list, and return a list of results
	// @arg flatBookmaks Array - An array of bookmarks with only one level
	// @arg query String - The query to search the bookmarks
	find(flatBookmarks, query) {
		const options = { includeScore: true, keys: ['title'] }
		const fuse = new Fuse(flatBookmarks, options)
		return fuse.search(query)
	}
}
</script>