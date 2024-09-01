<script>
	import { pb } from '$lib/pocketbase';

	let { currentInstitution = $bindable(), currentSubjectAreaCode = $bindable() } = $props();

	const getInstitutions = () => {
		return pb.send('/api/institutions', {});
	};
</script>

{#await getInstitutions() then institutions}
	{#each institutions as institution}
		<button
			onclick={() => {
				currentInstitution = institution;
				currentSubjectAreaCode = null;
			}}
			aria-current={currentInstitution?.id === institution?.id}
			class="waterfall-button"
		>
			<div>
				{institution.institution_name}
				<span>[{institution.count.toLocaleString()}]</span>
			</div>
		</button>
	{/each}
{/await}

<style>
	button div {
		display: flex;
		justify-content: space-between;
		max-width: 300px;
	}

	button:first-child {
		border-radius: var(--border-radius) 0 0 0;
	}

	span {
		font-weight: normal;
	}
</style>
