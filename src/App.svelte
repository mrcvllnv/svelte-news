<script>
	import { onMount } from 'svelte';
	import moment from 'moment';

	let data;
	let articles;

	async function fetchHeadlines() {
		data = await fetch(`https://newsapi.org/v2/top-headlines?country=ph&pageSize=50&apiKey=3d481d81a7414a969590652ec8fc6f17`)
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
			<div class="text-indigo-600 text-4xl font-bold">Top headlines <span class="text-gray-600 text-2xl"> in PH</span></div>
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
										<div class="pt-1 mt-auto">
											<a class="inline-flex items-center text-indigo-600 hover:text-indigo-800" href={article.url} target="_blank">
												<span class="text-sm md:text-base font-semibold">Read more</span>
												<svg class="fill-current w-4 h-4 ml-2" viewBox="0 0 18 12" xmlns="http://www.w3.org/2000/svg">
												<path fill-rule="evenodd" clip-rule="evenodd" d="M14.5858 7H1C0.447715 7 0 6.55228 0 6C0 5.44772 0.447715 5 1 5H14.5858L11.2929 1.70711C10.9024 1.31658 10.9024 0.683418 11.2929 0.292893C11.6834 -0.0976311 12.3166 -0.0976311 12.7071 0.292893L17.7071 5.29289C18.0976 5.68342 18.0976 6.31658 17.7071 6.70711L12.7071 11.7071C12.3166 12.0976 11.6834 12.0976 11.2929 11.7071C10.9024 11.3166 10.9024 10.6834 11.2929 10.2929L14.5858 7Z"></path>
												</svg>
											</a>
										</div>
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
