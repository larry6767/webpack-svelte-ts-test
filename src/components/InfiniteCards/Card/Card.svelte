<script lang="ts">
  import type { CardModel } from "./types";
  import { onMount } from "svelte";

  let urlForFetchingData = process.env.urlForFetchingData;
  if (!urlForFetchingData)
    throw new Error("You must to pass '.env' variables! See '.env.example.'");

  let card: CardModel | null = null;

  onMount(async () => {
    const response = await fetch(
      `${urlForFetchingData}/api/coffee/random_coffee`
    );
    card = await response.json();
  });

  function appearance(_: unknown, { duration }: { duration: number }) {
    return {
      duration,
      css: (t: number) => {
        return `
					transform: scale(${t});`;
      },
    };
  }
</script>

<div class="card" in:appearance={{ duration: 500 }}>
  <div class="image-wrapper plug">
    <img src="" alt="" />
  </div>

  <div class="card-body">
    <h3 class="title plug">{card?.blend_name || ""}</h3>

    {#if !card}
      <div class="characteristics">
        <div class="characteristic plug" />
        <div class="characteristic plug" />
        <div class="characteristic plug" />
      </div>
    {:else}
      <div class="characteristics">
        <div class="characteristic plug">
          <span class="label">origin</span>: <span>{card.origin}</span>
        </div>
        <div class="characteristic plug">
          <span class="label">variety</span>: <span>{card.variety}</span>
        </div>
        <div class="characteristic plug">
          <span class="label">intensifier</span>:
          <span>{card.intensifier}</span>
        </div>
      </div>
    {/if}

    <div class="notes-wrapper">
      {#each card?.notes.split(", ") || [1, 2] as note}
        <span class="note plug">{typeof note === "string" ? note : ""}</span>
      {/each}
    </div>
  </div>
</div>

<style lang="less">
  @import "../../../variables.less";

  .card {
    overflow: hidden;
    display: flex;
    flex-direction: column;
    width: 280px;
    min-height: 400px;
    flex-shrink: 0;
    margin-bottom: 20px;
    background: @primary-1;
    border-radius: @border-radius;

    &:last-child {
      margin-bottom: 0;
    }
  }

  .image-wrapper {
    overflow: hidden;
    display: flex;
    justify-content: center;
    align-items: center;
    width: 280px;
    min-height: 220px;
    background: @plug-4;
  }

  .card-body {
    padding: 20px 20px 10px;
    display: flex;
    flex-grow: 1;
    flex-direction: column;
  }

  .characteristics {
    margin-bottom: 20px;
  }

  .characteristic {
    margin-bottom: 2px;
  }

  .label {
    text-transform: capitalize;
    font-weight: bold;
  }

  .notes-wrapper {
    display: flex;
    overflow-x: auto;
    margin-top: auto;
    padding: 10px 0;
  }

  .note {
    display: flex;
    flex-shrink: 0;
    padding: 2px 10px;
    border-radius: @border-radius;
    background: @primary-3;
    margin-right: 5px;
  }

  .title {
    font-size: 18px;
    line-height: 22px;
    min-height: 22px;
    margin: 18px 0;
  }

  .plug:empty {
    display: block;
    height: 18px;
    min-width: 80px;
    color: @plug-4;
    background: linear-gradient(to right, @plug-4, @plug-3 50%, @plug-4 80%),
      @plug-4;
    background-repeat: repeat-y;
    background-size: 50px 500px;
    background-position: 0 0;
    animation: shine 2s infinite;
  }

  @keyframes shine {
    to {
      background-position: 100% 0, 0 0;
    }
  }
</style>
