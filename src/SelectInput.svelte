<script>
  export let name;
  export let value;
  export let handleInputChange;
  export let termTypes;
</script>

<style>
  .select-container {
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    width: 300px;
    height: 65px;
  }
</style>

<div class="select-container">
  <label>Select how often you will be paying</label>
  <select {value} on:change={handleInputChange} {name}>
    <option disabled selected value>-- select an option --</option>
    {#each termTypes as termType}
      <option value={termType.value}> {termType.key} </option>
    {/each}
  </select>
</div>
