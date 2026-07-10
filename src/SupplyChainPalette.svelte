<script>
    import {PaletteComponent} from '@visuallyjs/browser-ui-svelte';
    import {NODE_TYPES, resolveNodeType} from "./constants.js";

    /**
     * Component supporting dragging new elements onto the canvas.
     */
    function dataGenerator (el) {
        const type = el.getAttribute("data-vjs-type")
        const nodeType = resolveNodeType(type)
        return {
            type,
            name:nodeType.label
        }
    }
</script>

<PaletteComponent className="vjs-supply-chain-palette" {dataGenerator}>
    {#each NODE_TYPES as nt (nt.type)}
        <div data-vjs-type={nt.type} title="Drag to add new" class="vjs-supply-chain-palette-item" style="--node-color: {nt.color}">
            <div class="vjs-supply-chain-item-header">{nt.type.toUpperCase()}</div>
        </div>
    {/each}
</PaletteComponent>
