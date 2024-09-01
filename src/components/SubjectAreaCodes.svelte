<script>
	import { pb } from '$lib/pocketbase';
	let { institution, currentSubjectAreaCode = $bindable() } = $props();

	const getSubjectAreaCodes = () => {
		return pb.send(`/api/subject-area-codes/${institution.id}`, {});
	};
</script>

{#await getSubjectAreaCodes() then subjectAreaCodes}
	{#each subjectAreaCodes as { subject_area_code: subjectAreaCode, count }}
		<button
			onclick={() => (currentSubjectAreaCode = subjectAreaCode)}
			aria-current={currentSubjectAreaCode == subjectAreaCode}
			class="waterfall-button"
		>
			{subjectAreaCode}
			<span>[{count.toLocaleString()}]</span>
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
