<script setup lang="ts">
import Card from "./CardItem.vue";
import Pagination from "./PaginationComponent.vue";
import { ref, reactive, onMounted } from 'vue';

const items:Character[] = reactive([]);
const baseUrl = 'https://rickandmortyapi.com/api/character'
let next = ref(0);
let prev = ref(0);
let allPages = ref(0);
const filter = reactive({
	name: null,
	status: null
});

onMounted(() => getPage(baseUrl))

interface Episodes {
	[key: number]: string
}

type Episode = Episodes[] | string

interface Character {
	episode: Episode,
	image: string,
	location: {
		name: string,
		url: string
	},
	species: string,
	status: string,
	name: string
}


function getPage(baseUrl:string, page?:number) {
	items.splice(0, items.length);
	const url = new URL(baseUrl);

	if (page) url.searchParams.set('page', String(page));
	if (filter.name) url.searchParams.set('name', filter.name);
	if (filter.status) url.searchParams.set('status', filter.status);

	fetch(url)
		.then(response => response.json())
		.then(data => {
			console.log(data)
			const nextUrl = data.info.next ? new URL(data.info.next) : null;
			next.value = Number(nextUrl?.searchParams.get('page'));
			const prevUrl = data.info.prev ? new URL(data.info.prev) : null;
			prev.value = Number(prevUrl?.searchParams.get('page'));
			allPages.value = data.info.pages;

			data.results.forEach(async(element: Character) => {
				const episodeUrl: string = String(element.episode[element.episode.length - 1]);
				element.episode = await getEpisode(episodeUrl);
				items.push(element);
			});
		});
}

async function getEpisode(url: string) {
	let response = await fetch(url);
	let data = await response.json();
	return data.name;
}

</script>


<template>
  <div class="wrapper">
		<div class="filters">
			<select v-model="filter.status" name="status">
				<option value="">Не выбрано</option>
				<option value="alive">Alive</option>
				<option value="dead">Dead</option>
				<option value="unknown">Unknown</option>
			</select>
			<input v-model="filter.name" name="name" type="text" placeholder="Имя">
			<button class="btn" @click="getPage(baseUrl)">Применить</button>
		</div>
		<div class="cards">
			<template v-for="item in items" :key="item.id">
				<Card 
					:img-src='item.image' 
					:name='item.name' 
					:species='item.species' 
					:status='item.status' 
					:location='item.location.name' 
					:first-seen-in='String(item.episode)'
				/>
			</template>
		</div>

		<Pagination
			:prev="prev"
			:next="next"
			:allPages="allPages"
			@getPage="(page) => getPage(baseUrl, page)"
		/>
  </div>
</template>


<style>
	.cards {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(400px, 1fr));
		grid-gap: 30px;
	}

	.btn,
	input,
	select {
		padding: 5px 20px;
		min-width: 60px;
		min-height: 60px;
		border-radius: 10px;
		background-color: var(--color-background-soft);
		border: 2px solid var(--color-white);
		color: inherit;
		font-size: 28px;
		font-weight: 500;
	}

	.btn {
		background-size: 70% 70%;
		background-repeat: no-repeat;
		background-position: center;
	}

	.btn--prev {
		background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='14' height='16' fill='none'%3E%3Cpath fill-rule='evenodd' clip-rule='evenodd' d='M6.657 15.071.293 8.707a1 1 0 0 1 0-1.414L6.657.929A1 1 0 1 1 8.07 2.343L3.414 7H13a1 1 0 1 1 0 2H3.414l4.657 4.657a1 1 0 0 1-1.414 1.414z' fill='%23fff'/%3E%3C/svg%3E");
	}

	.btn--next {
		background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='14' height='16' fill='none'%3E%3Cpath fill-rule='evenodd' clip-rule='evenodd' d='m7.343.929 6.364 6.364a1 1 0 0 1 0 1.414l-6.364 6.364a1 1 0 0 1-1.414-1.414L10.586 9H1a1 1 0 0 1 0-2h9.586L5.929 2.343A1 1 0 0 1 7.343.93z' fill='%23fff'/%3E%3C/svg%3E");
	}

	.filters {
		display: flex;
		flex-wrap: wrap;
		gap: 20px;
		margin-bottom: 30px;
	}
</style>