<script lang="ts">
	import Buttonlink from "$lib/Buttonlink.svelte";
	import Card from "$lib/Card.svelte";
	import { browser } from "$app/environment";
	import { PUBLIC_API_HOST } from "$env/static/public";
	import type { PageData } from "./$types";
	import CardButton from "$lib/CardButton.svelte";
	export let data: PageData;
	function share() {
		navigator
			.share({
				url: "https://bothell.band",
				title: "Bothell High School Jazz Band"
			})
			.then(() => {
				console.log("Shared");
			});
	}
</script>

<svelte:head>
	<title>Bothell High School Jazz Band | Home</title>
	<meta
		name="description"
		content="Stay up-to-date on Bothell High School Jazz Band's news and events. Buy tickets, support fundraising efforts, and don't miss upcoming performances.
"
	/>
	{#if data.homePage}
		<meta
			name="og:description"
			content={data.homePage.data.attributes.MetaDescription}
		/>
	{:else}
		<meta
			name="og:description"
			content="Stay up-to-date on Bothell High School Jazz Band's news and events. Buy tickets, support fundraising efforts, and don't miss upcoming performances.
"
		/>
	{/if}
	<meta
		name="keywords"
		content="Bothell High School, Jazz Band, Jazz Music, School Band, Performance, Upcoming Events, Fundraising"
	/>
	<meta
		property="og:title"
		content="Bothell High School Jazz Band | Home"
	/>
	<meta
		property="og:url"
		content="https://bothell.band/"
	/>
	<meta
		property="og:site_name"
		content="Bothell High School Jazz Band"
	/>
</svelte:head>
<section class="mb-10 space-y-10">
	<h2 class="bebasneue text-center text-4xl text-white md:text-5xl">
		{#if data.homePage && data.events.data.length === 0}
			{data.homePage.data.attributes.NoEventsText}
		{:else if data.events && data.events.data.length !== 0}
			Welcome! Upcoming events:
		{:else}
			Welcome! There are no upcoming events!
		{/if}
	</h2>
	{#if data.events}
		<div class="grid grid-rows-1 justify-items-center space-y-10">
			<div class="mx-4 grid grid-cols-1 gap-x-10 gap-y-5 lg:grid-cols-2">
				{#each data.events.data as event}
					<Card
						let:Button
						alert={event.attributes.coverText}
						image={PUBLIC_API_HOST + event.attributes.thumbnail.data.attributes.url}
						imgAlt={event.attributes.thumbnail.data.attributes.alternativeText}
						name={event.attributes.title}
						date={event.attributes.date}
						location={event.attributes.location}
						locationURL={event.attributes.locationURL}
						highlighted={event.attributes.highlightedEvent}
					>
						{#each event.attributes.eventLinks as link}
							<Button
								link={link.url}
								disabled={link.disabled}
								buttonStyle={link.style}
							>
								{link.text}
							</Button>
						{/each}
					</Card>
				{/each}
			</div>
		</div>
	{/if}
</section>
<section>
	{#if data.homePage}
		<div class="space-y-5">
			<h2 class="bebasneue text-center text-4xl text-white md:text-5xl">
				{data.homePage.data.attributes.LinkHeader}
			</h2>
			<div class="flex w-full flex-col items-center">
				<div
					class="flex w-full max-w-2xl flex-col items-center items-stretch space-y-4 text-center"
				>
					{#if browser && navigator.share}
						<button
							on:click={share}
							class="bebasneue mx-3 bg-bothellblue p-3 align-middle text-3xl text-white shadow-2xl transition-all duration-300 ease-in-out hover:bg-white hover:text-bothellblue active:translate-y-1 md:text-4xl"
							>Share This Website!</button
						>
					{/if}
					{#each data.homePage.data.attributes.links as link}
						<Buttonlink url={link.url}>{link.text}</Buttonlink>
					{/each}
				</div>
			</div>
		</div>
	{/if}
</section>
