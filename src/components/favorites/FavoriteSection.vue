<template>
	<div class="favorite-section">
		<h2 class="section-title">{{ title }}</h2>

		<div class="favorite-list">
			<div 
				class="favorite-column"
				v-for="column in childrenColumns"
				:key="column.id">

				<favorite-item
					v-for="bookmark in column.items" 
					:key="bookmark.id"
					:title="bookmark.title"
					:url="bookmark.url"/>
			</div>
		</div>
	</div>
</template>


<script>
import FavoriteItem from './FavoriteItem.vue'

export default {
	components: { FavoriteItem },

	props: {
		title: String,
		children: Array,
		cols: Number
	},

	computed: {
		childrenColumns: function() {
			console.log(this.splitArray(this.children, this.cols))
			return this.splitArray(this.children, this.cols)
		}
	},

	methods: {
		splitArray(array, parts) {
			let clone = [...array]
			let out = []

			for (let i = parts; i > 0; i--)
				out.push({
					id: Math.random().toString(),
					items: clone.splice(0, Math.ceil(clone.length / i))
				});

			return out;
		}
	}
}
</script>


<style scoped>
.favorite-section {
	margin-top: 5rem;
}

.section-title {
	font-size: 2rem;
	margin: 0 0 .5rem .5rem;
}

.favorite-list {
	display: flex;	
}

.favorite-column {
	width: 50%;
}
</style>