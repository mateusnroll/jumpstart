<template>
	<div 
		class="search-container"
		@click="focusInput">

		<div 
			class="search-box"
			:class="{ focused: isFocused }">
			<img
				class="icon-search"
				src="assets/icons/icon-search.svg"/>

			<input 
				class="search-input"
				ref="searchInput"
				tabindex="1"
				type="text"
				v-model="searchQuery"
				@focus="isFocused = true"
				@blur="isFocused = false">
		</div>

	</div>
</template>

<script>
export default {	
	data: () => {
		return {
			isFocused: false,
			searchQuery: ''
		}
	},

	watch: {
		searchQuery: function(newVal) {
			this.$emit('query-update', newVal)
		}
	},

	methods: {
		focusInput() {
			this.$refs.searchInput.focus()
		}
	}
}
</script>

<style scoped>
.search-container {
	width: 100%;
	margin-top: 3.5rem;
	margin-bottom: 5rem;
	display: flex;
	justify-content: center;
}

.search-box {
	--angle: 0deg;

	display: flex;
	width: 100%;
	max-width: 30rem;
	padding: 1rem;

	cursor: text;
	background-color: #262729;
	border-radius: 8px;
	border: 1px solid #262729;

	animation: 10s rotate linear infinite;
}
.search-box.focused {
	border-color: white;
}

.icon-search {
	flex-shrink: 0;
	margin-right: .5rem;

	/* icon-color: 737680 */
	filter: invert(51%) sepia(6%) saturate(588%) hue-rotate(189deg) brightness(88%) contrast(86%); 
}

.search-input {
	flex-grow: 1;

	background-color: transparent;
	color: #fff;
	border: none;
	outline: none;
}
</style>