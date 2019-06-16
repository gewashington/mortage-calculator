<script>
  import InputForm from "./InputForm.svelte";
  import SelectInput from "./SelectInput.svelte";
  import Error from "./Error.svelte";
  import { fly } from "svelte/transition";
  let rateProperties = {
    loan: 0,
    downPayment: 0,
    rate: 0,
    years: 1,
    termType: 0
  };

  let payment;

  const termTypes = [
    { key: "Daily", value: 365 },
    { key: "Weekly", value: 52 },
    { key: "Monthly", value: 12 }
  ];

  let visible = false;

  function calculateMortgagePayments() {
    const { loan, downPayment, rate, years, termType } = rateProperties;
    let afterDownpayment = loan - downPayment;
    let terms = years * termType;
    let calculatedRate = rate / 100 / termType;
    let base = calculatedRate + 1;
    payment =
      (afterDownpayment * (calculatedRate * Math.pow(base, terms))) /
      (Math.pow(base, terms) - 1);
    visible = true;
  }

    function isError() {
    const { loan, downPayment, years } = rateProperties;
    let errors = {};
    console.log('errors at beginning', errors)
    if (parseInt(downPayment) > parseInt(loan)) {
      errors.downPayment =
        "Down payment amount cannot be greater than loan amount";
    }

    if (years < 1) {
      errors.years = "Years has to be greater than zero";
    }
      console.log('errors at end', errors)
    return Object.keys(errors).length > 0 ? errors : {};
  }

  function handleInputChange(e) {
    rateProperties[e.target.name] = e.target.value;
  }

  function handleButtonClick() {
    calculateMortgagePayments();
  }
</script>

<style>
  p,
  h2,
  h3 {
    padding: 0;
    margin: 0;
  }

  button {
    width: 100%;
    margin-top: 8px;
    padding: 14px 0;
    background: black;
    color: white;
    text-transform: uppercase;
    cursor: pointer;
  }

  .container,
  .form,
  .title-container,
  .payment-container {
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
  }

  .payment-container {
    width: 300px;
    height: 65px;
  }

  .container {
    width: 100%;
    height: 100%;
    background: url("images/home3.jpg");
  }

  .form {
    padding: 12px;
    max-width: 600px;
    min-height: 750px;
    background: white;
    border-radius: 8px;
  }

  .title-container {
    display: flex;
    margin-bottom: 20px;
  }
</style>

<!-- TODO: 
* Add error handling for infinity error
* Show term type after calculating mortgage
* Iterate properties through input forms
-->

<div class="container">
  <div class="form">
    <div class="title-container">
      <h2>Mortgage Calculator</h2>
      <p>To see how much you will be paying, fill out the form below</p>
    </div>
    <InputForm
      name="loan"
      {handleInputChange}
      value={rateProperties.loan}
      labelText="Enter loan amount" />
    <InputForm
      name="downPayment"
      {handleInputChange}
      value={rateProperties.downPayment}
      labelText="Enter down payment amount" />
    <InputForm
      name="rate"
      {handleInputChange}
      value={rateProperties.rate}
      labelText="Enter interest rate(%)" />
    <InputForm
      name="years"
      {handleInputChange}
      value={rateProperties.years}
      labelText="Enter number of repayment years" />
    <SelectInput
      name="termType"
      {termTypes}
      {handleInputChange}
      value={rateProperties.termType} />
    <button on:click={handleButtonClick}>Calculate monthly payments</button>
    <div class="payment-container">
      {#if Object.keys(isError()).length > 0}
        {#each Object.values(isError()) as error}
          <Error {error} />
        {/each}
      {:else if payment && visible}
        <h3 transition:fly={{ y: 50, duration: 2000 }}>
          You will pay ${payment.toFixed(2)}
        </h3>
      {/if}
    </div>
  </div>
</div>
