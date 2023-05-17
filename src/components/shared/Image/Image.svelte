<script lang="ts">
  export let src: string;
  export let alt: string;

  async function preload(src: string): Promise<string> {
    const resp = await fetch(src);
    const blob = await resp.blob();

    return new Promise(function (resolve, reject) {
      let reader = new FileReader();
      reader.readAsDataURL(blob);
      reader.onload = () => resolve(reader.result as string);
      reader.onerror = (error) => reject(error);
    });
  }
</script>

{#await preload(src)}
  Image Is Loading
{:then base64}
  <img src={base64} {alt} />
{:catch error}
  Image Not Available
{/await}
