<script setup lang="ts">
	import { computed } from 'vue';

	const showPages = 5;
	const props = defineProps({
		prev: Number,
		next: Number,
		allPages: Number
	})
	defineEmits(['getPage'])

	const currentPage = computed(() => {
		return props.prev ? props.prev + 1 : 1;
	});

	const renderedPages = computed(() => {
		const arr = []
		if (props.allPages) {
			if (props.allPages <= showPages) {
				for (let i = 1; i <= props.allPages; i++) {
					const page = {
						number: i,
						isActive: i === currentPage.value
					}
					arr.push(page)
				}
			} else {
				const firstPage = currentPage.value - Math.floor(showPages / 2) <= 1 ? 1 : currentPage.value - Math.floor(showPages / 2) + showPages >= props.allPages ? props.allPages - showPages + 1 : currentPage.value - Math.floor(showPages / 2);
				const lastPage = firstPage + showPages;
				for (let i = firstPage; i < lastPage; i++) {
					const page = {
						number: i,
						isActive: i === currentPage.value
					}
					arr.push(page)
				}
			}
		}
		return arr
	})
</script>


<template>
	<div class="pagination">
			<button v-if="prev" class="btn btn--prev" @click="$emit('getPage', prev)"></button>
			<div class="pagination__pages">
				<template v-if="allPages && renderedPages && renderedPages[0].number > 1">
					<span class="pagination__page" @click="$emit('getPage', 1)">1</span>
					<span>...</span>
				</template>
				<template v-if="renderedPages.length > 1">
					<template v-for="(page, idx) in renderedPages" :key="idx">
						<span 
							class="pagination__page" 
							:class="[page.isActive ? 'active' : '']"
							@click="$emit('getPage', page.number)"
						>{{page.number}}</span>
					</template>
				</template>
				<template v-if="allPages && renderedPages && renderedPages[renderedPages.length - 1].number + 2 <= allPages">
					<span>...</span>
					<span class="pagination__page" @click="$emit('getPage', allPages)">{{allPages}}</span>
				</template>
			</div>
			<button v-if="next" class="btn btn--next" @click="$emit('getPage', next)"></button>
		</div>
</template>

<style scoped>

	.pagination {
		margin-top: 40px;
		display: flex;
		align-items: center;
		justify-content: center;
		gap: 30px;
	}

	.pagination__pages {
		display: flex;
		gap: 5px;
		font-size: 28px;
	}

	.pagination__page {
		cursor: pointer;
	}

	.pagination__page.active {
		cursor: default;
		pointer-events: none;
	}

	.active {
		font-weight: 700;
	}
</style>