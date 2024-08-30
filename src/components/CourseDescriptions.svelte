<script>
	import { pb } from '$lib/pocketbase';
	let { institution, subjectAreaCode } = $props();

	console.log(institution, subjectAreaCode);
	const getCourseDescriptions = () => {
		return pb.collection('Courses').getFullList({
			filter: `
        institution.institution_id = '${institution.institution_id}'
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

			<a href={courseDescription.source} target="_blank">{courseDescription.source}</a>
		</section>
	{/each}
{/await}

<style>
	section {
		border-bottom: 1px solid #ccc;
		padding: 1rem;
	}

	a {
		display: inline-block;
		text-align: right;
		width: 100%;
	}
</style>
