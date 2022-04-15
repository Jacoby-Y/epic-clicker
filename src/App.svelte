<script>
	import { onDestroy } from "svelte";

	let cash = 0;
	let perClick = 1;
	let perSec = -1;
	let cashClickCost = 25;
	let cashSecCost = 10;

	const upgrCashClick = ()=>{
		if (cash < cashClickCost) return;
		perClick++;
		cash -= cashClickCost;
	}
	const upgrCashSec = ()=>{
		if (cash < cashSecCost) return;
		perSec++;
		cash -= cashSecCost;
	}
	const sackPerSec = ()=>{
		perSec--;
		cash += perClick * 5;
	}

	const idle_id = setInterval(() => {
		cash += perSec;
	}, 1000);
	onDestroy(()=> clearInterval(idle_id));
</script>

<main>
	{#if perSec < 0}<h4>YOU'RE LOSING {Math.abs(perSec)} CASH PER SECOND! :U</h4>
	{:else if perSec > 0}<h4>You are gaining {Math.abs(perSec)} cash per second! ;)</h4>
	{:else}<h4>You are sterile... :|</h4>{/if}

	<h3 id="cash">Cash: {cash.toLocaleString("en")}</h3>
	<button on:click={()=> cash+=perClick}>Get {perClick} Cash</button>
	{#if cash >= 100 || perClick > 1} <br><button on:click={upgrCashClick}>Cash/Click++ (${cashClickCost})</button> {/if}
	{#if cash >= 200 || perSec > 0} <br><button on:click={upgrCashSec}>Cash/Second++ (${cashSecCost})</button> {/if}
	{#if perSec > 0} <br><button on:click={sackPerSec}>Cash/Second-- ("free" money)</button> {/if}
	<!-- {#if perSec == 0} <br><button on:click={()=> perSec = 1}>Unsterilize Yourself</button> {/if} -->
</main>

<style>
	main {
		text-align: center;
		position: absolute;
		top: 35%; left: 50%;
		transform: translate(-50%, 0%);
	}
	#cash, button {
		padding: 0.5rem 0.7rem;
	}
	button {
		border-radius: none;
		background-color: #c6f1ff;
		margin-bottom: 5px;
		min-width: max-content;
		width: 12vw;
	}
</style>