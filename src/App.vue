<template>
 <SearchComponent/>
</template>

<script>
import SearchComponent from './components/SearchComponent.vue'

export default {
  name: 'App',
  components: {
    SearchComponent
  }
}
</script>

<style>
body {
	background: linear-gradient(-45deg, #6dd8ff, #1567ff, #23a6d5, #16f5e2);
	background-size: 400% 400%;
	animation: gradient 15s ease infinite;
	min-height: 100vh;
}

@keyframes gradient {
	0% {
		background-position: 0% 50%;
	}
	50% {
		background-position: 100% 50%;
	}
	100% {
		background-position: 0% 50%;
	}
}

</style>
