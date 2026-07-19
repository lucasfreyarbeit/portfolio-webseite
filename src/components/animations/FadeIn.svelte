<script>
    import { onMount } from "svelte";

    export let delay = 0;

    let isVisible = false;
    let container;

    onMount(() => {
        const observer = new IntersectionObserver((entries) => {
            entries.forEach((entry) => {
                if (entry.isIntersecting) {
                    isVisible = true;

                    /* Beobachtung nach der ersten Animation beenden */
                    observer.unobserve(entry.target);
                }
            });
        }, {
            root: null,

            /* Animation kurz vor dem vollständigen Eintritt starten */
            rootMargin: "0px 0px -10% 0px",
            threshold: 0
        });

        if (container) {
            observer.observe(container);
        }

        return () => {
            if (container) {
                observer.unobserve(container);
            }
        };
    });
</script>

<div
    bind:this={container}
    class="scroll-reveal"
    class:is-active={isVisible}
    style={`transition-delay: ${delay}ms;`}
>
    <slot />
</div>

<style>
    .scroll-reveal {
        opacity: 0;
        transform: translateY(40px);

        /* Einblendanimation */
        transition:
            opacity 0.8s cubic-bezier(0.16, 1, 0.3, 1),
            transform 0.8s cubic-bezier(0.16, 1, 0.3, 1);

        will-change: opacity, transform;
    }

    .scroll-reveal.is-active {
        opacity: 1;
        transform: translateY(0);
    }
</style>