<script setup lang="ts">
	const props = defineProps({
		imgSrc: String,
		name: String,
		species: String,
		status: String,
		location: String,
		firstSeenIn: String
	})

	const backgroundColor = props.status === 'Alive' ? '#55cb44' : '#d53c2e';
</script>

<template>
	<div class="card">
		<img class="card__img" :src="imgSrc" :alt="name">
		<div class="card__content">
			<div class="card__content-section">
				<h2 class="card__name">{{ name }}</h2>
				<div class="card__status">
					<span class="card__status-icon" :style="{ backgroundColor: backgroundColor }"></span>
					<span>{{ status }} - {{ species }}</span>
				</div>
			</div>

			<div class="card__content-section">
				<span class="grayText">Last known location:</span>
				<span class="card__content-data">{{ location }}</span>
			</div>

			<div class="card__content-section">
				<span class="grayText">First seen in:</span>
				<span class="card__content-data">{{ firstSeenIn }}</span>
			</div>
		</div>
	</div>
</template>

<style scoped>
	.grayText {
		color: var(--color-gray);
	}

	.card {
		display: flex;
		border-radius: 20px;
		overflow: hidden;
		background-color: var(--color-background-soft);
	}

	.card__img {
		width: 50%;
		object-fit: cover;
	}

	.card__content {
		padding: 20px;
		display: flex;
		flex-direction: column;
		gap: 20px;
	}

	.card__status {
		display: flex;
		align-items: center;
		justify-content: start;
		gap: 10px;
	}

	.card__status-icon {
		display: inline-block;
		width: 10px;
		height: 10px;
		border-radius: 50%;
	}

	.card__content-section {
		display: flex;
		flex-direction: column;
		gap: 5px;
	}

	.card__content-data {
		font-size: 1.2rem;
		font-weight: 500;
	}

	.card__name {
		font-weight: 700;
		font-size: 32px;
	}
</style>