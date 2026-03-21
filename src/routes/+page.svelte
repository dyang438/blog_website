<script>
	const experiences = [
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
			<p class="headline">Machine Learning Engineer</p>
			<p class="bio">
				CS student at UPenn focused on scaling ML systems. Most recently Founding Engineer at Idler (YC S'25); previously published at ICLR with Reticular AI (YC F'24) and interned at Amazon. On my <a href="/blog">blog</a> I write about the low-level systems fundamentals — cache, compilers, memory — that make scaling possible.
			</p>
		</div>
		<div class="hero-right">
			<img src="/images/david.jpg" alt="David Yang" class="profile-photo" />
		</div>
	</section>

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
							<p class="exp-role">{exp.role}</p>
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
			<p class="paper-venue">ICLR Workshop · Mar 2025 · Cited by Anthropic</p>
			<p class="paper-desc">Scaled SAE to ESM-2 3B (ESMFold backbone), enabling interpretability of structure prediction for the first time. Revealed that structural determinants are very sparse — 8–32 active latents/position needed for reconstruction.</p>
		</a>
	</section>

	<div class="bottom-links">
		<a href="https://github.com/dyang438" target="_blank" rel="noopener noreferrer">GitHub</a>
		<a href="https://www.linkedin.com/in/davidyang438/" target="_blank" rel="noopener noreferrer">LinkedIn</a>
		<a href="mailto:davidyang438@gmail.com">Email</a>
		<a href="/blog">Blog</a>
	</div>

</main>

<style>
	.home {
		max-width: 860px;
		margin: 0 auto;
		padding: 3rem 2rem 1.5rem;
	}

	/* ── Hero ── */
	.hero {
		display: flex;
		align-items: center;
		gap: 3rem;
		margin-bottom: 2rem;
		padding: 2rem 0 1rem;
		border-bottom: 1px solid rgba(0, 0, 0, 0.08);
	}

	.hero-left {
		flex: 1;
		min-width: 0;
	}

	.hero h1 {
		font-size: 2.8rem;
		font-weight: 700;
		margin: 0 0 0.25rem;
		text-align: left;
		letter-spacing: -0.5px;
	}

	.headline {
		font-size: 1.15rem;
		font-weight: 600;
		color: #c0392b;
		margin: 0 0 1rem;
		letter-spacing: 0.3px;
	}

	.bio {
		font-size: 1rem;
		line-height: 1.7;
		color: rgba(0, 0, 0, 0.72);
		margin: 0 0 1.5rem;
	}


	.hero-right {
		flex-shrink: 0;
	}

	.profile-photo {
		width: 180px;
		height: 180px;
		border-radius: 50%;
		object-fit: cover;
		border: 2px solid rgba(192, 57, 43, 0.15);
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
		color: rgba(0, 0, 0, 0.6);
		margin: 0;
	}

	.experience-list {
		display: flex;
		flex-direction: column;
		gap: 0.75rem;
	}

	.experience-item {
		padding: 1.1rem 1.5rem;
		border-radius: 6px;
		border: 1px solid rgba(0, 0, 0, 0.07);
		background: var(--color-bg-2);
		display: flex;
		flex-direction: column;
		transition: box-shadow 0.2s ease, border-color 0.2s ease;
	}

	.experience-item:hover {
		border-color: rgba(0, 0, 0, 0.13);
		box-shadow: 0 2px 10px rgba(0, 0, 0, 0.06);
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
		background: rgba(0, 0, 0, 0.06);
		display: flex;
		align-items: center;
		justify-content: center;
		font-size: 1.3rem;
		font-weight: 700;
		color: rgba(0, 0, 0, 0.3);
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
	}

	.exp-badge {
		font-size: 0.7rem;
		font-weight: 600;
		padding: 0.15rem 0.5rem;
		border-radius: 4px;
		background: rgba(0, 0, 0, 0.06);
		color: rgba(0, 0, 0, 0.45);
		letter-spacing: 0.3px;
	}

	.exp-date {
		font-size: 0.85rem;
		color: rgba(0, 0, 0, 0.45);
		font-weight: 500;
	}

	.exp-role {
		font-size: 0.9rem;
		font-weight: 600;
		color: #c0392b;
		margin: 0;
	}

	.exp-desc {
		font-size: 0.92rem;
		color: rgba(0, 0, 0, 0.68);
		line-height: 1.6;
		margin: 0 0 0.5rem;
	}

	.exp-link {
		font-size: 0.85rem;
		font-weight: 600;
		color: #c0392b;
		text-decoration: none;
		margin-top: 0.6rem;
		display: inline-block;
	}

	.exp-link:hover {
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
		border-radius: 6px;
		border: 1px solid rgba(0, 0, 0, 0.07);
		background: var(--color-bg-2);
		text-decoration: none;
		color: inherit;
		transition: box-shadow 0.2s ease, border-color 0.2s ease;
	}

	.paper-card:hover {
		border-color: rgba(0, 0, 0, 0.13);
		box-shadow: 0 2px 10px rgba(0, 0, 0, 0.06);
		text-decoration: none;
	}

	.paper-title {
		font-size: 1rem;
		font-weight: 700;
		color: #c0392b;
		margin: 0 0 0.3rem;
	}

	.paper-venue {
		font-size: 0.8rem;
		font-weight: 600;
		color: rgba(0, 0, 0, 0.4);
		text-transform: uppercase;
		letter-spacing: 0.5px;
		margin: 0 0 0.75rem;
	}

	.paper-desc {
		font-size: 0.92rem;
		color: rgba(0, 0, 0, 0.68);
		line-height: 1.6;
		margin: 0;
	}

	/* ── Bottom links ── */
	.bottom-links {
		display: flex;
		gap: 1.5rem;
		flex-wrap: wrap;
		justify-content: center;
		padding-top: 2rem;
		border-top: 1px solid rgba(0, 0, 0, 0.08);
		margin-top: 2rem;
	}

	.bottom-links a {
		font-size: 0.9rem;
		font-weight: 600;
		color: rgba(0, 0, 0, 0.45);
		text-decoration: none;
		transition: color 0.15s ease;
	}

	.bottom-links a:hover {
		color: #c0392b;
		text-decoration: none;
	}

	/* ── Responsive ── */
	@media (max-width: 640px) {
		.home {
			padding: 2rem 1.25rem 3rem;
		}

		.hero {
			flex-direction: column-reverse;
			align-items: center;
			text-align: center;
			gap: 2rem;
		}

		.hero h1 {
			text-align: center;
			font-size: 2.2rem;
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
