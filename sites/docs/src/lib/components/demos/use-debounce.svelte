<script lang="ts">
	import { useDebounce } from "runed";
	import Input from "../ui/input/input.svelte";
	import Label from "../ui/label/label.svelte";
	import DemoContainer from "$lib/components/demo-container.svelte";
	import { Button } from "$lib/components/ui/button";

	let count = $state(0);
	let logged = $state("");
	let isFirstTime = $state(true);
	let durationMs = $state(1000);

	const logCount = useDebounce(
		() => {
			if (isFirstTime) {
				isFirstTime = false;
				logged = `You pressed the button ${count} times!`;
			} else {
				logged = `You pressed the button ${count} times since last time!`;
			}
			count = 0;
		},
		() => durationMs
	);

	function ding() {
		count++;
		logCount();
	}
</script>

<DemoContainer class="flex flex-col gap-4">
	<div class="flex flex-col gap-1.5">
		<Label for="duration">Debounce duration (ms)</Label>
		<Input id="duration" type="number" bind:value={durationMs} />
	</div>
	<div class="flex items-center gap-4">
		<Button variant="brand" size="sm" onclick={ding}>DING DING DING</Button>
		<Button variant="subtle" size="sm" onclick={logCount.cancel} disabled={!logCount.pending}
			>Cancel message</Button
		>
	</div>
	<p class="mt-2">{logged || "Press the button!"}</p>
</DemoContainer>
