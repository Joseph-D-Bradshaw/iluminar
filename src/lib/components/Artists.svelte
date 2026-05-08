<script lang="ts">
	import { gsap } from 'gsap';
	import { ScrollTrigger } from 'gsap/ScrollTrigger';
	import type { Attachment } from 'svelte/attachments';

	const images = Array.from({ length: 8 }, (_, i) => `/artist-${i + 1}.webp`);

	let anim: gsap.core.Tween | undefined;
	let hovering = false;

	const trackAnimation: Attachment = (node) => {
		gsap.registerPlugin(ScrollTrigger);

		const track = node;
		const wrapper = track.parentElement!;
		const totalWidth = track.scrollWidth / 2;

		anim = gsap.to(track, {
			x: -totalWidth,
			duration: 40,
			ease: 'none',
			repeat: -1
		});

		ScrollTrigger.create({
			trigger: '#artists',
			start: 'top bottom',
			end: 'bottom top',
			onEnter: () => {
				if (!hovering) anim?.play();
			},
			onLeave: () => anim?.pause(),
			onEnterBack: () => {
				if (!hovering) anim?.play();
			},
			onLeaveBack: () => anim?.pause()
		});

		wrapper.addEventListener('mouseenter', () => {
			hovering = true;
			anim?.pause();
		});
		wrapper.addEventListener('mouseleave', () => {
			hovering = false;
			anim?.resume();
		});

		return () => {
			anim?.kill();
			anim = undefined;
		};
	};
</script>

<section id="artists" class="section artists-section">
	<div class="marquee-wrapper">
		<div class="marquee-track" {@attach trackAnimation}>
			{#each images as src, i (src + '-a')}
				<div class="marquee-item">
					<img {src} alt="" loading="lazy" />
				</div>
			{/each}
			{#each images as src, i (src + '-b')}
				<div class="marquee-item">
					<img {src} alt="" loading="lazy" />
				</div>
			{/each}
		</div>
	</div>
	<h2 class="section-title">ARTISTS</h2>
</section>

<style>
	.artists-section {
		min-height: 100vh;
		display: flex;
		align-items: center;
		padding: 0;
		position: relative;
		overflow: hidden;
	}

	.section-title {
		position: absolute;
		bottom: 3rem;
		left: 2rem;
		z-index: 1;
	}

	.marquee-wrapper {
		width: 100%;
		overflow: hidden;
		cursor: pointer;
		padding: 2rem 0;
	}

	.marquee-track {
		display: flex;
		gap: 1.5rem;
		width: max-content;
		padding: 0 0.75rem;
		align-items: center;
	}

	.marquee-item {
		flex-shrink: 0;
		width: min(40vh, 400px);
		aspect-ratio: 1;
		border-radius: var(--radius-md);
		overflow: hidden;
		border: 1px solid var(--color-border);
		transition: border-color var(--transition-fast);
	}

	.marquee-item:hover {
		border-color: var(--color-gold);
	}

	.marquee-item img {
		width: 100%;
		height: 100%;
		object-fit: cover;
		display: block;
	}

	@media (max-width: 640px) {
		.marquee-item {
			width: 30vh;
		}

		.section-title {
			bottom: 2rem;
			left: 1rem;
		}
	}
</style>
