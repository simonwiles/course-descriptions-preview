<script>
	import { pb } from '$lib/pocketbase';

	let { currentInstitution = $bindable(), currentSubjectAreaCode = $bindable() } = $props();

	const getInstitutions = () => {
		return pb.collection('Institutions').getList(1, 20);
	};
</script>

{#await getInstitutions() then institutions}
	{#each institutions.items as institution}
		<button
			onclick={() => {
				currentInstitution = institution;
				currentSubjectAreaCode = null;
			}}
			aria-current={currentInstitution?.id === institution?.id}
			class="waterfall-button"
		>
			{institution.institution_name}
		</button>
	{/each}
{/await}

<style>
	button:first-child {
		border-radius: var(--border-radius) 0 0 0;
	}
</style>
