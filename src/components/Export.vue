<script setup>
const props = defineProps({
  graph: {
    type: Object,
    required: true
  }
})

const handleExport = async () => {
  if (!props.graph) return
  
  const text = await props.graph.exportAsSvgText()
  const blob = new Blob([text], { type: 'image/svg+xml' })
  const url = URL.createObjectURL(blob)
  const link = document.createElement('a')
  link.href = url
  link.download = 'network-graph.svg'
  
  document.body.appendChild(link)
  link.click()
  
  document.body.removeChild(link)
  URL.revokeObjectURL(url)
}
</script>

<template>
  <div class="export-container">
    <button class="export-button" @click="handleExport">
      导出为SVG
    </button>
  </div>
</template>

<style scoped>
.export-container {
  position: absolute;
  top: 30px;
  right: 30px;
  z-index: 100;
}

.export-button {
  padding: 10px 20px;
  background-color: #ffffff;
  color: #333333;
  border: 1px solid #dddddd;
  border-radius: 6px;
  cursor: pointer;
  font-size: 14px;
  font-weight: 500;
  transition: all 0.2s ease;
  display: flex;
  align-items: center;
  gap: 6px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.05);
}

.export-button::before {
  content: "↓";
  font-size: 16px;
}

.export-button:hover {
  background-color: #f8f8f8;
  border-color: #cccccc;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
  transform: translateY(-1px);
}

.export-button:active {
  background-color: #f0f0f0;
  transform: translateY(0);
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.05);
}
</style>
