<script lang="ts">
  import InfiniteCards from "./components/InfiniteCards/InfiniteCards.svelte";
  let name = process.env.name;
</script>

<h1>Greetings {name}</h1>
<InfiniteCards />

<style lang="less">
  @color: purple;
  h1 {
    color: powderblue;
  }
</style>
