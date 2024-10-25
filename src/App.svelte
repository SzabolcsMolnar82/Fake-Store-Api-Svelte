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
      <td class="pointer" on:click={() => {
        document.getElementById(`h${elem.id}`).classList.toggle('hidden')
        document.getElementById(`d${elem.id}`).classList.toggle('hidden')
      }}>{elem.title}</td>
      <td>{elem.price}</td>
      <td>{elem.category}</td>
    </tr>
    <tr class="hidden hx" id="h{elem.id}">
      <td colspan="4"><img src="{elem.image}" alt="{elem.title}"></td>
    </tr>
    <tr class="hidden hx" id="d{elem.id}">
      <td colspan="4">{elem.description}</td>
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
  height: 400px;
  max-width: 500px;
  border:solid 10px white;
  border-radius: 10px;
  box-shadow: 1px 1px 5px black;
}
tr.hidden {
  display: none;
}
td.pointer {
  cursor: pointer;
}
tr.hx td {
  background-color: antiquewhite;
  color: black;
}
</style>
