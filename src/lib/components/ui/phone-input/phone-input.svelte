<script lang="ts">
	import CountrySelector from "./country-selector.svelte";
	import { defaultOptions, type Props } from ".";
	import { cn } from "$lib/utils";
	import { TelInput, normalizedCountries } from "svelte-tel-input";
	import "svelte-tel-input/styles/flags.css";

	const countries = normalizedCountries;

	type $$Props = Props;

	let className: $$Props["class"] = undefined;
	export let defaultCountry: $$Props["defaultCountry"] = null;
	export let country: $$Props["country"] = defaultCountry;
	export let options: $$Props["options"] = defaultOptions;
	export let placeholder: $$Props["placeholder"] = undefined;
	export let readonly: $$Props["readonly"] = false;
	export let disabled: $$Props["disabled"] = false;
	export let value: $$Props["value"] = null;
	export let valid: $$Props["valid"] = false;
	export let detailedValue: $$Props["detailedValue"] = null;
	export let order: $$Props["order"] = undefined;
	export let name: $$Props["name"] = undefined;
	export { className as class };

	let el: HTMLInputElement;

	export const focus = () => {
		// sort of an after update kinda thing
		setTimeout(() => {
			el.focus();
		}, 0);
	};
</script>

<div class="flex place-items-center">
	<CountrySelector
		{order}
		{countries}
		bind:selected={country}
		on:select={focus}
	/>
	<TelInput
		{...$$restProps}
		{name}
		bind:country
		bind:detailedValue
		bind:value
		bind:valid
		bind:readonly
		bind:disabled
		bind:placeholder
		bind:el
		{options}
		required
		class={cn(
			"flex h-9 w-full rounded-l-none border-l-0 bg-background px-3 text-sm ring-offset-background file:border-0 file:bg-transparent file:text-sm file:font-medium placeholder:text-muted-foreground focus-visible:outline-none focus-visible:ring-ring disabled:cursor-not-allowed disabled:opacity-50 border-input rounded-r-md border bg-transparent py-1 shadow-sm transition-colors focus-visible:ring-1",
			className,
		)}
	/>
</div>
