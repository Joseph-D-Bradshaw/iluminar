<script lang="ts">
	import { onMount } from 'svelte';
	import { gsap } from 'gsap';
	import { ScrollTrigger } from 'gsap/ScrollTrigger';

	onMount(() => {
		gsap.registerPlugin(ScrollTrigger);

		// 1. Hero parallax
		gsap.to('.hero-title', {
			scrollTrigger: {
				trigger: '.hero',
				start: 'top top',
				end: 'bottom top',
				scrub: true
			},
			y: 120,
			scale: 0.8,
			opacity: 0.3
		});

		// 2. Staggered card reveal
		gsap.from('.card', {
			scrollTrigger: {
				trigger: '.cards-grid',
				start: 'top 75%',
				toggleActions: 'play none none reverse'
			},
			opacity: 0,
			y: 80,
			rotateY: 30,
			duration: 0.8,
			stagger: 0.15,
			ease: 'power2.out'
		});

		// 3. Progress bar scrub
		gsap.to('.progress-fill', {
			scrollTrigger: {
				trigger: '.progress-section',
				start: 'top 70%',
				end: 'bottom 30%',
				scrub: true
			},
			scaleX: 1,
			ease: 'none'
		});

		// 4. Scale-in boxes
		gsap.from('.box', {
			scrollTrigger: {
				trigger: '.boxes',
				start: 'top 80%',
				toggleActions: 'play none none reverse'
			},
			scale: 0,
			rotation: 45,
			opacity: 0,
			duration: 0.6,
			stagger: 0.1,
			ease: 'back.out(2)'
		});

		// 5. Parallax background layer
		gsap.to('.parallax-layer', {
			scrollTrigger: {
				trigger: '.parallax-section',
				start: 'top bottom',
				end: 'bottom top',
				scrub: true
			},
			y: -100
		});

		// 6. Text reveal (clip animation)
		gsap.from('.reveal-line', {
			scrollTrigger: {
				trigger: '.reveal-section',
				start: 'top 75%',
				toggleActions: 'play none none reverse'
			},
			y: 40,
			opacity: 0,
			duration: 0.7,
			stagger: 0.2
		});
	});
</script>

<!-- Hero -->
<section class="hero">
	<h1 class="hero-title">iluminar</h1>
	<p class="hero-subtitle">A spark in the dark.</p>
</section>

<!-- Staggered cards -->
<section class="section cards-section">
	<h2 class="section-title">Fade &amp; Stagger</h2>
	<div class="cards-grid">
		<div class="card">One</div>
		<div class="card">Two</div>
		<div class="card">Three</div>
		<div class="card">Four</div>
	</div>
</section>

<!-- Progress bar -->
<section class="section progress-section">
	<h2 class="section-title">Scrub Progress</h2>
	<div class="progress-track">
		<div class="progress-fill"></div>
	</div>
	<p class="progress-label">Scroll to fill</p>
</section>

<!-- Scale-in boxes -->
<section class="section boxes-section">
	<h2 class="section-title">Scale &amp; Rotate</h2>
	<div class="boxes">
		<div class="box"></div>
		<div class="box"></div>
		<div class="box"></div>
		<div class="box"></div>
		<div class="box"></div>
	</div>
</section>

<!-- Parallax -->
<section class="section parallax-section">
	<div class="parallax-layer"></div>
	<div class="parallax-content">
		<h2 class="section-title">Parallax</h2>
		<p>The background moves at a different speed.</p>
	</div>
</section>

<!-- Text reveal -->
<section class="section reveal-section">
	<h2 class="section-title">Text Reveal</h2>
	<p class="reveal-line">Each line fades in</p>
	<p class="reveal-line">as you scroll down</p>
	<p class="reveal-line">one after another.</p>
</section>

<style>
	/* ─ Layout ─ */
	.section {
		min-height: 100vh;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		padding: 4rem 2rem;
		text-align: center;
		position: relative;
		overflow: hidden;
	}

	.section-title {
		margin-bottom: 2rem;
	}

	/* ─ Hero ─ */
	.hero {
		height: 100vh;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		text-align: center;
		position: relative;
		z-index: 1;
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

	/* ─ Cards ─ */
	.cards-grid {
		display: grid;
		grid-template-columns: repeat(4, 1fr);
		gap: 1.5rem;
		max-width: 720px;
		width: 100%;
		perspective: 1000px;
	}

	.card {
		background: var(--color-bg-elevated);
		border: 1px solid var(--color-border);
		border-radius: var(--radius-md);
		padding: 3rem 1rem;
		font-family: var(--font-heading);
		font-size: var(--text-xl);
		color: var(--color-gold);
	}

	/* ─ Progress ─ */
	.progress-track {
		width: min(400px, 80vw);
		height: 8px;
		background: var(--color-bg-subtle);
		border-radius: var(--radius-full);
		overflow: hidden;
	}

	.progress-fill {
		width: 100%;
		height: 100%;
		background: var(--color-gold);
		border-radius: var(--radius-full);
		transform-origin: left;
		transform: scaleX(0);
	}

	.progress-label {
		margin-top: var(--space-4);
		color: var(--color-text-muted);
		font-size: var(--text-sm);
	}

	/* ─ Boxes ─ */
	.boxes {
		display: flex;
		gap: 1rem;
		flex-wrap: wrap;
		justify-content: center;
	}

	.box {
		width: 80px;
		height: 80px;
		background: var(--color-gold);
		border-radius: var(--radius-sm);
	}

	/* ─ Parallax ─ */
	.parallax-section {
		min-height: 80vh;
	}

	.parallax-layer {
		position: absolute;
		inset: 0;
		background: radial-gradient(ellipse at 50% 50%, var(--color-gold-dark) 0%, transparent 70%);
		opacity: 0.15;
		pointer-events: none;
	}

	.parallax-content {
		position: relative;
		z-index: 1;
	}

	/* ─ Text Reveal ─ */
	.reveal-line {
		font-size: var(--text-2xl);
		margin: 0.3em 0;
	}
</style>
