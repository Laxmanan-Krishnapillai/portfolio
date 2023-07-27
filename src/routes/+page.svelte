<script lang="ts">
import { gsap } from "gsap";
import { onMount } from "svelte";
    let ready = false;
    let h1: HTMLHeadingElement;
    onMount(()=>{
    ready = true;
    const t = h1.innerText.split("");
    for (const tc of t!) {
        const c = document.createElement("div");
        c.innerText = tc;
        h1.appendChild(c)
    }
    gsap.to("h1 > div", {
        color: "red"
    })
    })
</script>


<section class="md:p-20 flex flex-col">
    <h1 bind:this={h1} class="text-6xl flex uppercase font-black relative overflow-hidden">Let's build the web together</h1>
    <span class="flex gap-4"><button>Reach out!!</button><button>What I am currentlty working on</button></span>
</section>