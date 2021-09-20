<script>
  import { onMount, onDestroy } from 'svelte';
  import { browser } from '$app/env';

  let observer;

  function handleView() {
    alert('Intersection Observer view event triggered');
  }

  onMount(() => {
    if (browser) {
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
      const element = window.document.querySelector('main').firstElementChild;
      observer.observe(element);
    }
  });

  onDestroy(() => {
    if (observer) {
      observer.disconnect();
    }
  });
</script>
