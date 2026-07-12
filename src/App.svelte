<script lang="ts">
    import {
        SurfaceComponent,
        SurfaceProvider,
        SankeyChartComponent,
        ControlsComponent
    } from "@visuallyjs/browser-ui-svelte"
    import { EVENT_TAP, type SankeyOptions } from "@visuallyjs/browser-ui"
    import SupplyChainNode from './components/SupplyChainNode.svelte';
    import renderOptions from "./render-options.js"
    import {resolveNodeColor} from "./constants.js";
    import SupplyChainPalette from "./SupplyChainPalette.svelte";
    import SupplyChainInspector from "./Inspector.svelte";

    let url = "/dataset.json";

    let pivotProperty = $state('');

    // options for the graph layout on the lhs
    const viewOptions = {
        nodes: {
            default:{
                component:SupplyChainNode
            }
        },
        edges:{
            default:{
                targetMarker:"PlainArrow",
                events: {
                    [EVENT_TAP]: ({obj, model}) => {
                        model.setSelection(obj)
                    }
                }
            }
        }
    };

    const modelOptions = {
        // this is our default edge spec
        edgeFactory:(model, type, data, cb) => {
            cb({
                type,
                value:100,
                transitMode:"Air",
                carrier:"FedEx"
            })
			return true
        }
    }

    // options for the sankey chart
    const sankeyOptions:SankeyOptions = {
        labelProperty:"name",
        linkColorStrategy:"source",
        colorGenerator: {
            generate:(obj) => resolveNodeColor(obj.type)
        }
    };

</script>

<div class="vjs-supply-chain">
    <div class="vjs-supply-chain-toolbar">
        <strong>Supply Chain Analyzer</strong>
        <div class="pivot-controls">
            <span>Pivot Sankey:</span>
            <select bind:value={pivotProperty}>
                <option value="">Direct (No Pivot)</option>
                <option value="transitMode">By Transit Mode</option>
                <option value="carrier">By Carrier</option>
            </select>
        </div>
    </div>
    <div class="vjs-supply-chain-canvas">
        <SurfaceProvider>
            <SupplyChainPalette/>
            <div class="vjs-supply-chain-view-panel">
                <SurfaceComponent {url}
                                  {modelOptions}
                                  {renderOptions}
                                  {viewOptions}>
                    <ControlsComponent/>
                </SurfaceComponent>
                <SupplyChainInspector/>
            </div>
            <div class="vjs-supply-chain-view-panel">
                <SankeyChartComponent options={sankeyOptions} pivot={pivotProperty}/>
            </div>
        </SurfaceProvider>
    </div>
</div>

<style>
    :global(.vjs-supply-chain) {
        display: flex;
        flex-direction: column;
        height: 100vh;
        width: 100vw;
    }
</style>
