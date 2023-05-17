<script lang="ts">
  import { onDestroy, onMount } from "svelte";
  // local libs
  import CardList from "./CardList/CardList.svelte";
  import Card from "./Card/Card.svelte";
  import { envErrorMessage } from "../../constants";

  const infiniteCardsTimer = process.env.infiniteCardsTimer;
  if (!infiniteCardsTimer) throw new Error(envErrorMessage);

  let buttonRef: HTMLButtonElement;
  $: buttonHeight = buttonRef?.clientHeight || 46; // 46 is default value

  let cards: Array<number | undefined> = [1];
  let interval: number;

  onDestroy(() => clearInterval(interval));

  onMount(function () {
    interval = setInterval(addNewCard, infiniteCardsTimer);
  });

  async function addNewCard() {
    clearInterval(interval);
    cards = [...cards, 1];
    interval = setInterval(addNewCard, infiniteCardsTimer);
  }
</script>

<div class="infinite-cards">
  <CardList {buttonHeight}>
    {#each cards as _}
      <Card />
    {/each}
  </CardList>

  <button class="button" on:click={addNewCard} bind:this={buttonRef}>
    add new card
  </button>
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
    border-radius: @border-radius;
    background: @primary-1;
    text-transform: capitalize;
    cursor: pointer;

    &:hover {
      background: @primary-2;
    }

    &:disabled {
      background: @primary-1;
      cursor: not-allowed;
    }
  }
</style>
