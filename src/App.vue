<script setup>
import Graph from './components/Graph.vue'
import Input from './components/Input.vue'
import { ref } from 'vue'

const input = ref('')
const graphType = ref('directed')
</script>

<template>
  <div class="container">
    <div class="input-panel">
      <Input 
        v-model="input"
        v-model:graphType="graphType"
      />
    </div>
    <div class="graph-panel">
      <Graph 
        :userInput="input"
        :graphType="graphType"
      />
    </div>
  </div>
</template>

<style scoped>
.container {
  display: flex;
  width: 100%;
  height: 100vh;
  background-color: #f5f5f5;
}

.input-panel {
  width: 300px;
  border-right: 1px solid #eee;
  background-color: #fff;
  height: 100%;
}

.graph-panel {
  flex: 1;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px;
  box-sizing: border-box;
}
</style>
