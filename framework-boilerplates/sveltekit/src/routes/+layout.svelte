<script>
	import Header from '$lib/header/Header.svelte';
  import { webVitals } from '$lib/vitals';
  import { browser, dev } from '$app/env';
  import { page } from '$app/stores';
  import { inject, pageview } from '@vercel/analytics';
  import '../app.css';

  let analyticsId = import.meta.env.VERCEL_ANALYTICS_ID;

  // Inject Vercel Web Analytics
  if (browser) {
    inject({
      mode: dev ? 'development' : 'production',
      framework: 'sveltekit'
    });
  }

  // Track page views on route changes
  $: if (browser && $page.url.pathname) {
    pageview({
      route: $page.route?.id || $page.url.pathname,
      path: $page.url.pathname
    });
  }

  // Track web vitals
  $: if (browser && analyticsId) {
    webVitals({
      path: $page.url.pathname,
      params: $page.params,
      analyticsId
    })
  }
</script>

<Header />

<main>
	<slot />
</main>

<footer>
	<p>visit <a href="https://kit.svelte.dev">kit.svelte.dev</a> to learn SvelteKit</p>
</footer>

<style>
	main {
		flex: 1;
		display: flex;
		flex-direction: column;
		padding: 1rem;
		width: 100%;
		max-width: 1024px;
		margin: 0 auto;
		box-sizing: border-box;
	}

	footer {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		padding: 40px;
	}

	footer a {
		font-weight: bold;
	}

	@media (min-width: 480px) {
		footer {
			padding: 40px 0;
		}
	}
</style>
