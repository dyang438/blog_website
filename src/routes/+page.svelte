<script>
	import '@fontsource/fraunces/latin-600.css';
	import '@fontsource/source-sans-3/latin-400.css';
	import '@fontsource/source-sans-3/latin-500.css';
	import '@fontsource/source-sans-3/latin-600.css';
	import '@fontsource/source-sans-3/latin-700.css';

	const experiences = [
		{
			company: 'Coming soon',
			badge: null,
			date: 'Mar 2026 – Present',
			role: '',
			logo: null,
			link: null
		},
		{
			company: 'Idler',
			badge: "YC S'25",
			date: 'Jan 2026 – Mar 2026',
			role: 'Founding Engineer',
			desc: 'Fourth engineer shipping long-horizon coding RL environments for a top foundation model lab to frontier specs. Owned scaling infrastructure for long-running agent harnesses end-to-end; redesigned job queue to leverage MQs.',
			logo: '/images/idler.png',
			link: null
		},
		{
			company: 'Amazon Fulfillment and Robotics',
			badge: null,
			date: 'May 2025 – Aug 2025',
			role: 'Intern, Software Development Engineer',
			desc: "Built Amazon's first AI automated chaos testing platform as part of inaugural productivity AI intern cohort. 1/4 projects chosen for VP level adoption, estimated $3M+ annual impact in service uptime and saved dev hours.",
			logo: '/images/amazon.jpg',
			link: null
		},
		{
			company: 'Reticular AI',
			badge: "YC F'24",
			date: 'Dec 2024 – Feb 2025',
			role: 'Intern, Machine Learning Engineer',
			desc: 'Published ICLR paper on SAE scaling laws; first to apply mech interp to structure prediction, cited by Anthropic. As sole engineer on team, owned scaling training bandwidth 100x via cloud streaming and distributed training.',
			logo: '/images/reticular.png',
			link: { href: '/blog/reticular_ai_winter', label: 'Read the writeup →' }
		},
		{
			company: 'Scale AI',
			badge: null,
			date: 'Nov 2024 – Dec 2024',
			role: 'Intern, Technical Advisor - Generative AI',
			desc: 'Contributed competitive programming solutions for prompt-based training of frontier Chain of Thought model.',
			logo: '/images/scale.jpg',
			link: null
		},
		{
			company: 'Nobias Therapeutics',
			badge: null,
			date: 'Jun 2024 – Aug 2024',
			role: 'Intern, Machine Learning Engineering',
			desc: 'Consolidated 20 largest public pharmaceutical databases into proprietary Knowledge Graph Postgres Database.',
			logo: '/images/nobias.jpg',
			link: null
		}
	];

</script>

<svelte:head>
	<title>David Yang</title>
	<meta name="description" content="CS student at UPenn. Machine learning engineer — scaling models, distributed training, interpretability research." />
</svelte:head>

<main class="home">

	<section class="hero">
		<div class="hero-left">
			<h1>David Yang</h1>
			<div class="bio">
				<p>
					CS student at UPenn focused on building actually useful tools. Been gripped by the power generation space as of late. Building now, launching soon.
				</p>
				<p>
					Highlights: Founding Engineer at Idler (YC S'25) shipping coding RL envs to frontier lab;
					previously published at ICLR with Reticular AI (YC F'24); interned at Amazon with an agentic
					project that was presented to VPs.
				</p>
				<p>
					On my <a href="/blog">blog</a> I've written about low-level systems fundamentals, past work
					experiences, and interesting projects.
				</p>
			</div>
		</div>
		<div class="hero-right">
			<img src="/images/david.jpg" alt="David Yang" class="profile-photo" />
		</div>
	</section>

	<div class="bottom-links">
		<a href="https://github.com/dyang438" target="_blank" rel="noopener noreferrer">GitHub</a>
		<a href="https://www.linkedin.com/in/davidyang438/" target="_blank" rel="noopener noreferrer">LinkedIn</a>
		<a href="mailto:davidyang438@gmail.com">Email</a>
		<a href="/blog">Blog</a>
	</div>

	<section class="experience">
		<h2>Experience</h2>
		<div class="experience-list">
			{#each experiences as exp}
				<div class="experience-item">
					<div class="exp-top">
						<div class="exp-logo-slot">
							{#if exp.logo}
								<img src={exp.logo} alt="{exp.company} logo" class="exp-logo" />
							{:else}
								<div class="exp-logo-placeholder">{exp.company[0]}</div>
							{/if}
						</div>
						<div class="exp-meta">
							<div class="exp-header">
								<div class="exp-title">
									<span class="exp-company">{exp.company}</span>
									{#if exp.badge}
										<span class="exp-badge">{exp.badge}</span>
									{/if}
								</div>
								<span class="exp-date">{exp.date}</span>
							</div>
							{#if exp.role}
								<p class="exp-role">{exp.role}</p>
							{/if}
						</div>
					</div>
					{#if exp.link}
						<a href={exp.link.href} class="exp-link">{exp.link.label}</a>
					{/if}
				</div>
			{/each}
		</div>
	</section>

	<section class="paper">
		<h2>Publication</h2>
		<a href="https://arxiv.org/abs/2503.08764" class="paper-card" target="_blank" rel="noopener noreferrer">
			<p class="paper-title">Towards Interpretable Protein Structure Prediction with Sparse Autoencoders</p>
			<p class="paper-venue">ICLR Workshop · Mar 2025</p>
			<p class="paper-desc">Scaled SAE to ESM-2 3B (ESMFold backbone), enabling interpretability of structure prediction for the first time. Revealed that structural determinants are very sparse — 8–32 active latents/position needed for reconstruction.</p>
		</a>
	</section>

</main>

<style>
	.home {
		max-width: 860px;
		margin: 0 auto;
		padding: 4.75rem 2rem 1.5rem;
		font-family: 'Source Sans 3', 'Segoe UI', system-ui, sans-serif;
		font-feature-settings: 'kern' 1, 'liga' 1;
		-webkit-font-smoothing: antialiased;
		--home-ink: hsl(220 20% 11%);
		--home-muted: hsl(218 14% 30%);
		--home-soft: hsl(220 10% 42%);
		--home-faint: hsl(220 9% 52%);
		--home-accent: hsl(352 50% 40%);
		--home-accent-hover: hsl(352 54% 33%);
		--home-border: hsl(220 14% 87%);
		--home-border-strong: hsl(220 12% 78%);
		--home-shadow: hsl(220 30% 20% / 0.07);
		--home-glass-fill: hsl(220 35% 100% / 0.48);
		--home-glass-fill-hover: hsl(220 35% 100% / 0.62);
		--home-glass-edge: hsl(0 0% 100% / 0.58);
		--home-glass-edge-hover: hsl(0 0% 100% / 0.78);
		isolation: isolate;
	}

	/* ── Hero ── */
	.hero {
		display: flex;
		align-items: center;
		gap: 3rem;
		margin-bottom: 1rem;
		padding: 1.75rem 2rem;
		border-radius: 10px;
		border: 1px solid var(--home-glass-edge);
		background: var(--home-glass-fill);
		backdrop-filter: blur(14px) saturate(1.12);
		-webkit-backdrop-filter: blur(14px) saturate(1.12);
		box-shadow:
			0 1px 2px hsl(220 25% 18% / 0.05),
			0 6px 20px hsl(220 30% 16% / 0.06),
			inset 0 1px 0 hsl(0 0% 100% / 0.65);
	}

	.hero-left {
		flex: 1;
		min-width: 0;
		overflow: visible;
	}

	.hero h1 {
		font-family: 'Fraunces', Georgia, 'Times New Roman', serif;
		font-size: 2.85rem;
		font-weight: 600;
		margin: 0 0 0.25rem;
		padding-bottom: 0.12em;
		text-align: left;
		letter-spacing: -0.035em;
		line-height: 1.22;
		color: hsl(352 44% 30%);
		overflow: visible;
	}

	.bio {
		margin: 0.75rem 0 0;
	}

	.bio p {
		font-size: 1rem;
		line-height: 1.7;
		color: var(--home-muted);
		margin: 0 0 0.65rem;
	}

	.bio p:last-child {
		margin-bottom: 0;
	}

	.bio a {
		color: var(--home-accent);
		font-weight: 600;
		text-decoration: none;
		text-underline-offset: 0.15em;
	}

	.bio a:hover {
		color: var(--home-accent-hover);
		text-decoration: underline;
	}


	.hero-right {
		flex-shrink: 0;
	}

	.profile-photo {
		width: 180px;
		height: 180px;
		border-radius: 50%;
		object-fit: cover;
		border: none;
		box-shadow:
			inset 0 0 0 1px hsl(0 0% 100% / 0.45),
			inset 0 2px 4px hsl(0 0% 100% / 0.2),
			0 0 0 1px hsl(220 14% 22% / 0.12),
			0 0 0 4px hsl(35 22% 97%),
			0 0 0 5px hsl(220 10% 72% / 0.32),
			0 4px 14px hsl(220 22% 12% / 0.14),
			0 14px 36px hsl(220 20% 10% / 0.18);
		padding: 0;
		margin: 0;
	}

	/* ── Experience Carousel ── */
	.experience {
		margin-bottom: 4rem;
	}

	.experience > h2 {
		margin-bottom: 1.25rem;
	}

	.experience h2,
	.paper h2,
	.connect h2 {
		font-size: 1.1rem;
		font-weight: 700;
		text-transform: uppercase;
		letter-spacing: 1.2px;
		color: var(--home-soft);
		margin: 0;
	}

	.experience-list {
		display: flex;
		flex-direction: column;
		gap: 0.75rem;
	}

	.experience-item {
		padding: 1.1rem 1.5rem;
		border-radius: 10px;
		border: 1px solid var(--home-glass-edge);
		background: var(--home-glass-fill);
		backdrop-filter: blur(14px) saturate(1.12);
		-webkit-backdrop-filter: blur(14px) saturate(1.12);
		box-shadow:
			0 1px 2px hsl(220 25% 18% / 0.05),
			0 6px 20px hsl(220 30% 16% / 0.06),
			inset 0 1px 0 hsl(0 0% 100% / 0.65);
		display: flex;
		flex-direction: column;
		transition:
			box-shadow 0.22s ease,
			border-color 0.22s ease,
			background 0.22s ease;
	}

	.experience-item:hover {
		border-color: var(--home-glass-edge-hover);
		background: var(--home-glass-fill-hover);
		box-shadow:
			0 2px 4px hsl(220 25% 18% / 0.06),
			0 10px 28px hsl(220 28% 14% / 0.1),
			inset 0 1px 0 hsl(0 0% 100% / 0.75);
	}

	.exp-top {
		display: flex;
		align-items: center;
		gap: 1rem;
	}

	.exp-logo-slot {
		flex-shrink: 0;
		width: 52px;
		height: 52px;
	}

	.exp-logo {
		width: 52px;
		height: 52px;
		object-fit: contain;
		border-radius: 8px;
		padding: 0;
		margin: 0;
	}

	.exp-logo-placeholder {
		width: 52px;
		height: 52px;
		border-radius: 8px;
		background: hsl(220 30% 100% / 0.45);
		border: 1px solid hsl(0 0% 100% / 0.45);
		display: flex;
		align-items: center;
		justify-content: center;
		font-size: 1.3rem;
		font-weight: 700;
		color: var(--home-faint);
	}

	.exp-meta {
		flex: 1;
		min-width: 0;
	}


	.exp-header {
		display: flex;
		justify-content: space-between;
		align-items: center;
		margin-bottom: 0.2rem;
	}

	.exp-title {
		display: flex;
		align-items: center;
		gap: 0.5rem;
	}

	.exp-company {
		font-weight: 700;
		font-size: 1.05rem;
		color: var(--home-ink);
	}

	.exp-badge {
		font-size: 0.7rem;
		font-weight: 600;
		padding: 0.15rem 0.5rem;
		border-radius: 4px;
		background: hsl(220 35% 100% / 0.52);
		border: 1px solid hsl(0 0% 100% / 0.45);
		color: var(--home-soft);
		letter-spacing: 0.3px;
	}

	.exp-date {
		font-size: 0.85rem;
		color: var(--home-soft);
		font-weight: 500;
	}

	.exp-role {
		font-size: 0.9rem;
		font-weight: 600;
		color: var(--home-accent);
		margin: 0;
	}

	.exp-desc {
		font-size: 0.92rem;
		color: var(--home-muted);
		line-height: 1.6;
		margin: 0 0 0.5rem;
	}

	.exp-link {
		font-size: 0.85rem;
		font-weight: 600;
		color: var(--home-accent);
		text-decoration: none;
		margin-top: 0.6rem;
		display: inline-block;
	}

	.exp-link:hover {
		color: var(--home-accent-hover);
		text-decoration: underline;
	}


	/* ── Paper ── */
	.paper {
		margin-bottom: 1rem;
	}

	.paper h2 {
		margin-bottom: 1.25rem;
	}

	.paper-card {
		display: block;
		padding: 1.5rem 1.75rem;
		border-radius: 10px;
		border: 1px solid var(--home-glass-edge);
		background: var(--home-glass-fill);
		backdrop-filter: blur(14px) saturate(1.12);
		-webkit-backdrop-filter: blur(14px) saturate(1.12);
		box-shadow:
			0 1px 2px hsl(220 25% 18% / 0.05),
			0 6px 20px hsl(220 30% 16% / 0.06),
			inset 0 1px 0 hsl(0 0% 100% / 0.65);
		text-decoration: none;
		color: inherit;
		transition:
			box-shadow 0.22s ease,
			border-color 0.22s ease,
			background 0.22s ease;
	}

	.paper-card:hover {
		border-color: var(--home-glass-edge-hover);
		background: var(--home-glass-fill-hover);
		box-shadow:
			0 2px 4px hsl(220 25% 18% / 0.06),
			0 10px 28px hsl(220 28% 14% / 0.1),
			inset 0 1px 0 hsl(0 0% 100% / 0.75);
		text-decoration: none;
	}

	.paper-title {
		font-size: 1rem;
		font-weight: 700;
		color: var(--home-accent);
		margin: 0 0 0.3rem;
	}

	.paper-venue {
		font-size: 0.8rem;
		font-weight: 600;
		color: var(--home-faint);
		text-transform: uppercase;
		letter-spacing: 0.5px;
		margin: 0 0 0.75rem;
	}

	.paper-desc {
		font-size: 0.92rem;
		color: var(--home-muted);
		line-height: 1.6;
		margin: 0;
	}

	/* ── Contact links (below hero) ── */
	.bottom-links {
		display: flex;
		gap: 1.5rem;
		flex-wrap: wrap;
		justify-content: flex-start;
		padding: 0 0 1.25rem;
		margin: 0 0 2rem;
		border-bottom: 1px solid var(--home-border);
	}

	.bottom-links a {
		font-size: 0.9rem;
		font-weight: 600;
		color: var(--home-soft);
		text-decoration: none;
		transition: color 0.15s ease;
	}

	.bottom-links a:hover {
		color: var(--home-accent-hover);
		text-decoration: none;
	}

	@media (prefers-reduced-transparency: reduce) {
		.hero,
		.experience-item,
		.paper-card {
			backdrop-filter: none;
			-webkit-backdrop-filter: none;
			background: hsl(220 28% 98% / 0.96);
		}

		.experience-item:hover,
		.paper-card:hover {
			background: hsl(220 26% 99% / 0.98);
		}
	}

	/* ── Responsive ── */
	@media (max-width: 640px) {
		.home {
			padding: 4.5rem 1.25rem 3rem;
		}

		.hero {
			flex-direction: column-reverse;
			align-items: center;
			text-align: center;
			gap: 2rem;
			padding: 1.5rem 1.35rem;
		}

		.hero h1 {
			text-align: center;
			font-size: 2.2rem;
		}

		.bottom-links {
			justify-content: center;
		}

		.profile-photo {
			width: 140px;
			height: 140px;
		}

		.exp-date {
			display: none;
		}
	}
</style>
