<script>
	import { onMount } from 'svelte';
	import { fade, fly } from 'svelte/transition';
	import DailyGoals from "./DailyGoals.svelte";
	import FocusTimer from "./FocusTimer.svelte";
	import MoodCheck from "./MoodCheck.svelte";

	let isLoaded = false;
	let currentTime = new Date();

	onMount(() => {
		isLoaded = true;
		const timer = setInterval(() => {
			currentTime = new Date();
		}, 1000);

		return () => clearInterval(timer);
	});

	$: formattedTime = currentTime.toLocaleTimeString('en-US', { 
		hour: 'numeric', 
		minute: '2-digit',
		hour12: true 
	});
	
	$: formattedDate = currentTime.toLocaleDateString('en-US', { 
		weekday: 'long',
		month: 'long',
		day: 'numeric'
	});
</script>

{#if isLoaded}
	<main>
		<div class="dashboard">
			<header class="card-hover" in:fly={{ y: -20, duration: 1000, delay: 200 }}>
				<div class="header-content">
					<h1>Quickbytes</h1>
					<div class="header-info">
						<p class="time">{formattedTime}</p>
						<p class="date">{formattedDate}</p>
					</div>
				</div>
			</header>
			<div class="card-container">
				<div class="card card-hover" in:fly={{ x: -20, duration: 800, delay: 400 }}>
					<DailyGoals />
				</div>
				<div class="card card-hover" in:fly={{ y: 20, duration: 800, delay: 600 }}>
					<FocusTimer />
				</div>
				<div class="card card-hover" in:fly={{ x: 20, duration: 800, delay: 800 }}>
					<MoodCheck />
				</div>
			</div>
			<footer class="footer">
				<div class="footer-content card-hover">
					<p>Made by <a href="https://github.com/bracedd" target="_blank" rel="noopener">Divpreet</a></p>
					<a  href="https://github.com/bracedd/quickbytes" target="_blank" rel="noopener" class="source-link">Source Code</a>
				</div>
			</footer>
		</div>
	</main>
{/if}

<style>
	:global(body) {
		background: #f4f5f7 !important;
		margin: 0 !important;
		padding: 0 !important;
		overflow-x: hidden !important;
	}

	:global(.daily-goals),
	:global(.focus-timer),
	:global(.mood-check) {
		background: #fcfcfd !important;
		border: 1px solid #eaecef !important;
		box-shadow: 0 2px 12px rgba(0, 0, 0, 0.02) !important;
		width: 100% !important;
		box-sizing: border-box !important;
		transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1) !important;
	}

	:global(input),
	:global(select),
	:global(textarea) {
		background: #f8f9fa !important;
		border-color: #eaecef !important;
		width: 100% !important;
		box-sizing: border-box !important;
		transition: all 0.2s ease !important;
	}

	:global(input:focus),
	:global(select:focus),
	:global(textarea:focus) {
		border-color: #2d3748 !important;
		box-shadow: 0 0 0 3px rgba(45, 55, 72, 0.1) !important;
	}

	header {
		background: #fcfcfd;
		border: 1px solid #eaecef;
		border-radius: 1rem;
		padding: clamp(1.25rem, 4vw, 2rem);
		margin-bottom: clamp(1.25rem, 4vw, 2rem);
		box-shadow: 0 2px 12px rgba(0, 0, 0, 0.02);
		width: 100%;
		box-sizing: border-box;
	}

	.header-content {
		display: flex;
		justify-content: space-between;
		align-items: center;
		flex-wrap: wrap;
		gap: 1rem;
	}

	.header-info {
		text-align: right;
	}

	.time {
		font-size: clamp(1.25rem, 4vw, 1.5rem);
		font-weight: 600;
		color: #2d3748;
		margin: 0;
	}

	.date {
		font-size: clamp(0.875rem, 3vw, 1rem);
		color: #718096;
		margin: 0.25rem 0 0 0;
	}

	main {
		width: 100%;
		min-height: 100vh;
		background: #f4f5f7;
		overflow-x: hidden;
	}

	.dashboard {
		box-sizing: border-box;
		margin: 0 auto;
		padding: clamp(1.5rem, 4vw, 6rem) clamp(1rem, 4vw, 2rem);
		width: 100%;
		max-width: 72rem;
	}

	h1 {
		font-size: clamp(2rem, 8vw, 3.5rem);
		font-weight: 700;
		letter-spacing: -0.03em;
		color: #2d3748;
		margin: 0;
	}

	.card-container {
		display: grid;
		gap: clamp(1.25rem, 4vw, 2rem);
		width: 100%;
	}

	@media (min-width: 768px) {
		.card-container {
			grid-template-columns: repeat(2, 1fr);
		}
	}

	@media (min-width: 1024px) {
		.card-container {
			grid-template-columns: repeat(3, 1fr);
		}
	}

	@media (max-width: 767px) {
		.card-container {
			grid-template-columns: 1fr;
		}

		.header-content {
			flex-direction: column;
			align-items: flex-start;
		}

		.header-info {
			text-align: left;
			width: 100%;
		}

		:global(.daily-goals),
		:global(.focus-timer),
		:global(.mood-check) {
			padding: clamp(1.25rem, 4vw, 2rem) !important;
		}
	}

	/* Handle very small screens */
	@media (max-width: 360px) {
		.dashboard {
			padding: 1rem;
		}

		header {
			padding: 1rem;
		}

		:global(.daily-goals),
		:global(.focus-timer),
		:global(.mood-check) {
			padding: 1rem !important;
		}
	}

	/* Handle landscape orientation */
	@media (max-height: 480px) and (orientation: landscape) {
		.dashboard {
			padding: 1rem;
		}
		
		.card-container {
			grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
		}
	}

	.footer {
		width: 100%;
		margin-top: clamp(2rem, 4vw, 3rem);
	}

	.footer-content {
		background: #fcfcfd;
		border: 1px solid #eaecef;
		border-radius: 1rem;
		padding: clamp(1.25rem, 4vw, 2rem);
		display: flex;
		justify-content: space-between;
		align-items: center;
		box-shadow: 0 2px 12px rgba(0, 0, 0, 0.02);
	}

	.footer a {
		color: #718096;
		text-decoration: none;
		transition: all 0.2s ease;
	}

	.footer a:hover {
		color: #2d3748;
	}

	.source-link {
		padding: 0.5rem 0.75rem;
		border: 1px solid #eaecef;
		border-radius: 0.5rem;
		background: #fcfcfd;
		transition: all 0.2s ease;
	}

	.source-link:hover {


		color: #fcfcfd;
	}

	@media (max-width: 640px) {
		.footer-content {
			flex-direction: column;
			gap: 1rem;
			align-items: flex-start;
		}
	}

	.card-hover {
		transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
		will-change: transform;
	}

	.card-hover:hover {
		transform: translateY(-2px);
		box-shadow: 0 4px 24px rgba(0, 0, 0, 0.05);
	}

	/* Add smooth scrollbar for Firefox */
	* {
		scrollbar-width: thin;
		scrollbar-color: #718096 #f4f5f7;
	}

	/* Add smooth scrollbar for Chrome/Safari */
	::-webkit-scrollbar {
		width: 8px;
	}

	::-webkit-scrollbar-track {
		background: #f4f5f7;
	}

	::-webkit-scrollbar-thumb {
		background: #718096;
		border-radius: 4px;
	}

	::-webkit-scrollbar-thumb:hover {
		background: #2d3748;
	}

	/* Selection color */
	::selection {
		background: rgba(45, 55, 72, 0.1);
		color: #2d3748;
	}
</style>