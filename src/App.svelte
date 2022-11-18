<script>
  import ChatBox from "./ChatBox.svelte";
  import BouncingDots from "./BouncingDots.svelte";
  import { onMount } from "svelte";

  function readingTime(text) {
    const wps = 225 / 60;
    const words = text.trim().split(/\s+/).length;
    const time = Math.ceil((words / wps) * 1000);
    return time;
  }

  const text = "Hi GAGO  KA ANIMAL KA INA MO!";

  const readtime = readingTime(text);
  const time = readtime < 2000 ? 100 : 200;

  let current = [
    {
      text: text,
      who: "them",
      ready: false,
      isolateDelay: time,
    },
  ];

  let active_index = 0;
  let displacement = 0;

  const popchat = () => {
    if (displacement < current[active_index].isolateDelay) {
      displacement += 1;
      requestAnimationFrame(popchat);
    } else {
      current[active_index].ready = true;
    }
  };

  onMount(() => {
    let frame;

    popchat();

    return () => {
      cancelAnimationFrame(frame);
    };
  });

  function popnew() {
    const text =
      "Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industrys standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged. It was popularised in the 1960s with the release of Letraset sheets containing Lorem Ipsum passages, and more recently with desktop publishing software like Aldus PageMaker including versions of Lorem Ipsum.";

    const readtime = readingTime(text);
    const time = readtime < 2000 ? 100 : 200;

    current = [
      ...current,
      {
        text: text,
        who: "them",
        ready: false,
        isolateDelay: time,
      },
    ];
    displacement = 0;
    active_index = current.length - 1;
    popchat();
  }
</script>

<main>
  {#each current as { text, who, ready, isolateDelay }}
    <ChatBox {who} {text} {ready} {isolateDelay} />
  {/each}
  <button on:click={popnew}>pop</button>
</main>

<style>
  :global(body) {
    background: #000;
  }

  main {
    width: 500px;
    margin: 50px auto;
  }
</style>
