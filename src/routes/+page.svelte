<svelte:head>
	<title>David Yang - Systems Programming & Low-Level Computing</title>
	<meta name="description" content="Computer Science student at UPenn exploring systems programming, performance optimization, and low-level computing concepts" />
</svelte:head>

<main class="about-page">
	<section class="hero">
		<h1>David Yang</h1>
		<p class="subtitle">Computer Science Student & Systems Programming Enthusiast</p>
	</section>

	<section class="about">
		<div class="about-content">
			<h2>About Me</h2>
			<p>
				I'm a computer science undergraduate at the University of Pennsylvania with a deep fascination for 
				low-level programming and systems architecture. My passion lies in understanding how computers work 
				at the fundamental level—from cache optimization to memory management to performance profiling.
			</p>
			
			<h2>What I Do</h2>
			<p>
				This website serves as my digital laboratory where I document investigations into systems programming 
				concepts, share performance optimization techniques, and explore the intricate details that make 
				modern computing possible. Each post represents a deep dive into a specific topic, complete with 
				benchmarks, code examples, and real-world applications.
			</p>

			<div class="interests">
				<h2>Areas of Focus</h2>
				<ul>
					<li><strong>Performance Optimization</strong> - Cache locality, memory management, and algorithmic efficiency</li>
					<li><strong>Systems Programming</strong> - Low-level data structures, memory allocation, and system calls</li>
					<li><strong>Computer Architecture</strong> - Understanding how hardware and software interact</li>
					<li><strong>Profiling & Analysis</strong> - Assembly analysis, performance measurement, and bottleneck identification</li>
				</ul>
			</div>

			<div class="links">
				<h2>Connect</h2>
				<div class="link-grid">
					<a href="https://github.com/dyang438/blog_investigations" class="link-card" target="_blank" rel="noopener noreferrer">
						<h3>Source Code 📁</h3>
						<p>All investigation code and examples</p>
					</a>
					<a href="https://github.com/dyang438" class="link-card" target="_blank" rel="noopener noreferrer">
						<h3>GitHub 🔗</h3>
						<p>My open source projects and contributions</p>
					</a>
					<a href="mailto:davidyang438@gmail.com" class="link-card">
						<h3>Email 📧</h3>
						<p>davidyang438@gmail.com</p>
					</a>
				</div>
			</div>
		</div>
	</section>
</main>

<style>
	.about-page {
		max-width: 800px;
		margin: 0 auto;
		padding: 2rem;
	}

	.hero {
		text-align: center;
		margin-bottom: 3rem;
		padding: 2rem 0;
	}

	.hero h1 {
		font-size: 3rem;
		margin-bottom: 0.5rem;
		font-weight: 700;
	}

	.subtitle {
		font-size: 1.25rem;
		color: rgba(0, 0, 0, 0.6);
		margin: 0;
	}

	.about-content {
		line-height: 1.7;
	}

	.about-content h2 {
		font-size: 1.5rem;
		margin-top: 2.5rem;
		margin-bottom: 1rem;
		color: var(--color-theme-1);
		font-weight: 600;
	}

	.about-content h2:first-of-type {
		margin-top: 0;
	}

	.about-content p {
		margin-bottom: 1.5rem;
		font-size: 1.05rem;
	}

	.interests ul {
		list-style: none;
		padding: 0;
	}

	.interests li {
		padding: 0.75rem 0;
		border-bottom: 1px solid rgba(0, 0, 0, 0.1);
	}

	.interests li:last-child {
		border-bottom: none;
	}

	.interests strong {
		color: var(--color-theme-1);
		font-weight: 600;
	}

	.link-grid {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
		gap: 1rem;
		margin-top: 1rem;
	}

	.link-card {
		padding: 1.5rem;
		border: 1px solid rgba(0, 0, 0, 0.1);
		border-radius: 8px;
		text-decoration: none;
		color: inherit;
		transition: all 0.2s ease;
		background: rgba(255, 255, 255, 0.5);
	}

	.link-card:hover {
		border-color: var(--color-theme-1);
		background: rgba(255, 255, 255, 0.8);
		transform: translateY(-2px);
		box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
	}

	.link-card h3 {
		margin: 0 0 0.5rem 0;
		font-size: 1.1rem;
		font-weight: 600;
		color: var(--color-theme-1);
	}

	.link-card p {
		margin: 0;
		font-size: 0.9rem;
		color: rgba(0, 0, 0, 0.7);
	}

	@media (max-width: 640px) {
		.about-page {
			padding: 1rem;
		}

		.hero h1 {
			font-size: 2rem;
		}

		.subtitle {
			font-size: 1rem;
		}

		.link-grid {
			grid-template-columns: 1fr;
		}
	}
</style>
