<script lang="ts">
	import { gsap, ScrollTrigger } from '$lib/utils/gsap';
	import type { Attachment } from 'svelte/attachments';
	import { base } from '$app/paths';

	const img = (path: string) => `${base}${path}`;

	const artists = [
		{ src: img('/artist-1.webp'), name: 'Artist One' },
		{ src: img('/artist-2.webp'), name: 'Bob Smith' },
		{ src: img('/artist-3.webp'), name: 'Joseph B' },
		{ src: img('/artist-4.webp'), name: 'Little Przemememe' },
		{ src: img('/artist-5.webp'), name: 'Annabobana' },
		{ src: img('/artist-6.webp'), name: 'Poco Schmoko' },
		{ src: img('/artist-7.webp'), name: 'Big Pizza Slice' },
		{ src: img('/artist-8.webp'), name: 'Small Tootin' }
	];

	const orbs = [
		{ size: 12, x: 8, y: 18 },
		{ size: 7, x: 28, y: 65 },
		{ size: 18, x: 52, y: 12 },
		{ size: 9, x: 72, y: 50 },
		{ size: 14, x: 88, y: 78 },
		{ size: 6, x: 18, y: 82 },
		{ size: 11, x: 45, y: 38 },
		{ size: 8, x: 65, y: 22 },
		{ size: 10, x: 82, y: 40 },
		{ size: 5, x: 35, y: 90 }
	];

	const sectionAnimation: Attachment = (node) => {
		const section = node as HTMLElement;
		const track = section.querySelector<HTMLElement>('.marquee-track')!;
		const wrapper = track.parentElement!;

		function getDistance() {
			return track.scrollWidth - wrapper.clientWidth;
		}

		const ctx = gsap.context(() => {
			const scrollTween = gsap.to(track, {
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

			gsap.to('.orb', {
				x: (i) => [180, -120, 260, -180, 150, -220, 190, -150, 130, -170][i] ?? 0,
				y: (i) => [20, -30, -15, 40, -25, 15, -40, 30, -10, 35][i] ?? 0,
				scrollTrigger: {
					trigger: '#artists',
					start: 'top top',
					end: () => `+=${getDistance()}`,
					scrub: 1
				},
				ease: 'none'
			});

			gsap.to('.progress-fill', {
				scaleX: 1,
				transformOrigin: 'left center',
				ease: 'none',
				scrollTrigger: {
					trigger: '#artists',
					start: 'top top',
					end: () => `+=${getDistance()}`,
					scrub: true
				}
			});

			track.querySelectorAll('.marquee-item').forEach((item) => {
				const nameEl = item.querySelector('.artist-name');
				if (!nameEl) return;

				gsap.fromTo(
					nameEl,
					{ opacity: 0, y: 16, filter: 'blur(4px)' },
					{
						opacity: 1,
						y: 0,
						filter: 'blur(0px)',
						duration: 0.5,
						ease: 'power2.out',
						scrollTrigger: {
							containerAnimation: scrollTween,
							trigger: item,
							start: 'left 62%',
							toggleActions: 'play none none reset'
						}
					}
				);
			});
		});

		ScrollTrigger.refresh();

		return () => ctx.revert();
	};
</script>

<section id="artists" class="section artists-section" {@attach sectionAnimation}>
	<div class="orbs-container">
		{#each orbs as orb, i (i)}
			<div
				class="orb"
				style="width: {orb.size}px; height: {orb.size}px; left: {orb.x}%; top: {orb.y}%;"
			></div>
		{/each}
	</div>

	<div class="marquee-wrapper">
		<div class="marquee-track">
			{#each artists as artist, i (artist.src)}
				<div class="marquee-item" class:offset-up={i % 2 === 0} class:offset-down={i % 2 !== 0}>
					<div class="marquee-inner">
						<img src={artist.src} alt={artist.name} loading="lazy" />
						<div class="artist-name">{artist.name}</div>
					</div>
				</div>
			{/each}
		</div>
	</div>

	<h2 class="section-title">ARTISTS</h2>

	<div class="progress-bar" aria-hidden="true">
		<div class="progress-fill"></div>
	</div>
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

	/* ── Floating Orbs ── */
	.orbs-container {
		position: absolute;
		inset: 0;
		pointer-events: none;
		z-index: 0;
	}

	.orb {
		position: absolute;
		border-radius: 50%;
		background: radial-gradient(
			circle at 30% 30%,
			rgba(255, 215, 100, 0.6),
			rgba(255, 180, 50, 0.15)
		);
		box-shadow:
			0 0 12px rgba(255, 200, 80, 0.3),
			0 0 40px rgba(255, 180, 50, 0.1);
		will-change: transform;
	}

	/* ── Marquee ── */
	.marquee-wrapper {
		width: 100%;
		overflow: hidden;
		padding: 2rem 0;
		position: relative;
		z-index: 1;
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
		transition:
			border-color var(--transition-fast),
			transform 0.4s cubic-bezier(0.25, 1, 0.5, 1),
			box-shadow 0.4s cubic-bezier(0.25, 1, 0.5, 1);
	}

	.marquee-item:hover {
		border-color: var(--color-gold);
		transform: scale(1.04);
		box-shadow:
			0 0 24px rgba(255, 200, 80, 0.25),
			0 0 60px rgba(255, 180, 50, 0.08);
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
		position: relative;
	}

	.marquee-inner img {
		width: 100%;
		height: 100%;
		object-fit: cover;
		display: block;
		transition: transform 0.5s cubic-bezier(0.25, 1, 0.5, 1);
	}

	.marquee-item:hover .marquee-inner img {
		transform: scale(1.08);
	}

	/* ── Artist Names ── */
	.artist-name {
		position: absolute;
		bottom: 0;
		left: 0;
		right: 0;
		padding: 1.5rem 1rem 1rem;
		background: linear-gradient(transparent, rgba(0, 0, 0, 0.7));
		color: var(--color-gold-light);
		font-family: var(--font-heading);
		font-size: var(--text-lg);
		text-align: center;
		letter-spacing: 0.04em;
		pointer-events: none;
	}

	/* ── Title ── */
	.section-title {
		position: absolute;
		bottom: 3rem;
		left: 2rem;
		z-index: 1;
	}

	/* ── Progress Bar ── */
	.progress-bar {
		position: absolute;
		bottom: 0;
		left: 0;
		right: 0;
		height: 2px;
		background: rgba(255, 255, 255, 0.06);
		z-index: 1;
	}

	.progress-fill {
		width: 100%;
		height: 100%;
		background: linear-gradient(
			90deg,
			var(--color-gold-dark),
			var(--color-gold),
			var(--color-gold-light)
		);
		transform: scaleX(0);
		transform-origin: left center;
		box-shadow: 0 0 12px rgba(255, 200, 80, 0.4);
	}

	/* ── Responsive ── */
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

		.artist-name {
			padding: 1rem 0.75rem 0.75rem;
			font-size: var(--text-sm);
		}

		.orb {
			opacity: 0.5;
		}
	}
</style>
