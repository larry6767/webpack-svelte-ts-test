<script lang="ts">
  import { onDestroy, onMount } from "svelte";

  let cards: Array<number | undefined> = [1];
  let interval: number;

  onDestroy(() => clearInterval(interval));

  onMount(function () {
    interval = setInterval(addNewCard, 10000);
  });

  async function addNewCard() {
    clearInterval(interval);
    cards = [...cards, 1];
    interval = setInterval(addNewCard, 10000);
  }
</script>

<div class="infinite-cards">
  <div>
    {#each cards as _}
      <div>xxx</div>
    {/each}
  </div>

  <button class="button" on:click={addNewCard}>add new card</button>
</div>

<style lang="less">
  @import "../../variables.less";

  .infinite-cards {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 100%;
    padding: 20px;
  }

  .button {
    width: 280px;
    padding: 15px;
    border: none;
    border-radius: 5px;
    background: @primary-2;
    text-transform: capitalize;
    cursor: pointer;

    &:hover {
      background: @primary-1;
    }

    &:disabled {
      background: @primary-2;
      cursor: not-allowed;
    }
  }
</style>
