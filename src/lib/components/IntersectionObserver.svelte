<script>
	import { onMount, onDestroy } from 'svelte';
	import { browser } from '$app/environment';

	function handleView() {
		alert('Intersection Observer view event triggered');
	}

	let container;

	/**
	 * @type {IntersectionObserver}
	 */
	let observer;

	onMount(() => {
		if (browser) {
			/**
			 * @param entries {IntersectionObserverEntry[]}
			 * @param observer {IntersectionObserver}
			 */
			const handleIntersect = (entries, observer) => {
				entries.forEach((entry) => {
					if (entry.isIntersecting) {
						observer.unobserve(entry.target);
						handleView();
					}
				});
			};
			const options = { threshold: 1, rootMargin: '100% 0% -100%' };
			observer = new IntersectionObserver(handleIntersect, options);
			observer.observe(container);
		}
	});

	onDestroy(() => {
		if (observer) {
			observer.disconnect();
		}
	});
</script>

<div bind:this={container}>
	<slot />
</div>
