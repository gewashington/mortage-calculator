<script>
  export let name;
  export let handleInputChange;
  export let labelText;
  export let value;
</script>

<style>
  label {
    margin-bottom: 8px;
    text-transform: uppercase;
    font-size: 14px;
  }

  .input-container {
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    width: 300px;
    height: 65px;
    border: 1px solid gray;
    margin: 12px;
    padding: 12px;
    border-radius: 8px;
  }
</style>

<div class="input-container">
  <label>{labelText}</label>
  <input
    type="number"
    {name}
    on:change={handleInputChange}
    bind:value
    min="0" />
</div>
