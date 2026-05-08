<script lang="ts">
	import { base } from '$app/paths';
</script>

<section id="people" class="section people-section">
	<div class="people-layout">
		<img class="people-image" src="{base}/people-background.webp" alt="" />
		<div class="people-text">
			<h2 class="people-heading">The People<br />and the Place</h2>
		</div>
	</div>
</section>

<style>
	.people-section {
		min-height: 100vh;
		display: flex;
		align-items: center;
		justify-content: center;
		padding: 0;
		position: relative;
		overflow: hidden;
	}

	.people-layout {
		display: flex;
		width: 100%;
		max-width: 1400px;
		align-items: flex-end;
		gap: 2rem;
		padding: 4rem 2rem;
	}

	.people-image {
		width: 65%;
		height: min(55vh, 500px);
		object-fit: cover;
		border-radius: var(--radius-md);
		flex-shrink: 0;
		display: block;
	}

	.people-text {
		flex: 1;
		display: flex;
		align-items: flex-end;
	}

	.people-heading {
		font-family: var(--font-heading);
		font-size: clamp(2.5rem, 6vw, 6rem);
		color: var(--color-gold-light);
		line-height: var(--leading-snug);
		margin: 0;
		word-wrap: break-word;
		overflow-wrap: break-word;
	}

	@media (max-width: 768px) {
		.people-layout {
			flex-direction: column;
			align-items: flex-start;
			justify-content: flex-end;
			gap: 1rem;
			padding: 1.5rem 1.25rem;
		}

		.people-image {
			width: 100%;
			height: min(25vh, 220px);
		}

		.people-text {
			width: 100%;
		}

		.people-heading {
			font-size: var(--text-2xl);
		}
	}

	@media (max-width: 480px) {
		.people-layout {
			padding: 1rem;
		}

		.people-image {
			height: min(20vh, 160px);
		}

		.people-heading {
			font-size: var(--text-xl);
		}
	}
</style>
