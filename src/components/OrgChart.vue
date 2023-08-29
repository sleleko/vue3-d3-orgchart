<template>
  <div :style="{ height: computedHeight + 'px' }">
    <svg ref="svg" :width="width" :height="computedHeight"></svg>
  </div>
</template>

<script setup>
import { ref, onMounted, reactive, computed, defineProps } from 'vue'
import * as d3 from 'd3'

const props = defineProps(['data', 'width', 'height'])
const svg = ref(null)
const state = reactive({
  root: null,
  maxDepth: 0
})

const computedHeight = computed(() => {
  return (state.maxDepth + 1) * 300
})

const drawChart = () => {
  const svgElement = d3.select(svg.value)
  svgElement.selectAll('*').remove()

  if (!state.root) {
    state.root = d3.hierarchy(props.data)
    state.root.dx = 200
    state.root.dy = 300
    state.root.descendants().forEach((d, i) => {
      d.id = i
      d._children = d.children
      d.children = null
    })
  }

  const tree = d3.tree().nodeSize([state.root.dx, state.root.dy])
  tree(state.root)

  state.maxDepth = Math.max(...state.root.descendants().map(d => d.depth))

  const g = svgElement.append('g').attr('transform', `translate(${props.width / 2},${50})`)

  const link = g
      .append('g')
      .attr('fill', 'none')
      .attr('stroke', '#555')
      .attr('stroke-opacity', 0.4)
      .attr('stroke-width', 1.5)
      .selectAll('path')
      .data(state.root.links())
      .join('path')
      .attr('d', d3.linkVertical().x(d => d.x).y(d => d.y))

  const node = g
      .append('g')
      .attr('stroke-linejoin', 'round')
      .attr('stroke-width', 3)
      .selectAll('g')
      .data(state.root.descendants())
      .join('g')
      .attr('transform', d => `translate(${d.x},${d.y})`)

  const rect = node
      .append('rect')
      .attr('width', 200)
      .attr('height', 90)
      .attr('x', -100)
      .attr('y', -30)
      .attr('fill', d => (d._children || d.children ? '#555' : '#aaa'))
      .attr('stroke', 'white')
      .style('cursor', 'pointer')
      .on('click', (event, d) => {
        d.children = d.children ? null : d._children
        drawChart()
      })

  const text = node
      .append('text')
      .attr('dy', '0.31em')
      .attr('y', -10) // Перемещаем вверх, чтобы сделать место для второй строки
      .attr('text-anchor', 'middle')
      .style('cursor', 'pointer')
      .on('click', (event, d) => {
        d.children = d.children ? null : d._children
        drawChart()
      })

  text.append('tspan') // Первая строка
      .attr('x', 0)
      .attr('dy', '1.2em')
      .text(d => d.data.name)

  text.append('tspan') // Вторая строка
      .attr('x', 0)
      .attr('dy', '1.2em')
      .text(d => d.data.contact)
}

onMounted(() => {
  drawChart()
})
</script>

<style>
svg {
  background-color: white;
}
</style>
