<script>
	import { pb } from '$lib/pocketbase';
	let { institution, currentSubjectAreaCode = $bindable() } = $props();

	const getSubjectAreaCodes = () => {
		return pb.collection('Subject_Area_Codes').getFullList({
			filter: "institution.institution_id = '" + institution.institution_id + "'",
			fields: 'code, count',
			sort: 'code'
		});
	};
</script>

{#await getSubjectAreaCodes() then subjectAreaCodes}
	{#each subjectAreaCodes as { code: subjectAreaCode, count }}
		<button
			onclick={() => (currentSubjectAreaCode = subjectAreaCode)}
			aria-current={currentSubjectAreaCode?.id === institution?.id}
			class="waterfall-button"
		>
			{subjectAreaCode}
			<span>[{count}]</span>
		</button>
	{/each}
{/await}

<style>
	button {
		display: flex;
		justify-content: space-between;
		max-width: 300px;
	}

	span {
		font-weight: normal;
	}
</style>
