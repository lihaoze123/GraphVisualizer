<script setup>
import { ref, watch } from "vue"
import * as vNG from "v-network-graph"
import { VNetworkGraph } from 'v-network-graph'
import 'v-network-graph/lib/style.css'
import Export from './Export.vue'

import dagre from "dagre/dist/dagre.min.js"

const props = defineProps({
  userInput: {
    type: String,
    default: ''
  },
  graphType: {
    type: String,
    default: 'directed'
  }
})

const nodeSize = 40
const graph = ref()
const graphData = ref({
  nodes: {},
  edges: {},
  layouts: {
    nodes: {}
  }
})

watch(() => props.userInput, (newInput) => {
  if (!newInput) {
    graphData.value = {
      nodes: {},
      edges: {},
      layouts: { nodes: {} }
    }
    return
  }

  const lines = newInput.trim().split('\n')
  const nodes = new Set()
  const edges = {}
  let edgeId = 1

  lines.forEach(line => {
    const [source, target] = line.trim().split(/\s+/).map(Number)
    if (!isNaN(source) && !isNaN(target)) {
      nodes.add(source)
      nodes.add(target)
      edges[`e${edgeId}`] = {
        source: source.toString(),
        target: target.toString()
      }
      edgeId++
    }
  })

  graphData.value.nodes = {}
  nodes.forEach(node => {
    graphData.value.nodes[node] = {
      name: node.toString()
    }
  })
  graphData.value.edges = edges
  graphData.value.layouts = { nodes: {} }

  layout("TB")
})

const configs = ref(vNG.defineConfigs({
  view: {
    scalingObjects: true,
    onBeforeInitialDisplay: () => layout("TB"),
    autoPanAndZoomOnLoad: true,
    minZoomLevel: 0.1,
    maxZoomLevel: 2,
    panEnabled: true,
    zoomEnabled: true,
    fitContentMargin: 20,
  },
  node: {
    normal: { 
      radius: nodeSize / 2, 
      color: "#ffffff",
      strokeWidth: 1,
      strokeColor: "#000000"
    },
    hover: {
      radius: nodeSize / 2,
      color: "#f8f8f8", 
      strokeWidth: 2,
      strokeColor: "#000000"
    },
    label: { 
      direction: "center", 
      color: "#000",
      fontSize: 14,
      fontFamily: "Arial, sans-serif",
    },
  },
  edge: {
    normal: {
      color: "#000",
      width: 1,
    },
    hover: {
      color: "#000",
      width: 1,
    },
    margin: 0,
    marker: {
      target: {
        type: props.graphType === 'directed' ? "arrow" : "none",
        width: 4,
        height: 4,
        margin: 0,
        units: "strokeWidth",
        color: "#000",
      },
    },
  },
}))

watch(() => props.graphType, (newType) => {
  configs.value = vNG.defineConfigs({
    ...configs.value,
    edge: {
      ...configs.value.edge,
      marker: {
        target: {
          type: newType === 'directed' ? "arrow" : "none",
          width: 8,
          height: 8,
          margin: 0,
          units: "strokeWidth",
          color: "#000",
        },
      },
    },
  })
})

function layout(direction) {
  if (Object.keys(graphData.value.nodes).length <= 1 || Object.keys(graphData.value.edges).length == 0) {
    return
  }

  const g = new dagre.graphlib.Graph()
  g.setGraph({
    rankdir: direction,
    nodesep: nodeSize * 2,
    edgesep: nodeSize,
    ranksep: nodeSize * 2,
  })
  g.setDefaultEdgeLabel(() => ({}))

  Object.entries(graphData.value.nodes).forEach(([nodeId, node]) => {
    g.setNode(nodeId, { label: node.name, width: nodeSize, height: nodeSize })
  })

  Object.values(graphData.value.edges).forEach(edge => {
    g.setEdge(edge.source, edge.target)
  })

  dagre.layout(g)

  g.nodes().forEach((nodeId) => {
    const x = g.node(nodeId).x
    const y = g.node(nodeId).y
    graphData.value.layouts.nodes[nodeId] = { x, y }
  })
}

function updateLayout(direction) {
  graph.value?.transitionWhile(() => {
    layout(direction)
  })
}

const zoomLevel = ref(1)
</script>

<template>
  <div class="graph-container">
    <Export :graph="graph" />
    <VNetworkGraph
      ref="graph"
      class="graph"
      v-model:zoom-level="zoomLevel"
      :nodes="graphData.nodes"
      :edges="graphData.edges"
      :layouts="graphData.layouts"
      :configs="configs"
    />
  </div>
</template>

<style scoped>
.graph-container {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100%;
  padding: 20px;
  box-sizing: border-box;
  background-color: #f8f8f8;
}

.graph {
  position: relative;
  display: flex;
  width: 100%;
  height: 100%;
  background-color: #ffffff;
  border-radius: 12px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.06);
  overflow: hidden;
}

:deep(.v-ng-viewport) {
  position: absolute !important;
  width: 100% !important;
  height: 100% !important;
  display: flex;
  justify-content: center;
  align-items: center;
}

:deep(.v-ng-canvas) {
  position: relative;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
