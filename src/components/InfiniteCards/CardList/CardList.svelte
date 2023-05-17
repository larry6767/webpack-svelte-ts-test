<script lang="ts">
  import { beforeUpdate, afterUpdate } from "svelte";

  export let buttonHeight: number;

  let cardListRef: HTMLDivElement;
  let autoscroll: boolean;

  beforeUpdate(() => {
    autoscroll =
      cardListRef &&
      cardListRef.offsetHeight + cardListRef.scrollTop >
        cardListRef.scrollHeight - 50;
  });

  afterUpdate(() => {
    if (autoscroll) cardListRef.scrollTo(0, cardListRef.scrollHeight);
  });
</script>

<div
  class="card-list"
  style={`max-height: calc(100vh - ${buttonHeight}px - 60px)`}
  bind:this={cardListRef}
>
  <slot />
</div>

<style lang="less">
  .card-list {
    display: flex;
    flex-direction: column;
    overflow-y: scroll;
    scroll-behavior: smooth;
    margin-bottom: 20px;
  }
</style>
