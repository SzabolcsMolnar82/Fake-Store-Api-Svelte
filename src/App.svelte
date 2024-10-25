<script lang="ts">
  import { distance } from 'fastest-levenshtein'

  import { onMount } from "svelte";
  var data = []
  var data2 = []
  var kateg:any = []
  var search = ""
  const api_key = import.meta.env.VITE_API_KEY
  onMount(async() => {
    kateg = new Set()
    data = await fetch(`https://fakestoreapi.com/products/`).then(res => res.json()) 
    data.forEach(elem => {
      kateg.add(elem.category)
    })
    kateg = [...kateg]
    console.log(kateg)
    data2 = data
})
</script>

<main>
  <input type="text" bind:value={search}
  on:input={() => 
  data2 = data.filter(elem => (
    distance(elem.title, search)/search.length < 0.3) 
    ||
    elem.title.includes(search)
    )
  }>
  {#if kateg.length > 0}
  Kategória: 
  <select on:change={(e) => data2 = data.filter(elem => elem.category == e.target.value)}>
    <option value="all">mind</option>
    {#each kateg as kat}
    <option value="{kat}">{kat}</option>
    {/each}
  </select>
  {/if}
  {#if data2.length > 0}
  <table>
    <tr>
      <th>id</th>
      <th>title</th>
      <th>price</th>
      <th>category</th>
    </tr>
    {#each data2 as elem}
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
