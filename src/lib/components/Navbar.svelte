<script lang="ts">
	import logoSvg from '$lib/assets/text-logo.svg';

	let menuOpen = $state(false);

	function toggle() {
		menuOpen = !menuOpen;
	}

	function close() {
		menuOpen = false;
	}
</script>

<nav class="navbar">
	<div class="nav-inner">
		<div class="nav-left">
			<a href="#about">ABOUT</a>
			<a href="#artists">ARTISTS</a>
		</div>

		<button class="hamburger" onclick={toggle} aria-label="Toggle menu">
			<span class:open={menuOpen}></span>
			<span class:open={menuOpen}></span>
			<span class:open={menuOpen}></span>
		</button>

		<a href="#hero" class="nav-center">
			<img src={logoSvg} alt="iluminar" />
		</a>

		<div class="nav-right">
			<a href="#info">INFO</a>
			<a href="#tickets">TICKETS</a>
		</div>
	</div>
</nav>

<div class="overlay" class:visible={menuOpen} onclick={close} role="presentation"></div>

<aside class="side-menu" class:open={menuOpen}>
	<nav>
		<a href="#about" onclick={close}>ABOUT</a>
		<a href="#artists" onclick={close}>ARTISTS</a>
		<a href="#info" onclick={close}>INFO</a>
		<a href="#tickets" onclick={close}>TICKETS</a>
	</nav>
</aside>

<style>
	.navbar {
		position: fixed;
		top: 0;
		left: 0;
		width: 100%;
		z-index: 100;
		background: linear-gradient(
			to bottom,
			rgba(10, 10, 10, 0.92) 0%,
			rgba(10, 10, 10, 0.6) 60%,
			transparent 100%
		);
	}

	.nav-inner {
		display: grid;
		grid-template-columns: 1fr auto 1fr;
		column-gap: 1.5rem;
		align-items: center;
		max-width: 1200px;
		margin: 0 auto;
		padding: 0.75rem 2rem;
	}

	.nav-left {
		display: flex;
		gap: 2rem;
	}

	.nav-left a {
		font-family: var(--font-body);
		font-size: var(--text-sm);
		font-weight: var(--font-weight-body);
		color: var(--color-text-muted);
		text-decoration: none;
		letter-spacing: 0.12em;
		transition: color var(--transition-fast);
	}

	.nav-left a:hover {
		color: var(--color-gold-light);
	}

	.hamburger {
		display: none;
		flex-direction: column;
		gap: 5px;
		background: none;
		border: none;
		cursor: pointer;
		padding: 4px 0;
	}

	.hamburger span {
		display: block;
		width: 22px;
		height: 2px;
		background: var(--color-text-muted);
		border-radius: 1px;
		transition:
			transform var(--transition-fast),
			opacity var(--transition-fast);
		transform-origin: center;
	}

	.hamburger span.open:first-child {
		transform: translateY(7px) rotate(45deg);
	}

	.hamburger span.open:nth-child(2) {
		opacity: 0;
	}

	.hamburger span.open:last-child {
		transform: translateY(-7px) rotate(-45deg);
	}

	.nav-center {
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.nav-center img {
		height: 28px;
		min-width: 120px;
		display: block;
	}

	.nav-right {
		justify-self: end;
		display: flex;
		gap: 2rem;
	}

	.nav-right a {
		font-family: var(--font-body);
		font-size: var(--text-sm);
		font-weight: var(--font-weight-body);
		color: var(--color-text-muted);
		text-decoration: none;
		letter-spacing: 0.12em;
		transition: color var(--transition-fast);
	}

	.nav-right a:hover {
		color: var(--color-gold-light);
	}

	/* ─ Overlay ─ */
	.overlay {
		position: fixed;
		inset: 0;
		z-index: 90;
		background: rgba(0, 0, 0, 0.6);
		opacity: 0;
		pointer-events: none;
		transition: opacity var(--transition-base);
	}

	.overlay.visible {
		opacity: 1;
		pointer-events: auto;
	}

	/* ─ Side Menu ─ */
	.side-menu {
		position: fixed;
		top: 0;
		left: 0;
		bottom: 0;
		z-index: 95;
		width: 240px;
		background: #000;
		padding: 5rem 0 2rem 2rem;
		transform: translateX(-100%);
		transition: transform var(--transition-base);
	}

	.side-menu.open {
		transform: translateX(0);
	}

	.side-menu nav {
		display: flex;
		flex-direction: column;
		gap: 1.5rem;
	}

	.side-menu a {
		font-family: var(--font-body);
		font-size: var(--text-lg);
		font-weight: var(--font-weight-body);
		color: var(--color-text-muted);
		text-decoration: none;
		letter-spacing: 0.12em;
		transition: color var(--transition-fast);
	}

	.side-menu a:hover {
		color: var(--color-gold-light);
	}

	/* ─ Responsive ─ */
	@media (max-width: 640px) {
		.nav-left,
		.nav-right {
			display: none;
		}

		.hamburger {
			display: flex;
		}
	}
</style>
