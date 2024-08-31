<script>
	import '@fontsource/sarabun';

	import Institutions from '../components/Institutions.svelte';
	import SubjectAreaCodes from '../components/SubjectAreaCodes.svelte';
	import CourseDescriptions from '../components/CourseDescriptions.svelte';

	let currentInstitution = $state();
	let currentSubjectAreaCode = $state();
	let upstreamSource = $state();

	const gridTemplateColumns = () => {
		if (upstreamSource) return '0fr 1fr 4fr 5fr';
		if (currentSubjectAreaCode) return '1fr 1fr 4fr 0fr';
		if (currentInstitution) return '1fr 2fr 0fr 0fr';
		return '1fr 0fr 0fr 0fr';
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
				bind:upstreamSource
			/>
		{/if}
	</div>

	{#if upstreamSource}
		<iframe
			src={'https://course-descriptions.yaffle.xyz/proxy?url=' + encodeURIComponent(upstreamSource)}
			title="Course Descriptions"
		></iframe>
		<div class="institution-title">
			<button onclick={() => (upstreamSource = null)}>
				<svg
					xmlns="http://www.w3.org/2000/svg"
					width="24"
					height="24"
					viewBox="0 0 24 24"
					fill="none"
					stroke="currentColor"
					stroke-width="2"
					stroke-linecap="round"
					stroke-linejoin="round"
				>
					<path
						d="M12 15v3.586a1 1 0 0 1 -1.707 .707l-6.586 -6.586a1 1 0 0 1 0 -1.414l6.586 -6.586a1 1 0 0 1 1.707 .707v3.586h3v6h-3z"
					/>
					<path d="M21 15v-6" />
					<path d="M18 15v-6" />
				</svg>
				{currentInstitution?.institution_name}
			</button>
		</div>
	{/if}
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
			font-family: 'Sarabun', sans-serif;
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
			cursor: pointer;
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
		gap: 0 1px;
		grid-template-columns: var(--cols);
		height: calc(100vh - 4rem);
		overflow: hidden;
		transition: 500ms;
		width: calc(100vw - 8rem);

		grid-template-areas:
			'institutions institution-title institution-title iframe'
			'institutions subject-area-codes course-descriptions iframe';

		& > div {
			background-color: white;
			display: flex;
			flex-direction: column;
			gap: 0;
			overflow-x: hidden;
			overflow-y: auto;
		}

		.institution-title {
			box-sizing: border-box;
			flex-direction: row;
			width: 100%;
			grid-area: institution-title;
			font-size: 1.4rem;
			background-color: var(--primary-color);
			color: #fff;
			font-weight: bold;
			padding: 0.5rem 1rem;

			button {
				align-items: center;
				appearance: none;
				background-color: transparent;
				border-radius: 0;
				border: none;
				color: inherit;
				cursor: pointer;
				display: flex;
				font: inherit;
				gap: 1rem;

				svg {
					transition: transform 100ms;
				}

				&:hover svg,
				&:focus svg {
					transform: translateX(-4px);
				}
			}
		}
	}

	.institutions {
		grid-area: institutions;
	}

	.subject-area-codes {
		grid-area: subject-area-codes;
	}

	.course-descriptions {
		grid-area: course-descriptions;
	}

	iframe {
		height: 100%;
		width: 100%;
		grid-area: iframe;
	}
</style>
