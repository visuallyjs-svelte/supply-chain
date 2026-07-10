<script>
    import {resolveNodeColor} from "../constants";

    let {data, model, vertex} = $props()

    let color = $derived(resolveNodeColor(data.type))
</script>

<div style="--node-color: {color}" data-vjs-target="true" class="vjs-supply-chain-node" onclick={() => model.setSelection(vertex)}>
    <div class="vjs-supply-chain-node-type">{data.type.toUpperCase()}</div>
    <div style="font-size: 14px;">{data.name || data.label}</div>
    <div
        class="vjs-connect"
        data-vjs-source="true"
    ></div>
</div>
