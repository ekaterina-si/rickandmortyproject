<script setup lang="ts">
import CardContainer from './components/CardContainer.vue'
</script>

<template>
  <header>
		<div class="wrapper">
			<img class="logo" src="@/assets/Rick_and_Morty_logo.svg" alt="Rick and Morty">
		</div>
  </header>

	<CardContainer />
</template>

<style scoped>

	header {
		line-height: 1.5;
		max-height: 50vh;
	}

	.logo {
		display: block;
		height: 100%;
		width: auto;
		max-width: 100%;
		object-fit: contain;
		margin: 0 auto;
	}
</style>
