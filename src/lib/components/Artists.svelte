<script lang="ts">
	import { gsap, ScrollTrigger } from '$lib/utils/gsap';
	import type { Attachment } from 'svelte/attachments';

	const images = Array.from({ length: 8 }, (_, i) => `/artist-${i + 1}.webp`);

	const trackAnimation: Attachment = (node) => {
		const track = node;
		const wrapper = track.parentElement!;

		function getDistance() {
			return track.scrollWidth - wrapper.clientWidth;
		}

		const ctx = gsap.context(() => {
			gsap.to(track, {
				x: () => -getDistance(),
				ease: 'none',
				scrollTrigger: {
					trigger: '#artists',
					start: 'top top',
					end: () => `+=${getDistance()}`,
					pin: true,
					pinSpacing: true,
					scrub: 1,
					invalidateOnRefresh: true,
					anticipatePin: 1
				}
			});
		});

		ScrollTrigger.refresh();

		return () => ctx.revert();
	};
</script>

<section id="artists" class="section artists-section">
	<div class="marquee-wrapper">
		<div class="marquee-track" {@attach trackAnimation}>
			{#each images as src, i (src + '-a')}
				<div class="marquee-item" class:offset-up={i % 2 === 0} class:offset-down={i % 2 !== 0}>
					<div class="marquee-inner">
						<img {src} alt="" loading="lazy" />
					</div>
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
		overflow: visible;
		border: 1px solid var(--color-border);
		transition: border-color var(--transition-fast);
	}

	.marquee-item:hover {
		border-color: var(--color-gold);
	}

	.marquee-item.offset-up {
		margin-top: -40px;
	}

	.marquee-item.offset-down {
		margin-top: 40px;
	}

	.marquee-inner {
		width: 100%;
		height: 100%;
		border-radius: var(--radius-md);
		overflow: hidden;
	}

	.marquee-inner img {
		width: 100%;
		height: 100%;
		object-fit: cover;
		display: block;
	}

	@media (max-width: 640px) {
		.marquee-item {
			width: 30vh;
		}

		.marquee-item.offset-up {
			margin-top: -20px;
		}

		.marquee-item.offset-down {
			margin-top: 20px;
		}

		.section-title {
			bottom: 2rem;
			left: 1rem;
		}
	}
</style>
