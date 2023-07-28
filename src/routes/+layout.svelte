<script lang="ts">
    import "../app.css";
    import { gsap } from "gsap/dist/gsap";
    import { Flip } from "gsap/dist/Flip";
    import { ScrollTrigger } from "gsap/dist/ScrollTrigger";
    import { Observer } from "gsap/dist/Observer";
    import { ScrollToPlugin } from "gsap/dist/ScrollToPlugin";
    import { Draggable } from "gsap/dist/Draggable";
    import { EaselPlugin } from "gsap/dist/EaselPlugin";
    import { MotionPathPlugin } from "gsap/dist/MotionPathPlugin";
    import { PixiPlugin } from "gsap/dist/PixiPlugin";
    import { TextPlugin } from "gsap/dist/TextPlugin";
    import { quartInOut } from "svelte/easing";
    import Icon from '@iconify/svelte';
    import { fly } from "svelte/transition";
import { browser } from "$app/environment";
    gsap.registerPlugin(Flip, ScrollTrigger, Observer, ScrollToPlugin, Draggable, EaselPlugin, MotionPathPlugin, PixiPlugin, TextPlugin);
    let palette: "dark" | "light" = "light";
    let i = "fill-black stroke-0.5 w-auto h-10";
</script>

<header class="flex h-20 justify-between items-center md:px-10">
    <a href="#" on:focusin={(e)=>gsap.to(e.currentTarget, {fontWeight: 800})} on:focusout={(e)=>gsap.to(e.currentTarget, {fontWeight: 300})}>Laxmanan</a>
    <button on:click={()=> palette === "dark" ? palette = "light" : palette = "dark"}>
        {#if palette === "light"}
            <span><Icon class={i} icon="akar-icons:moon"/></span>
        {:else if palette === "dark"}
            <span><Icon class={i} icon="solar:sun-outline"/></span>
        {/if}
    </button>
</header>
<slot /> 

<style lang="postcss">
    :global(html) {
        @apply text-slate-900;
    }
</style>