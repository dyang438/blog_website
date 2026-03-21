<script>
	import { page } from '$app/stores';
	import github from '$lib/images/github.svg';
</script>

<header>
	<div class="corner">
		<a id="username" href="/">David Yang</a>
	</div>

	<nav>
		<ul>
			<li aria-current={$page.url.pathname === '/' ? 'page' : undefined}>
				<a href="/">Home</a>
			</li>
			<li aria-current={$page.url.pathname.startsWith('/blog') || $page.url.pathname.startsWith('/snippets') ? 'page' : undefined}>
				<a href="/blog">Blog</a>
			</li>
		</ul>
	</nav>

	<div class="padding"></div>
</header>

<style>
	header {
		display: flex;
		justify-content: space-between;
		align-items: center;
		background: rgba(255, 255, 255, 0.95);
		backdrop-filter: blur(10px);
		border-bottom: 1px solid rgba(0, 0, 0, 0.05);
		padding: 0 1rem;
		height: 4rem;
		box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
	}

	.corner {
		display: flex;
		align-items: center;
		gap: 0.75rem;
		color: var(--color-text);
		font-weight: 600;
		font-size: 0.9rem;
		letter-spacing: 0.05em;
		text-decoration: none;
		transition: all 0.2s ease;
	}

	#username{
		color: var(--color-text);
		text-decoration: none;
		font-weight: 600;
		transition: color 0.2s ease;
	}

	#username:hover {
		color: var(--color-theme-1);
	}

	.padding{
		width: 3em;
	}

	.corner a {
		display: flex;
		align-items: center;
		justify-content: center;
		text-decoration: none;
	}

	.corner img {
		width: 1.75rem;
		height: 1.75rem;
		object-fit: contain;
		opacity: 0.8;
		transition: opacity 0.2s ease;
	}

	.corner a:hover img {
		opacity: 1;
	}

	nav {
		display: flex;
		justify-content: center;
	}

	ul {
		position: relative;
		padding: 0;
		margin: 0;
		height: 100%;
		display: flex;
		justify-content: center;
		align-items: center;
		list-style: none;
		gap: 1.5rem;
	}

	li {
		position: relative;
		height: 100%;
		display: flex;
		align-items: center;
	}

	li[aria-current='page']::after {
		content: '';
		position: absolute;
		bottom: -0.5rem;
		left: 50%;
		transform: translateX(-50%);
		width: 4px;
		height: 4px;
		border-radius: 50%;
		background-color: var(--color-theme-1);
	}

	nav a {
		display: flex;
		align-items: center;
		padding: 0.5rem 1rem;
		color: var(--color-text);
		font-weight: 500;
		font-size: 0.9rem;
		text-decoration: none;
		transition: all 0.2s ease;
		border-radius: 6px;
	}

	nav a:hover {
		color: var(--color-theme-1);
		background-color: rgba(0, 0, 0, 0.05);
	}
</style>
