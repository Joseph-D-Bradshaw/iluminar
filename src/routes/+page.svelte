<script lang="ts">
	import Hero from '$lib/components/Hero.svelte';
	import About from '$lib/components/About.svelte';
	import Artists from '$lib/components/Artists.svelte';
	import { onMount } from 'svelte';
	import { gsap } from 'gsap';
	import { ScrollTrigger } from 'gsap/ScrollTrigger';
	import logoSvg from '$lib/assets/no-text-logo.svg';

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

		// 3. Scale-in boxes
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

		// 7. Background logo fade-in
		gsap.set('.bg-logo', { opacity: 0 });
		gsap.to('.bg-logo', {
			scrollTrigger: {
				trigger: '#about',
				start: 'top bottom',
				end: 'top 30%',
				scrub: true
			},
			opacity: 0.5,
			ease: 'none'
		});

		ScrollTrigger.refresh();
	});
</script>

<Hero />

<img class="bg-logo" src={logoSvg} alt="" />

<About />

<Artists />

<!-- Info -->
<section id="info" class="section boxes-section">
	<h2 class="section-title">INFO</h2>
	<div class="boxes">
		<div class="box"></div>
		<div class="box"></div>
		<div class="box"></div>
		<div class="box"></div>
		<div class="box"></div>
	</div>
</section>

<!-- Tickets -->
<section id="tickets" class="section parallax-section">
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

	/* ─ Background Logo ─ */
	.bg-logo {
		position: fixed;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		z-index: 0;
		pointer-events: none;
		width: min(600px, 80vw);
		height: auto;
	}
</style>
