<script>
  import { onMount } from "svelte";
  var data = []
  const api_key = import.meta.env.VITE_API_KEY
  onMount(async() => {
    data = await fetch(`https://fakestoreapi.com/products/`).then(res => res.json()) 
  })
</script>

<main>
  {#if data.length > 0}
  <table>
    <tr>
      <th>id</th>
      <th>title</th>
      <th>price</th>
      <th>category</th>
    </tr>
    {#each data as elem}
    <tr>
      <td>{elem.id}</td>
      <td class="pointer" on:click={() => document.getElementById(`h${elem.id}`).classList.toggle('hidden')}>{elem.title}</td>
      <td>{elem.price}</td>
      <td>{elem.category}</td>
    </tr>
    <tr class="hidden" id="h{elem.id}">
      <td colspan="6">{elem.description}</td>
      <td><img src="{elem.image}" alt="{elem.title}"></td>
    </tr>
    {/each}
  </table>
  {/if}
</main>

<style>
table {
 font-size: 12px;
}
img {
  width: 300px;
}
tr.hidden {
  display: none;
}
td.pointer {
  cursor: pointer;
}
</style>
