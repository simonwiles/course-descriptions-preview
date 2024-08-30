<script>
	import Institutions from '../components/Institutions.svelte';
	import SubjectAreaCodes from '../components/SubjectAreaCodes.svelte';
	import CourseDescriptions from '../components/CourseDescriptions.svelte';

	let currentInstitution = $state();
	let currentSubjectAreaCode = $state();

	const gridTemplateColumns = () => {
		if (currentSubjectAreaCode) return '1fr 1fr 4fr';
		if (currentInstitution) return '1fr 2fr 0fr';
		return '1fr 0fr 0fr';
	};
</script>

<div class="container" style="--cols: {gridTemplateColumns()}">
	<div class="institutions">
		<Institutions bind:currentInstitution bind:currentSubjectAreaCode />
	</div>

	<div class="subject-area-codes">
		{#if currentInstitution}
			<SubjectAreaCodes institution={currentInstitution} bind:currentSubjectAreaCode />
		{/if}
	</div>

	<div class="course-descriptions">
		{#if currentSubjectAreaCode}
			<CourseDescriptions
				institution={currentInstitution}
				subjectAreaCode={currentSubjectAreaCode}
			/>
		{/if}
	</div>
</div>

<style>
	:root {
		--primary-color: hotpink;
		--border-radius: 0.5rem;
	}
	:global {
		body {
			background-image: linear-gradient(to right bottom, rgb(83, 105, 118), rgb(41, 46, 73));
			color: #222;
			font-family: sans-serif;
			font-size: 18px;
			margin: 0;
			padding: 2rem 4rem;
		}

		a {
			color: var(--primary-color);
			font-weight: bold;
			text-decoration: none;
		}

		.waterfall-button {
			appearance: none;
			background-color: transparent;
			border-radius: 0;
			border: none;
			font: inherit;
			margin: 0;
			padding: 1rem;
			text-align: inherit;
			font-weight: bold;
			color: inherit;
			white-space: nowrap;

			&:hover,
			&:focus,
			&[aria-current='true'] {
				background: var(--primary-color);
				color: #fff;
			}

			&:focus {
				outline: 1px solid #fff;
				outline-offset: -4px;
			}
		}
	}

	.container {
		background-color: black;
		border-radius: var(--border-radius);
		display: grid;
		gap: 1px;
		grid-template-columns: var(--cols);
		height: calc(100vh - 4rem);
		overflow: hidden;
		transition: 500ms;
		width: calc(100vw - 8rem);

		& > div {
			background-color: white;
			display: flex;
			flex-direction: column;
			gap: 0;
			overflow-x: hidden;
			overflow-y: auto;
		}
	}
</style>
