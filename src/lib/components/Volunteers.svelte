<script lang="ts">
	import { gsap } from '$lib/utils/gsap';
	import type { Attachment } from 'svelte/attachments';

	const hoverSwap: Attachment = (node) => {
		const defaultText = node.querySelector<HTMLElement>('[data-text="default"]')!;
		const hoverText = node.querySelector<HTMLElement>('[data-text="hover"]')!;

		const ctx = gsap.context(() => {
			gsap.set(hoverText, { opacity: 0, y: 12 });

			const tl = gsap.timeline({ paused: true });
			tl.to(defaultText, { opacity: 0, y: -12, duration: 0.3, ease: 'power2.in' }, 0);
			tl.to(hoverText, { opacity: 1, y: 0, duration: 0.3, ease: 'power2.out' }, 0);

			const handleEnter = () => tl.play();
			const handleLeave = () => tl.reverse();

			node.addEventListener('mouseenter', handleEnter);
			node.addEventListener('mouseleave', handleLeave);

			return () => {
				node.removeEventListener('mouseenter', handleEnter);
				node.removeEventListener('mouseleave', handleLeave);
			};
		});

		return () => ctx.revert();
	};
</script>

<section id="volunteers" class="section volunteers-section">
	<div class="volunteers-layout">
		<div class="volunteers-text">
			<h2 class="volunteers-heading" {@attach hoverSwap}>
				<span data-text="default">Volunteers</span>
				<span data-text="hover">Join the Team</span>
			</h2>
		</div>
		<div class="volunteers-left">
			<h2 class="volunteers-participate">Participate</h2>
			<div class="volunteers-image" aria-hidden="true"></div>
		</div>
	</div>
</section>

<style>
	.volunteers-section {
		min-height: 100vh;
		display: flex;
		align-items: center;
		justify-content: center;
		padding: 0;
		position: relative;
		overflow: hidden;
	}

	.volunteers-layout {
		display: flex;
		width: 100%;
		max-width: 1400px;
		align-items: flex-end;
		gap: 2rem;
		padding: 4rem 2rem;
	}

	.volunteers-left {
		width: 65%;
		flex-shrink: 0;
		display: flex;
		flex-direction: column;
		gap: 1.5rem;
	}

	.volunteers-image {
		width: 100%;
		height: min(55vh, 500px);
		background: var(--color-border);
		border-radius: var(--radius-md);
	}

	.volunteers-participate {
		font-family: var(--font-heading);
		font-size: clamp(2.5rem, 6vw, 6rem);
		color: var(--color-gold-light);
		line-height: var(--leading-snug);
		margin: 0;
	}

	.volunteers-text {
		flex: 1;
		display: flex;
		align-items: flex-end;
		order: 1;
	}

	.volunteers-heading {
		font-family: var(--font-heading);
		font-size: clamp(2.5rem, 6vw, 6rem);
		color: var(--color-gold-light);
		line-height: var(--leading-snug);
		margin: 0;
		position: relative;
		cursor: pointer;
		user-select: none;
	}

	.volunteers-heading [data-text='hover'] {
		position: absolute;
		inset: 0;
		pointer-events: none;
	}

	@media (max-width: 768px) {
		.volunteers-layout {
			flex-direction: column;
			align-items: flex-start;
			justify-content: flex-end;
			gap: 1rem;
			padding: 1.5rem 1.25rem;
		}

		.volunteers-left {
			width: 100%;
			gap: 0.75rem;
		}

		.volunteers-image {
			height: min(25vh, 220px);
		}

		.volunteers-text {
			width: 100%;
		}

		.volunteers-participate {
			font-size: var(--text-2xl);
		}

		.volunteers-heading {
			font-size: var(--text-2xl);
		}
	}

	@media (max-width: 480px) {
		.volunteers-layout {
			padding: 1rem;
		}

		.volunteers-image {
			height: min(20vh, 160px);
		}

		.volunteers-participate {
			font-size: var(--text-xl);
		}

		.volunteers-heading {
			font-size: var(--text-xl);
		}
	}
</style>
