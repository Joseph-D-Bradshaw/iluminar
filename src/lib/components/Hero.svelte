<script lang="ts">
	import { base } from '$app/paths';
</script>

<section id="hero" class="hero">
	<div class="hero-video">
		<video autoplay muted loop playsinline>
			<source src="{base}/hero-bg.mp4" type="video/mp4" />
		</video>
	</div>
	<div class="hero-overlay"></div>
	<div class="hero-fade"></div>
	<h1 class="hero-title">iluminar</h1>
	<p class="hero-subtitle">A spark in the dark.</p>
</section>

<style>
	.hero {
		height: 100vh;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		text-align: center;
		position: relative;
		overflow: hidden;
	}

	.hero-video {
		position: absolute;
		inset: 0;
		z-index: -2;
	}

	.hero-video video {
		width: 100%;
		height: 100%;
		object-fit: cover;
	}

	.hero-overlay {
		position: absolute;
		inset: 0;
		z-index: -1;
		background: rgba(0, 0, 0, 0.65);
	}

	.hero-fade {
		position: absolute;
		inset: 0;
		z-index: -1;
		background: linear-gradient(to bottom, transparent 60%, var(--color-bg) 100%);
		pointer-events: none;
	}

	.hero-title {
		font-size: clamp(3rem, 10vw, 7rem);
		letter-spacing: 0.08em;
		text-transform: uppercase;
	}

	.hero-subtitle {
		font-size: var(--text-lg);
		color: var(--color-text-muted);
		margin-top: var(--space-4);
	}
</style>
