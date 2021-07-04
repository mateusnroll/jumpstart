<template>
	<div id="app">
		<NewTab />
	</div>
</template>

<script>
import NewTab from './components/NewTab.vue'

export default {
	name: 'App',
	components: {
		NewTab
	}
}
</script>

<style>
html {
  box-sizing: border-box;
}
*, *:before, *:after {
  box-sizing: inherit;
}

body {
	font-size: 16px;
	font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", 
		Roboto, Oxygen-Sans, Ubuntu, Cantarell,
		"Helvetica Neue", sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;

	background-color: #202124;
	color: #fff;
}

#app {
	width: 100%;
	max-width: 960px;
	margin: 0 auto;
}
</style>
