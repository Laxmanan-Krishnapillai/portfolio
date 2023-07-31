<script lang="ts">
  import { gsap } from "gsap/dist/gsap";
  import { onMount } from "svelte";
  let ready = false;
  let h1: HTMLHeadingElement;
  onMount(() => {
    let ss = h1.getElementsByTagName("span");
    for (const s of ss) {
      let t = s.innerText;
      s.innerText = "";
      for (const c of t) {
        let d = document.createElement("div");
        d.innerHTML = c;
        s.appendChild(d);
      }
    }
    gsap.set("h1 div", {
      display: "inline-block",
      yPercent: -100,
      willChange: "transform",
    });
    gsap.to("h1 div", {
      yPercent: 0,
      duration: 1,
      stagger: 0.02,
      delay: 0.5,
      ease: "power4.out",
    });
  });
</script>

<section class="md:p-20 flex flex-col gap-20">
  <h1 bind:this={h1} class="text-4xl md:text-9xl uppercase font-black">
    <span class="block overflow-hidden relative">Laxmanan</span>
    <span class="block overflow-hidden relative">Krishnapillai</span>
  </h1>
</section>

<style lang="postcss">
  h1 div {
    display: "inline-block";
    translate: 0 -100%;
    will-change: "transform";
  }
  button {
    @apply px-10 py-4 rounded-md bg-slate-900 text-white font-medium w-min whitespace-nowrap;
  }
</style>
