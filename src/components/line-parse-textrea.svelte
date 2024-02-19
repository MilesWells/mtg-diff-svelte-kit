<script lang="ts">
	import type { HTMLTextareaAttributes } from 'svelte/elements';

	type LineParseTextareaProps = Omit<HTMLTextareaAttributes, 'onchange' | 'value'> & {
		onLinesParsed: (lines: string[]) => void;
	};

	let { onLinesParsed, ...textareaProps } = $props<LineParseTextareaProps>();

	let value: string;

	function parseLinesFromValue(value: string) {
		return value
			.trim()
			.split(/\r\n|\r|\n/)
			.map(parseLine)
			.filter(line => line.length > 0);
	}

	const numberedRegex = /^\d /;

	function parseLine(line: string) {
		if (!numberedRegex.test(line)) return line.trim();

		return line.replace(numberedRegex, '').trim();
	}
</script>

<textarea
	bind:value
	onchange={() => onLinesParsed(parseLinesFromValue(value))}
	{...textareaProps}
/>

<style>
	textarea {
		height: 45rem;
		resize: vertical;
		width: 100%;
	}
</style>
