<script>
	import { pb } from '$lib/pocketbase';
	let { institution, subjectAreaCode, upstreamSource = $bindable() } = $props();

	console.log(institution, subjectAreaCode);
	const getCourseDescriptions = () => {
		return pb.collection('Courses').getFullList({
			filter: `
        institution.id = '${institution.id}'
        && subject_area_code = '${subjectAreaCode}'
      `,
			sort: 'subject_area_code'
		});
	};
</script>

{#await getCourseDescriptions() then courseDescriptions}
	{#each courseDescriptions as courseDescription}
		<section>
			<header>
				<strong>{courseDescription.course_code}</strong>
				{courseDescription.course_title}
			</header>
			<p>{courseDescription.course_description}</p>

			<div>
				<a href={courseDescription.source} target="_blank">
					{courseDescription.source}
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
						<path d="M12 6h-6a2 2 0 0 0 -2 2v10a2 2 0 0 0 2 2h10a2 2 0 0 0 2 -2v-6" />
						<path d="M11 13l9 -9" />
						<path d="M15 4h5v5" />
					</svg></a
				>
				<a
					href={courseDescription.source}
					target="_blank"
					onclick={(e) => {
						e.preventDefault();
						upstreamSource = courseDescription.source;
					}}
				>
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
							d="M12 9v-3.586a1 1 0 0 1 1.707 -.707l6.586 6.586a1 1 0 0 1 0 1.414l-6.586 6.586a1 1 0 0 1 -1.707 -.707v-3.586h-3v-6h3z"
						/>
						<path d="M3 9v6" />
						<path d="M6 9v6" />
					</svg></a
				>
			</div>
		</section>
	{/each}
{/await}

<style>
	section {
		border-bottom: 1px solid #ccc;
		padding: 1rem;
	}

	div {
		align-items: center;
		display: flex;
		gap: 0.5rem;
		justify-content: flex-end;
	}

	a {
		align-items: center;
		border: 2px solid transparent;
		border-left: none;
		border-right: none;
		display: flex;
		gap: 0.5rem;
		padding-bottom: 2px;
		text-align: right;

		&:hover {
			border-color: var(--primary-color);
		}
	}
</style>
