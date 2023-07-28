<script lang="ts">
import { gsap } from "gsap";
import { onMount } from "svelte";
    let ready = false;
    let h1: HTMLHeadingElement;
    onMount(()=>{
    let ss = h1.getElementsByTagName("span");
    for (const s of ss) {
        let t = s.innerText;
        s.innerText = ""
        for(const c of t) {
            let d = document.createElement("div");
            d.innerHTML=c;
            s.appendChild(d);
        }
    }
    gsap.set("h1 div", {display: "inline-block",yPercent: -100, willChange: "transform"})
    gsap.to("h1 div", {
        yPercent: 0,
        duration: 1,
        stagger: 0.01,
        ease: "power4.out",
        absolute: true,
    })
    })
</script>


<section class="md:p-20 flex flex-col">
    <h1 bind:this={h1} class="text-9xl uppercase font-black"><span class="block overflow-hidden relative">Laxmanan</span> <span class="block overflow-hidden relative">Krishnapillai</span></h1>
    <span class="flex gap-4"><button>Reach out!!</button><button>What I am currentlty working on</button></span>
</section>

<style lang="postcss"></style>