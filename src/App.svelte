<script>
	let rateProperties = {
		loan: 0,
		downPayment: 0,
		rate: 0,
		years: 0,
		termType: '',
		payment: '',
	}

	const termTypes = {
		daily: 365,
		weekly: 52,
		monthly: 12,
	}

	function handleInput(e) {
		rateProperties[e.target.name] = e.target.value;
	}

	function calculateMortgatePayments() {
		const { loan, downPayment, rate, years, termType } = rateProperties;
		let afterDownpayment = loan - downPayment;
		let terms = years * termTypes[termType];
		let calculatedRate = ( rate / 100) / termTypes[termType]; 
		let base = calculatedRate + 1;
		rateProperties.payment = afterDownpayment * (calculatedRate * Math.pow(base, terms)) / ((Math.pow(base, terms)) - 1);
	}
</script>

<style>
	p, h2, h3 {
		padding: 0;
		margin: 0;
	}

	label {
		margin-bottom: 8px;
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
		min-height: 800px;
		background: white;
		border-radius: 8px;
		}

	.title-container {
		display: flex;
		margin-bottom: 20px;
	}

    .input-container {
		border: 1px solid gray;
		margin: 8px;
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
			<input type="text" name="loan" on:change={handleInput}>
		</div>
		<div class="input-container">
			<label>Enter down payment amount</label>
			<input type="text" name="downPayment" on:change={handleInput}>
		</div>
		<div class="input-container">
			<label>Enter interest rate</label>
			<input type="text" name="rate" on:change={handleInput}>
		</div>
		<div class="input-container">
			<label>Enter number of years</label>
			<input type="text" name="years" on:change={handleInput}>
		</div>
		<div class="input-container">	
			<label>Select how often you will be paying</label>
			<select name="termType" on:change={handleInput}>
				<option disabled selected value> -- select an option -- </option>
				<option value="monthly">Monthly</option>
				<option value="weekly">Weekly</option>
				<option value="daily">Daily</option>
			</select>
		</div>
		<div class="button-container">	
			<button on:click={calculateMortgatePayments}>Calculate payments</button>
		</div>
		<div class="payment-container">
			{#if rateProperties.payment}
				<h3>
					You will pay ${rateProperties.payment.toFixed(2)} {rateProperties.termType}
				</h3>
			{/if}
		</div>
	</div>
</div>
