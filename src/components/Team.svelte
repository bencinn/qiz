<script>
	export let teamname;
	export let points;
	export let changepoint;
</script>

<button on:click={changepoint}>
	{teamname}: {points}
</button>
