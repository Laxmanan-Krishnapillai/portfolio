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
  import Icon from "@iconify/svelte";
  import { browser } from "$app/environment";
  import { onMount, tick } from "svelte";
  gsap.registerPlugin(
    Flip,
    ScrollTrigger,
    Observer,
    ScrollToPlugin,
    Draggable,
    EaselPlugin,
    MotionPathPlugin,
    PixiPlugin,
    TextPlugin,
  );
  let palette: "dark" | "light" = "light";
  let i = "fill-black stroke-0.5 w-auto h-10";
  let logo: HTMLAnchorElement;
  let frameCount = 0; // Variable to keep track of the frame count
  let then = 0;
  const bganim = () => {
    const now = Date.now();
    const deltaTime = (now - then) / 1000; // Calculate time difference in seconds
    const interval = 0.5; // Interval between animations in seconds (change this if needed)

    // Check if the time interval for the animation has passed (every 10th frame)
    if (deltaTime >= interval && frameCount % 500 === 0) {
      gsap.to("html", {
        "--v1n": Math.floor(Math.random() * 100),
        duration: 5,
        ease: "power1.out",
      });
      gsap.to("html", {
        "--v2n": Math.floor(Math.random() * 100),
        duration: 5,
        ease: "power1.out",
      });
      gsap.to("html", {
        "--v3n": Math.floor(Math.random() * 100),
        duration: 5,
        ease: "power1.out",
      });
      then = now; // Reset the timer for the next animation
    }

    frameCount++; // Increment the frame count

    // Request the next animation frame
    requestAnimationFrame(bganim);
  };
  let tl: gsap.core.Timeline
  let mover: gsap.core.Tween
  onMount(() => {
    then = Date.now(); // Initialize 'then' with the current timestamp
    bganim();
    window
      .matchMedia("(prefers-color-scheme: dark)")
      .addEventListener("change", (e) => {
        e.matches ? (palette = "dark") : "light";
      });
    logo.addEventListener("focusin", (e) => fin(e.currentTarget));
    logo.addEventListener("focusout", (e) => fout(e.currentTarget));
    logo.addEventListener("mouseenter", (e) => fin(e.currentTarget));
    logo.addEventListener("mouseleave", (e) => fout(e.currentTarget));
    bganim();
    gsap.set("#cursor", { xPercent: -50, yPercent: -50 });
    tl = gsap.timeline({defaults: {
    ease: "power1.inOut",
    duration: 0.1
    },
  });
    tl.to("#menu div:nth-child(1)", {translateY: "0.75rem"});
    tl.to("#menu div:nth-child(4)", {translateY: "-0.75rem"}, "<");
    tl.to("#menu div:not(:nth-child(2))", {scaleX: 0});
    tl.to("#menu div:nth-child(2)", {scale: menuOpen ? 12 : 8, duration: 0.3, ease: "power4.out"})
    mover = gsap.to("#menu div:nth-child(2)", {scale: "+=4", duration: 0.2}).pause()
    tl.pause();
  });
  const fin = (t: HTMLAnchorElement) => gsap.to(t, { fontWeight: 900 });
  const fout = (t: HTMLAnchorElement) => gsap.to(t, { fontWeight: 500 });
  $: if (browser) {
    palette === "dark"
      ? document.documentElement.classList.add("dark")
      : document.documentElement.classList.remove("dark");
  }
  let out = true;
  const menu = [
    { name: "Home", href: "#" },
    { name: "About", href: "#" },
    { name: "Contact", href: "#" },
  ];
  let menuOpen = false;
  let prev = false;
</script>

<svelte:window
  on:mousemove={(e) => {
    // gsap.quickTo("#cursor", "--x",{duration: 0.5, ease: "power4.out"})(e.clientX);
    // gsap.quickTo("#cursor", "--y",{duration: 0.5, ease: "power4.out"})(e.clientY);
    gsap.to("#cursor", {
      translateX: `${e.clientX}px - 50%`,
      translateY: `${e.clientY}px - 50%`,
    });
  }}
  on:mouseover={(e) => {
    const el = e.target.closest("button, a");
    if (el && out && !prev) {
      console.log(el);
      gsap.to("#cursor", {
        ease: "power4.out",
        duration: 0.3,
        backgroundColor: "transparent",
        scale: 3,
      });
      out = false;
      prev = true;
    } else{
      gsap.to("#cursor", {
        ease: "power4.out",
        duration: 0.3,
        backgroundColor: "rgb(15 23 42)",
        scale: 1,
      });
      out = true;
      prev = false;
    }
  }}
/>
<div
  id="cursor"
  class="fixed pointer-events-none w-10 h-10 rounded-full bg-slate-900 border-[10%] border-slate-900"
/>
<header class="flex h-20 justify-between items-center md:px-10">
  <a class="font-medium p-2" bind:this={logo} href="#">Laxmanan</a>
  <button
    on:click={() =>
      palette === "dark" ? (palette = "light") : (palette = "dark")}
  >
    {#if palette === "light"}
      <Icon class={i} icon="akar-icons:moon" />
    {:else if palette === "dark"}
      <Icon class={i} icon="solar:sun-outline" />
    {/if}
  </button>
</header>
<slot />
<nav class="hidden">
  <ul class="flex gap-2">
    {#each menu as { href, name }}
      <li><a {href}>{name}</a></li>
    {/each}
  </ul>
</nav>
<button
  on:click={async (e) => {
    if(!menuOpen) {tl.play() } else { await mover.reverse().then(); tl.reverse() }
    menuOpen = !menuOpen
  }}
  on:mouseenter={(e)=>{
    menuOpen && mover.play()
  }}
  on:mouseleave={(e)=>{
    menuOpen && mover.reverse()
  }}
  id="menu"
  class="fixed bottom-10 left-1/2 -translate-x-1/2 rounded-full bg-slate-900 flex flex-col gap-2 items-center p-6 justify-center min-w-max aspect-square"
>
  <div class="bg-white pointer-events-none w-10 h-1" />
  <div class="fixed bg-white pointer-events-none w-1 z-50 h-1 rounded-full top-1/2 -translate-y-1/2 -translate-x-1/2"/>
  <div class="bg-white pointer-events-none w-10 h-1" />
  <div class="bg-white pointer-events-none w-10 h-1" />
</button>

<style lang="postcss">
  :global(html) {
    @apply text-slate-900 dark:text-white;
    background-color: hsla(0, 100%, 50%, 1);
    --v1n: 0;
    --v2n: 0;
    --v3n: 0;
    --v1: calc(var(--v1n) * 1%);
    --v2: calc(var(--v2n) * 1%);
    --v3: calc(var(--v3n) * 1%);
    background-image: radial-gradient(
        at 35% 23%,
        hsla(28, 100%, 74%, 1) 0px,
        transparent 50%
      ),
      radial-gradient(at 80% 0%, hsla(189, 100%, 56%, 1) 0px, transparent 50%),
      radial-gradient(
        at 0% var(--v1),
        hsla(355, 100%, 93%, 1) 0px,
        transparent 50%
      ),
      radial-gradient(
        at 80% var(--v2),
        hsla(340, 100%, 76%, 1) 0px,
        transparent 50%
      ),
      radial-gradient(at 0% 100%, hsla(22, 100%, 77%, 1) 0px, transparent 50%),
      radial-gradient(at 80% 100%, hsla(242, 100%, 70%, 1) 0px, transparent 50%),
      radial-gradient(at 0% 0%, hsla(343, 100%, 76%, 1) 0px, transparent 50%);
    height: 100%;
    width: 100%;
  }
  #cursor {
    /* --xpx: calc(var(--x) * 1px); */
    /* --ypx: calc(var(--y) * 1px); */
    /* translate: calc(var(--xpx) - 50%) calc(var(--ypx)  - 50%); */
  }
</style>
