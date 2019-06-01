<script>
	let rateProperties = {
		loan:0,
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
		console.log('Loan', loan)
		console.log('Down', downPayment)
		console.log(afterDownpayment)
		let terms = years * termTypes[termType]; //Update 12 to whatever number needed for daily and weekly
		console.log(terms)
		let calculatedRate = ( rate / 100) / termTypes[termType]; //update 12 to whatever number needed for daily and weekly
		console.log(calculatedRate)
		let base = calculatedRate + 1;
		console.log(base)
		rateProperties.payment = afterDownpayment * (calculatedRate * Math.pow(base, terms)) / ((Math.pow(base, terms)) - 1);
	}
</script>


<div>
	<div>
		<label>Enter loan here</label>
		<input type="text" name="loan" on:change={handleInput}>
	</div>
	<div>
		<label>Enter downpayment here</label>
		<input type="text" name="downPayment" on:change={handleInput}>
	</div>
	<div>
		<label>Enter rate here</label>
		<input type="text" name="rate" on:change={handleInput}>
	</div>
	<div>	
		<label>Enter years here</label>
		<input type="text" name="years" on:change={handleInput}>
	</div>
	<div>	
		<label>Select how often you will be paying</label>
		<select name="termType" on:change={handleInput}>
			<option disabled selected value> -- select an option -- </option>
			<option value="monthly">Monthly</option>
			<option value="weekly">Weekly</option>
			<option value="daily">Daily</option>
		</select>
	</div>
	<div>
		<button on:click={calculateMortgatePayments}>Calculate monthly payments</button>
	</div>
	{#if rateProperties.payment}
		You will pay {rateProperties.payment.toFixed(2)} {rateProperties.termType}
	{/if}
</div>
