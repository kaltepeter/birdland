<script>
	import { getContext } from 'svelte';
	import ExpandToggleIcon from '../ExpandToggleIcon.svelte';
	import { get_color } from './temperature_scale.js';

	export let day;
	export let today;
	export let expanded;
	export let scale;

	const { temp } = getContext('utils');

	const days = 'Sun Mon Tues Weds Thurs Fri Sat'.split(' ');

	const get_day_of_week = day => {
		const [y, m, d] = day.valid_date.split('-');
		const date = new Date(+y, +m - 1, +d);
		return days[date.getDay()];
	};
</script>

<div class="summary" style="background-image: url(icons/weather/{day.weather.icon}.svg)">
	<span>{today ? 'Today' : get_day_of_week(day)}</span>
	<div class="temperature-range-outer">
		<div
			class="temperature-range-inner"
			style="left: {scale(day.low_temp)}%; right: {100 - scale(day.high_temp)}%; --cold: {get_color(day.low_temp)}; --hot: {get_color(day.high_temp)}"
		>
			<span>{$temp(day.low_temp, true)}</span>
			<span>{$temp(day.high_temp, true)}</span>
		</div>
	</div>

	<ExpandToggleIcon {expanded}/>
</div>

<style>
	.summary {
		display: grid;
		grid-template-columns: 7em 1fr 1.2rem;
		background: 0.5em 50% no-repeat;
		background-size: 2em 2em;
		text-align: left;
		padding: 1em 1em 1em 3.5em;
		align-items: center;
	}

	.temperature-range-outer {
		position: relative;
		flex-grow: 1;
		height: 1em;
		margin-right: 3em;
	}

	.temperature-range-inner {
		position: absolute;
		height: 100%;
		background: linear-gradient(to right, var(--cold), var(--hot));
		border-radius: 0.5em;
	}

	.temperature-range-inner span {
		position: absolute;
		padding: 0 0.5em;
	}

	.temperature-range-inner span:first-child {
		right: 100%;
	}

	.temperature-range-inner span:last-child {
		left: 100%;
	}
</style>