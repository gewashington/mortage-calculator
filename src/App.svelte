<!-- TODO -->
<!-- Fix inputs to update calculations live -->
<!-- Error Handling -->
<!-- Add UI Framework? -->


<script>
	import Error from './Error.svelte'
	import { fly } from 'svelte/transition';
	let rateProperties = {
		loan: 0,
		downPayment: 0,
		rate: 0,
		years: 1,
		termType: '',
		payment: 0,
	}

	const termTypes = {
		daily: 365,
		weekly: 52,
		monthly: 12,
	}

	let visible = false;
	let errors = {};
	function calculateMortgagePayments() {
		const { loan, downPayment, rate, years, termType } = rateProperties;
		let afterDownpayment = loan - downPayment;
		let terms = years * termTypes[termType];
		let calculatedRate = ( rate / 100) / termTypes[termType]; 
		let base = calculatedRate + 1;
		rateProperties.payment = afterDownpayment * (calculatedRate * Math.pow(base, terms)) / ((Math.pow(base, terms)) - 1);
		visible = true;
	}

	function isError() {
		const { loan, downPayment, years } = rateProperties;
		if (parseInt(downPayment) > parseInt(loan)) {
			errors.downPayment = 'Down payment amount cannot be greater than loan amount';
		}

		if(years <= 0) {
			errors.years = 'Years has to be greater than zero'
		}
		
		return Object.keys(errors).length > 0 ? errors : null;
	}

	function handleInputChange(e) {
		rateProperties[e.target.name] = e.target.value;
	}

	function handleButtonClick() {
		isError();
		calculateMortgagePayments();
	}
</script>

<style>
	p, h2, h3 {
		padding: 0;
		margin: 0;
	}

	label {
		margin-bottom: 8px;
		text-transform: uppercase;
		font-size: 14px;
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
	.input-container, 
	.payment-container, 
	.button-container {
		display: flex;
		align-items: center;
		justify-content: center;
		flex-direction: column;
	}

	.input-container,
	.payment-container,
	.button-container {
		width: 300px;
		height: 65px;
	}

	.container {
		width: 100%;
		height: 100%;
		background: url('images/home3.jpg');
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

    .input-container {
		border: 1px solid gray;
		margin: 12px;
		padding: 12px;
		border-radius: 8px; 
	}
</style>

<div class="container">
	<div class="form">
		<div class="title-container">
			<h2>Mortgage Calculator</h2>
			<p>To see how much you will be paying, fill out the form below</p>
		</div>
		<div class="input-container">
			<label>Enter loan amount</label>
			<input type="number" name="loan" on:change={handleInputChange} bind:value={rateProperties.loan} min=0>
		</div>
		<div class="input-container">
			<label>Enter down payment amount</label>
			<input type="number" name="downPayment" on:change={handleInputChange} bind:value={rateProperties.downPayment} min=0>
		</div>
		<div class="input-container">
			<label>Enter interest rate (%)</label>
			<input type="number" name="rate" on:change={handleInputChange} bind:value={rateProperties.rate} min=0>
		</div>
		<div class="input-container">
			<label>Enter number of years</label>
			<input type="number" name="years" on:change={handleInputChange} bind:value={rateProperties.years} min=1>
		</div>
		<div class="input-container">	
			<label>Select how often you will be paying</label>
			<select name="termType" on:change={handleInputChange}  bind:value={rateProperties.termType}>
				<option disabled selected value> -- select an option -- </option>
				<option value="monthly">Monthly</option>
				<option value="weekly">Weekly</option>
				<option value="daily">Daily</option>
			</select>
		</div>
		<button on:click={handleButtonClick}>Calculate monthly payments</button>
		<div class="payment-container">
			{#if errors}
				{#each Object.values(errors) as error }
					<Error error={error} />
				{/each}
			{/if}
			{#if rateProperties.payment && visible}
				<h3 transition:fly="{{ y: 50, duration: 2000 }}">
					You will pay ${rateProperties.payment.toFixed(2)} {rateProperties.termType}
				</h3>
			{/if}
		</div>
	</div>
</div>
