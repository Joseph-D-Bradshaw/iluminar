<script lang="ts">
	import { onMount } from 'svelte';
	import { gsap } from 'gsap';
	import { ScrollTrigger } from 'gsap/ScrollTrigger';

	onMount(() => {
		gsap.registerPlugin(ScrollTrigger);

		// Scroll-triggered stagger reveal
		gsap.from('.about-content details', {
			scrollTrigger: {
				trigger: '#about',
				start: 'top 75%',
				toggleActions: 'play none none reverse'
			},
			opacity: 0,
			y: 40,
			duration: 0.6,
			stagger: 0.15,
			ease: 'power2.out'
		});

		gsap.from('.about-content blockquote', {
			scrollTrigger: {
				trigger: '#about',
				start: 'top 60%',
				toggleActions: 'play none none reverse'
			},
			opacity: 0,
			y: 30,
			duration: 0.8,
			delay: 0.3,
			ease: 'power2.out'
		});

		// Details open/close animation
		document.querySelectorAll('.about-content details').forEach((details) => {
			const body = details.querySelector<HTMLElement>('.details-body');
			const indicator = details.querySelector<HTMLElement>('.indicator');
			const summary = details.querySelector('summary');
			if (!body || !summary) return;

			let isAnimating = false;

			gsap.set(body, {
				height: 0,
				opacity: 0,
				overflow: 'hidden',
				paddingTop: 0,
				paddingBottom: 0
			});

			summary.addEventListener('click', (e) => {
				if (isAnimating) return;
				e.preventDefault();
				const isOpen = (details as HTMLDetailsElement).open;

				if (isOpen) {
					isAnimating = true;
					gsap.to(body, {
						height: 0,
						opacity: 0,
						paddingTop: 0,
						paddingBottom: 0,
						duration: 0.3,
						ease: 'power2.in',
						onComplete: () => {
							details.removeAttribute('open');
							isAnimating = false;
						}
					});
					if (indicator) gsap.to(indicator, { rotation: 0, duration: 0.3, ease: 'power2.out' });
				} else {
					details.setAttribute('open', '');
					requestAnimationFrame(() => {
						gsap.fromTo(
							body,
							{ height: 0, opacity: 0, paddingTop: 0, paddingBottom: 0 },
							{
								height: body.scrollHeight,
								opacity: 1,
								paddingTop: '0.5rem',
								paddingBottom: '1rem',
								duration: 0.4,
								ease: 'power2.out',
								overflow: 'hidden'
							}
						);
					});
					if (indicator) gsap.to(indicator, { rotation: 45, duration: 0.3, ease: 'power2.out' });
				}
			});
		});
	});
</script>

<section id="about" class="section about-section">
	<h2 class="section-title">ABOUT</h2>

	<div class="about-content">
		<details>
			<summary><span>What is Iluminar?</span><span class="indicator">+</span></summary>
			<div class="details-body">
				<p>
					Iluminar is a five-day intimate and immersive gathering. The Festival is made with
					devotion to music, movement, art, healing, and conscious connection. It is a celebration
					created to bring light, love, and inspiration, in support of development and growth.
				</p>
				<p>
					Across two carefully curated stages, you will experience deep electronic journeys and
					soulful acoustic performances. Daily rituals, embodied practices, and transformative
					workshops come together in experiences of presence and exploration.
				</p>
			</div>
		</details>

		<details>
			<summary><span>What can you do?</span><span class="indicator">+</span></summary>
			<div class="details-body">
				<p>
					Wander through flowering fields, immersive art installations, fairy lit forests. Swim in
					the refreshing lake, or heal your dancing body in the <em>löyly</em> of our sauna.
				</p>
				<p>
					Explore our charming marketplace and nourish yourself with delicious, healthy food and
					drinks, prepared with care and intention.
				</p>
			</div>
		</details>

		<details>
			<summary><span>When does the festival start?</span><span class="indicator">+</span></summary>
			<div class="details-body">
				<p>
					On the 12th of August a total solar eclipse will take place. The festival will be held in
					resonance with the energy of this cosmic event — reflecting unity of opposites. A grand
					Fire Closing Ceremony will honor the eternal cycles of death, rebirth, and renewal.
				</p>
			</div>
		</details>

		<blockquote>
			Iluminar is more than a festival. It is a shared field of presence, practice, and becoming. It
			is the feeling of finally coming home.
		</blockquote>
	</div>
</section>

<style>
	.about-section {
		min-height: 100vh;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: start;
		padding: 8rem 2rem 4rem;
		text-align: center;
		position: relative;
		overflow: hidden;
		scroll-margin-top: 70px;
	}

	.section-title {
		margin-bottom: 2rem;
	}

	.about-content {
		max-width: 640px;
		width: 100%;
		text-align: left;
	}

	.about-content details {
		border-bottom: 1px solid var(--color-border);
		padding: 1rem 0;
	}

	.about-content summary {
		font-family: var(--font-heading);
		font-size: var(--text-xl);
		color: var(--color-gold);
		cursor: pointer;
		padding: 0.5rem 0;
		list-style: none;
		display: flex;
		align-items: center;
		justify-content: space-between;
	}

	.about-content summary::-webkit-details-marker {
		display: none;
	}

	.about-content summary:hover {
		color: var(--color-gold-light);
	}

	.indicator {
		font-family: var(--font-body);
		font-size: var(--text-xl);
		color: var(--color-text-muted);
		display: inline-block;
	}

	.details-body {
		padding: 0.5rem 0 1rem;
		display: flex;
		flex-direction: column;
		gap: 1rem;
	}

	.details-body p {
		color: var(--color-text-muted);
		font-size: var(--text-base);
		line-height: var(--leading-relaxed);
	}

	.about-content blockquote {
		font-family: var(--font-heading);
		font-size: var(--text-2xl);
		color: var(--color-gold-light);
		text-align: center;
		padding: 2rem 0;
		margin-top: 1rem;
		line-height: var(--leading-snug);
	}
</style>
