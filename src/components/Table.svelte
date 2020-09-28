<script>
  import { onMount } from "svelte";
  import Row from "./Row.svelte";

  let cols = 8;
  let rows = 15;
  let data = {};

  export let tableIdentifier;

  onMount(() => {
    if (typeof window !== "undefined") {
      const localStorageData = window.localStorage.getItem(tableIdentifier);
      if (localStorageData) {
        data = JSON.parse(localStorageData);
      }
    }
  });

  const handleChangedCell = (col, row, value) => {
    if (!data[row]) data[row] = {};
    data[row][col] = value;

    if (window && window.localStorage) {
      window.localStorage.setItem(tableIdentifier, JSON.stringify(data));
    }
  };
</script>

{#each Array(rows) as _, row}
  <Row {row} {cols} rowData={data[row] || {}} {handleChangedCell} />
{/each}
