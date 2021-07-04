<template>
	<a 
		class="fav-item"
		:href="url">

		<img
			class="favicon" 
			:src="'chrome://favicon/' + url"
			height=16 width=16>
		<span 
			class="title">
			{{ title }}
			</span>
	</a>
</template>

<script>
export default {
	props: {
		title: String,
		url: String
	}
}
</script>

<style scoped>
.fav-item {
	display: flex;
	align-items: center;
	color: #889097;
	text-decoration: none;
	padding: .5rem;

	transition: all .15s ease-in-out;
}
.fav-item:hover {
	background-color: rgba(255,255,2555,.05);
	border-radius: .5rem;
	color: #fff;
	text-decoration: underline;
}

.title {
	display: block;
	margin-left: .5rem;

	overflow: hidden;
	white-space: nowrap;
	text-overflow: ellipsis;
	
	transition: all .01s ease-in-out;
}
</style>