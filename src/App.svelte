<script>
	import { onMount } from 'svelte';
	import moment from 'moment';

	let data;
	let articles;

	async function fetchHeadlines() {
		data = await fetch(`https://newsapi.org/v2/top-headlines?country=us&pageSize=50&apiKey=232713e853bc4208942d8fb7b7017174`)
		.then(res => res.json());

		articles = data.articles;

		window.scrollTo(0,0);
	}

	function formatDate(date) {
		return moment(date).format('MMMM Do YYYY, h:mm a');
	}

	onMount(fetchHeadlines);
</script>

<style>
</style>

<svelte:head>
	<title>Top headlines in US</title>
</svelte:head>

<div class="font-sans bg-gray-100">
	<div class="py-8">
		<div class="container max-w-4xl my-0 mx-auto px-4 py-8 lg:my-12 md:px-8 md:my-2">
			<div class="text-indigo-600 text-4xl font-bold">Top headlines <span class="text-gray-600 text-2xl"> in US</span></div>
			<div class="flex flex-wrap -mx-1 lg:-mx-4">
				{#if articles}
				{#each articles as article}
					<transition name="slide-fade">
						<div class="my-1 px-1 w-full md:w-full my-4 lg:px-4">
						<a href={article.url} target="_blank">
							<article class="bg-white overflow-hidden rounded-lg shadow-md p-8 cursor-pointer transition-fast hover:shadow-lg">
								<div class="md:flex">
									{#if article.urlToImage}
									<div class="md:flex-shrink-0">
										<img class="rounded-lg md:w-56" alt={article.title} src={article.urlToImage}>
									</div>
									{/if}
									<div class="mt-4 md:mt-0 md:ml-6">
										<div class="uppercase tracking-wide text-sm text-indigo-600 font-bold">{formatDate(article.publishedAt)} &middot; { article.source.name }</div>
										<a href={article.url} target="_blank" class="block mt-1 text-lg leading-tight font-semibold text-gray-900 hover:underline">{article.title}</a>
										<p class="mt-2 text-gray-600">{article.description}</p>
									</div>
								</div>
							</article>
							</a>
						</div>            
					</transition>
				{/each}
				{:else}
					<div class="p-4">Loading...</div>
				{/if}
			</div>
			<div class="container text-center my-10">
				<span class="inline-flex items-center text-base text-gray-500">Powered by
				<a href="https://newsapi.org" target="_blank" class="group transition-fast ml-1 font-bold font-serif text-base hover:text-gray-800">News API</a>
				</span>
			</div>
		</div>
	</div>
</div>
