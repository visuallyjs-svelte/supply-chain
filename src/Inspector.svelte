<script lang="ts">
    import { Node, Edge} from "@visuallyjs/browser-ui"
    import { InspectorComponent } from "@visuallyjs/browser-ui-svelte"
	import {TRANSIT_MODES, CARRIERS} from "./constants"

	let current = $state(null)

</script>

<InspectorComponent className="vjs-supply-chain-inspector" bind:current={current}>
	{#if current?.objectType === Node.objectType}
		<label>
			Name:
			<input type="text" vjs-att="name" placeholder="name"/>
		</label>
	{/if}

	{#if current?.objectType === Edge.objectType}
		<label>
			Value
			<input type="text" vjs-att="value" vjs-datatype="integer"/>
		</label>
		<label>
			Transit Mode
			<select vjs-att="transitMode">
				{#each TRANSIT_MODES as mode}
					<option value={mode}>{mode}</option>
				{/each}
			</select>
		</label>
		<label>
			Carrier
			<select vjs-att="carrier">
				{#each CARRIERS as carrier}
					<option value={carrier}>{carrier}</option>
				{/each}
			</select>
		</label>
	{/if}
</InspectorComponent>
