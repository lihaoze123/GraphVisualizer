<script setup>
import { ref } from 'vue'

const props = defineProps({
  modelValue: {
    type: String,
    default: ''
  }
})

const emit = defineEmits(['update:modelValue', 'update:graphType'])
const graphType = ref('directed')

const handleInput = (e) => {
  emit('update:modelValue', e.target.value)
}

const handleGraphTypeChange = (type) => {
  graphType.value = type
  emit('update:graphType', type)
}
</script>

<template>
  <div class="input-container">
    <div class="graph-type-selector">
      <button 
        :class="{ active: graphType === 'directed' }"
        @click="handleGraphTypeChange('directed')"
      >
        有向图
      </button>
      <button 
        :class="{ active: graphType === 'undirected' }"
        @click="handleGraphTypeChange('undirected')"
      >
        无向图
      </button>
    </div>
    <textarea 
      :value="modelValue"
      @input="handleInput"
      placeholder="请输入边的关系，每行两个数字，用空格分隔
例如:
1 2
2 3
3 1"
    ></textarea>
    <div class="input-tips">
      <p>输入说明：</p>
      <ul>
        <li>每行输入两个正整数，用空格分隔</li>
        <li>第一个数字表示边的{{ graphType === 'directed' ? '起点' : '端点' }}</li>
        <li>第二个数字表示边的{{ graphType === 'directed' ? '终点' : '端点' }}</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.input-container {
  padding: 20px;
  display: flex;
  flex-direction: column;
  gap: 15px;
  height: 100%;
  box-sizing: border-box;
}

.graph-type-selector {
  display: flex;
  gap: 10px;
  justify-content: center;
}

.graph-type-selector button {
  padding: 8px 16px;
  border: 1px solid #ddd;
  border-radius: 4px;
  background: white;
  cursor: pointer;
  transition: all 0.2s;
}

.graph-type-selector button.active {
  background: #4CAF50;
  color: white;
  border-color: #4CAF50;
}

.graph-type-selector button:hover:not(.active) {
  background: #f5f5f5;
}

.input-container::before,
.input-container::after {
  content: '';
  flex: 1;
}

textarea {
  height: 100%;
  padding: 12px;
  border: 1px solid #ddd;
  border-radius: 4px;
  resize: vertical;
  font-family: monospace;
  font-size: 14px;
  line-height: 1.5;
}

textarea:focus {
  outline: none;
}

.input-tips {
  background-color: #f5f5f5;
  padding: 12px;
  border-radius: 4px;
}

.input-tips p {
  margin: 0 0 8px 0;
  font-weight: bold;
  color: #333;
}

.input-tips ul {
  margin: 0;
  padding-left: 20px;
}

.input-tips li {
  color: #666;
  margin-bottom: 4px;
}

.input-tips li:last-child {
  margin-bottom: 0;
}
</style>
