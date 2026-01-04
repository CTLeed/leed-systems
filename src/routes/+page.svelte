<script lang="ts">
	import { onMount } from 'svelte';
	import { fade, fly } from 'svelte/transition';
	import { base } from '$app/paths';

	let mounted = false;
	let formMessage = '';
	let isSubmitting = false;

	onMount(() => {
		mounted = true;
	});

	async function handleSubmit(event: Event) {
		event.preventDefault();
		const form = event.target as HTMLFormElement;
		const formData = new FormData(form);
		const email = formData.get('email');

		formMessage = 'Submitting…';
		isSubmitting = true;

		try {
			const res = await fetch('https://leed-systems-signup.ctleed.workers.dev', {
				method: 'POST',
				headers: { 'Content-Type': 'application/json' },
				body: JSON.stringify({ email })
			});

			const data = await res.json().catch(() => ({}));
			if (!res.ok) throw new Error(data?.error || 'Unable to subscribe.');

			formMessage = "You're in! Watch your inbox.";
			form.reset();
		} catch (err) {
			formMessage = (err as Error).message || 'Something went wrong.';
		} finally {
			isSubmitting = false;
		}
	}

	const currentYear = new Date().getFullYear();
</script>

<svelte:head>
	<title>Leed Systems - Developer Tools & Automation</title>

	{@html `<script type="application/ld+json">
	{
		"@context": "https://schema.org",
		"@type": "Organization",
		"name": "Leed Systems",
		"description": "Developer tools and digital products focused on automation and clean project scaffolding",
		"url": "https://ctleed.github.io/leed-systems/",
		"logo": "https://ctleed.github.io/leed-systems/LeedSystemsLogo1.png",
		"email": "ctleed@gmail.com",
		"sameAs": [],
		"offers": {
			"@type": "AggregateOffer",
			"name": "Developer Tools & Products",
			"description": "CLI utilities, project templates, and automation tools for developers",
			"url": "https://leed-systems.lemonsqueezy.com/"
		}
	}
	</script>`}
</svelte:head>

<div class="wrap">
	{#if mounted}
		<header class="header" in:fade={{ duration: 400 }}>
			<div class="brand">
				<div class="logo" aria-hidden="true"></div>
				<div>
					<h1 class="brand-title">Leed Systems</h1>
					<p class="brand-sub">Automation • Developer Tools • Digital Products</p>
				</div>
			</div>

			<nav class="nav">
				<a class="pill" href="#products">Products</a>
				<a class="pill" href="#newsletter">Updates</a>
				<a class="pill" href="#contact">Contact</a>
				<a
					class="pill"
					href="https://leed-systems.lemonsqueezy.com/"
					target="_blank"
					rel="noreferrer"
				>
					Shop ↗
				</a>
			</nav>
		</header>

		<main class="hero" in:fly={{ y: 30, duration: 600, delay: 200 }}>
			<div class="hero-inner">
				<div class="kicker">Built for developers who value speed and clarity</div>

				<h2 class="headline">
					Modern scaffolding and automation tools that keep projects moving.
				</h2>

				<p class="lead">
					Leed Systems creates practical CLI utilities, templates, and digital products designed to
					reduce setup friction and help teams ship faster.
				</p>

				<div class="cta-row">
					<a
						class="btn btn-primary"
						href="https://leed-systems.lemonsqueezy.com/"
						target="_blank"
						rel="noreferrer"
					>
						Visit the Shop
					</a>

					<a class="btn" href="mailto:ctleed@gmail.com"> Email Support </a>
				</div>

				<section id="products" class="grid" aria-label="Product highlights">
					<div class="card">
						<div class="card-icon">🛠️</div>
						<h3>CLI Utilities</h3>
						<p>Developer-focused tools for automating setup and daily workflows.</p>
					</div>

					<div class="card">
						<div class="card-icon">📦</div>
						<h3>Project Templates</h3>
						<p>Opinionated starters for modern web stacks with clean structure.</p>
					</div>

					<div class="card">
						<div class="card-icon">⚡</div>
						<h3>Downloadable Tools</h3>
						<p>Focused utilities you can add without committing to a large platform.</p>
					</div>
				</section>

				<section id="newsletter" class="newsletter">
					<div class="newsletter-head">
						<h3>Get updates</h3>
						<p>Occasional emails when new tools or templates are released.</p>
					</div>

					<form class="form" on:submit={handleSubmit}>
						<label class="sr-only" for="email">Email address</label>

						<input
							id="email"
							name="email"
							type="email"
							placeholder="you@domain.com"
							autocomplete="email"
							required
							disabled={isSubmitting}
						/>

						<button class="btn btn-primary" type="submit" disabled={isSubmitting}>
							{isSubmitting ? 'Submitting...' : 'Notify me'}
						</button>
					</form>

					{#if formMessage}
						<p class="form-msg" role="status" aria-live="polite" in:fade={{ duration: 300 }}>
							{formMessage}
						</p>
					{/if}

					<p class="fineprint">No spam. Unsubscribe anytime.</p>
				</section>
			</div>
		</main>

		<footer id="contact" class="footer" in:fade={{ duration: 400, delay: 400 }}>
			<div class="fine">
				Contact:
				<a href="mailto:ctleed@gmail.com">ctleed@gmail.com</a>
			</div>
			<div class="fine">© {currentYear} Leed Systems</div>
		</footer>
	{/if}
</div>

<style>
	:global(:root) {
		--bg1: #0b1220;
		--bg2: #0a1b2e;
		--text: #e5e7eb;
		--muted: #9ca3af;
		--line: rgba(255, 255, 255, 0.1);
		--shadow: 0 10px 30px rgba(0, 0, 0, 0.35);
		--radius: 18px;
		--accent: #60a5fa;
		--accent2: #a78bfa;
	}

	:global(*) {
		box-sizing: border-box;
	}

	:global(body) {
		margin: 0;
		font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
		color: var(--text);
		background: radial-gradient(900px 600px at 15% 10%, rgba(96, 165, 250, 0.22), transparent 55%),
			radial-gradient(800px 500px at 85% 15%, rgba(167, 139, 250, 0.18), transparent 55%),
			linear-gradient(180deg, var(--bg1), var(--bg2));
		line-height: 1.55;
		min-height: 100vh;
	}

	:global(a) {
		color: inherit;
		text-decoration: none;
		transition: all 0.2s ease;
	}

	:global(a:hover) {
		text-decoration: underline;
	}

	.wrap {
		max-width: 980px;
		margin: 0 auto;
		padding: 28px 18px 56px;
	}

	@media (max-width: 640px) {
		.wrap {
			padding: 20px 16px 40px;
		}
	}

	.header {
		display: flex;
		justify-content: space-between;
		align-items: center;
		gap: 14px;
		padding-bottom: 18px;
		flex-wrap: wrap;
	}

	.brand {
		display: flex;
		align-items: center;
		gap: 12px;
	}

	@media (max-width: 640px) {
		.header {
			flex-direction: column;
			align-items: flex-start;
			gap: 16px;
		}
	}

	.logo {
		width: 44px;
		height: 44px;
		border-radius: 14px;
		background-image: url('/LeedSystemsLogo1.png');
		background-size: contain;
		background-position: center;
		background-repeat: no-repeat;
		box-shadow: 0 8px 22px rgba(96, 165, 250, 0.3);
		transition: transform 0.3s ease, box-shadow 0.3s ease;
	}

	.logo:hover {
		transform: scale(1.05);
		box-shadow: 0 12px 28px rgba(96, 165, 250, 0.4);
	}

	.brand-title {
		margin: 0;
		font-size: 16px;
	}

	.brand-sub {
		margin: 0;
		font-size: 13px;
		color: var(--muted);
	}

	@media (max-width: 480px) {
		.brand-sub {
			font-size: 12px;
		}
	}

	.nav {
		display: flex;
		gap: 10px;
		flex-wrap: wrap;
	}

	@media (max-width: 640px) {
		.nav {
			width: 100%;
			justify-content: flex-start;
		}
	}

	.pill {
		padding: 10px 12px;
		border-radius: 999px;
		border: 1px solid var(--line);
		background: rgba(255, 255, 255, 0.04);
		font-size: 14px;
		transition: all 0.2s ease;
	}

	.pill:hover {
		background: rgba(255, 255, 255, 0.08);
		border-color: rgba(255, 255, 255, 0.2);
		text-decoration: none;
		transform: translateY(-1px);
	}

	.hero {
		border-radius: var(--radius);
		border: 1px solid var(--line);
		background: rgba(255, 255, 255, 0.04);
		box-shadow: var(--shadow);
		backdrop-filter: blur(10px);
	}

	.hero-inner {
		padding: 36px 28px;
	}

	@media (max-width: 640px) {
		.hero-inner {
			padding: 24px 20px;
		}
	}

	.kicker {
		font-size: 13px;
		color: var(--muted);
		margin-bottom: 10px;
	}

	.headline {
		font-size: clamp(24px, 7vw, 42px);
		margin: 0 0 12px;
		background: linear-gradient(135deg, var(--text), var(--accent));
		-webkit-background-clip: text;
		-webkit-text-fill-color: transparent;
		background-clip: text;
		line-height: 1.2;
	}

	@media (max-width: 640px) {
		.headline {
			font-size: clamp(22px, 8vw, 32px);
		}
	}

	.lead {
		max-width: 70ch;
		color: rgba(229, 231, 235, 0.9);
		font-size: 16px;
		line-height: 1.6;
	}

	@media (max-width: 640px) {
		.lead {
			font-size: 15px;
		}
	}

	.cta-row {
		display: flex;
		gap: 12px;
		flex-wrap: wrap;
		margin-top: 18px;
	}

	@media (max-width: 640px) {
		.cta-row {
			flex-direction: column;
			gap: 10px;
		}
	}

	.btn {
		padding: 12px 16px;
		border-radius: 12px;
		border: 1px solid var(--line);
		background: rgba(255, 255, 255, 0.06);
		font-weight: 600;
		cursor: pointer;
		transition: all 0.2s ease;
		font-size: 14px;
		text-align: center;
		display: inline-block;
	}

	@media (max-width: 640px) {
		.btn {
			width: 100%;
			padding: 14px 16px;
		}
	}

	.btn:hover {
		background: rgba(255, 255, 255, 0.1);
		text-decoration: none;
		transform: translateY(-2px);
		box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
	}

	.btn-primary {
		background: linear-gradient(135deg, var(--accent), var(--accent2));
		color: #0b1220;
		border-color: transparent;
	}

	.btn-primary:hover {
		box-shadow: 0 4px 20px rgba(96, 165, 250, 0.4);
		color: rgba(132, 170, 246, 0.9);
	}

	.btn:disabled {
		opacity: 0.6;
		cursor: not-allowed;
		transform: none;
	}

	.grid {
		display: grid;
		gap: 12px;
		margin-top: 18px;
		grid-template-columns: 1fr;
	}

	@media (min-width: 640px) {
		.grid {
			grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
		}
	}

	@media (min-width: 760px) {
		.grid {
			grid-template-columns: repeat(3, 1fr);
		}
	}

	.card {
		border: 1px solid var(--line);
		border-radius: var(--radius);
		padding: 16px;
		background: rgba(15, 23, 42, 0.7);
		transition: all 0.3s ease;
	}

	@media (max-width: 640px) {
		.card {
			padding: 20px 16px;
		}
	}

	.card:hover {
		background: rgba(15, 23, 42, 0.9);
		border-color: rgba(96, 165, 250, 0.3);
		transform: translateY(-4px);
		box-shadow: 0 8px 20px rgba(0, 0, 0, 0.3);
	}

	.card-icon {
		font-size: 32px;
		margin-bottom: 8px;
		filter: grayscale(0.3);
		transition: filter 0.3s ease;
	}

	.card:hover .card-icon {
		filter: grayscale(0);
	}

	.card h3 {
		margin: 0 0 8px;
		font-size: 18px;
	}

	.card p {
		margin: 0;
		color: var(--muted);
		font-size: 14px;
		line-height: 1.5;
	}

	.newsletter {
		margin-top: 18px;
		padding: 16px;
		border: 1px solid var(--line);
		border-radius: var(--radius);
		background: rgba(15, 23, 42, 0.6);
	}

	@media (max-width: 640px) {
		.newsletter {
			padding: 20px 16px;
		}
	}

	.newsletter-head h3 {
		margin: 0 0 4px;
	}

	.newsletter-head p {
		margin: 0 0 12px;
		color: var(--muted);
		font-size: 14px;
	}

	.form {
		display: flex;
		gap: 10px;
		flex-wrap: wrap;
	}

	@media (max-width: 640px) {
		.form {
			flex-direction: column;
		}
	}

	input[type='email'] {
		flex: 1;
		min-width: 220px;
		padding: 12px;
		border-radius: 12px;
		border: 1px solid var(--line);
		background: rgba(0, 0, 0, 0.2);
		color: var(--text);
		font-size: 14px;
		transition: all 0.2s ease;
	}

	@media (max-width: 640px) {
		input[type='email'] {
			width: 100%;
			min-width: unset;
			padding: 14px 12px;
		}
	}

	input[type='email']:focus {
		outline: none;
		border-color: var(--accent);
		background: rgba(0, 0, 0, 0.3);
	}

	input[type='email']:disabled {
		opacity: 0.6;
	}

	.form-msg {
		font-size: 13px;
		color: var(--muted);
		margin-top: 8px;
	}

	.fineprint {
		font-size: 12px;
		color: var(--muted);
		margin-top: 8px;
		margin-bottom: 0;
	}

	.footer {
		margin-top: 20px;
		display: flex;
		justify-content: space-between;
		flex-wrap: wrap;
		font-size: 13px;
		color: var(--muted);
		gap: 12px;
	}

	@media (max-width: 640px) {
		.footer {
			flex-direction: column;
			gap: 8px;
			text-align: center;
		}
	}

	.sr-only {
		position: absolute;
		width: 1px;
		height: 1px;
		overflow: hidden;
		clip: rect(0, 0, 0, 0);
	}
</style>
