<template>
  <div ref="stage" class="stage">
    <div
      ref="canvas"
      :class="['canvas', { 'crewai-only': isCrewAIOnly }]"
      :style="{ transform: canvasTransform }"
    >
      <div class="panel">
        <h2 class="caption">CrewAI vs. LangGraph Comparison</h2>

        <div class="table-wrap">
          <table ref="table" class="compare">
            <thead>
              <tr>
                <th class="corner">Aspect</th>
                <th ref="crewHead" class="head" style="view-transition-name: crew-head">CrewAI</th>
                <th class="head is-langgraph">LangGraph</th>
              </tr>
            </thead>

            <tbody>
              <tr v-for="(row, r) in rows" :key="row.label">
                <th scope="row" class="label">{{ row.label }}</th>

                <td
                  v-for="(cell, i) in [row.crewai, row.langgraph]"
                  :key="i"
                  :class="['cell', { 'is-langgraph': i === 1 }]"
                  :style="i === 0 ? { viewTransitionName: `crew-cell-${r}` } : undefined"
                >
                  <p class="main">{{ cell.text }}</p>
                  <p class="example">
                    <span class="ex-label">Example:</span>
                    <template v-for="(part, j) in cell.example" :key="j">
                      <code v-if="part.code">{{ part.code }}</code>
                      <span v-else>{{ part }}</span>
                    </template>
                  </p>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue'

// Shared between instances: the next slide is mounted hidden (size 0) by Slidev,
// so it reuses the geometry measured by whichever instance is visible.
const sharedScale = ref(0.9)
const sharedShift = ref(0)
</script>

<script setup>
import { computed, onBeforeUnmount, onMounted, watch } from 'vue'

const props = defineProps({
  escala: {
    type: Number,
    default: 1,
  },
  // 'full' shows the whole table; 'crewai' keeps only the CrewAI column, moved to the left
  variant: {
    type: String,
    default: 'full',
  },
})

// Left margin (px) of the CrewAI column in the 'crewai' variant
const LEFT_MARGIN = 48

const isCrewAIOnly = computed(() => props.variant === 'crewai')

const stage = ref(null)
const canvas = ref(null)
const table = ref(null)
const crewHead = ref(null)
let observer

const canvasTransform = computed(() => {
  const shift = isCrewAIOnly.value ? sharedShift.value : 0
  return `translate(calc(-50% + ${shift}px), -50%) scale(${sharedScale.value})`
})

function updateScale() {
  if (!stage.value || !canvas.value) return
  const width = stage.value.clientWidth
  const height = stage.value.clientHeight
  const canvasWidth = canvas.value.offsetWidth
  const canvasHeight = canvas.value.offsetHeight
  if (!width || !height || !canvasWidth || !canvasHeight) return
  const scale = Math.min(width / canvasWidth, height / canvasHeight) * 0.96 * props.escala
  // Where the CrewAI column starts on screen, and how far to move it to reach the left margin
  const colX = table.value.offsetLeft + crewHead.value.offsetLeft
  const colScreenX = width / 2 - (canvasWidth * scale) / 2 + colX * scale
  sharedScale.value = scale
  sharedShift.value = LEFT_MARGIN - colScreenX
}

watch(() => props.escala, updateScale)

onMounted(() => {
  updateScale()
  observer = new ResizeObserver(updateScale)
  observer.observe(stage.value)
  observer.observe(canvas.value)
})

onBeforeUnmount(() => {
  observer?.disconnect()
})

const rows = [
  {
    label: 'Approach',
    crewai: {
      text: 'Collaboration between multiple agents',
      example: ['specialized agents working together'],
    },
    langgraph: {
      text: 'Flexible multi-agent workflows',
      example: ['complex and dynamic workflows'],
    },
  },
  {
    label: 'Execution',
    crewai: {
      text: 'Parallel task execution',
      example: [{ code: 'async_execution=True' }],
    },
    langgraph: {
      text: 'Graph-based execution',
      example: ['parallel branches using edges (fan-out)'],
    },
  },
  {
    label: 'State Management',
    crewai: {
      text: 'Shared context across the team',
      example: [{ code: 'context=[...]' }],
    },
    langgraph: {
      text: 'Explicit and persistent state management',
      example: [{ code: 'TypedDict' }, ' and persistent state'],
    },
  },
  {
    label: 'Human Intervention',
    crewai: {
      text: 'Possible through structured interactions',
      example: [{ code: 'human_input=True' }],
    },
    langgraph: {
      text: 'Checkpoints for human review',
      example: [{ code: 'interrupt()' }, ' and ', { code: 'checkpointer' }],
    },
  },
  {
    label: 'Ideal Use Case',
    crewai: {
      text: 'Agent teams with specialized roles',
      example: ['design based on ', { code: 'Agent' }, ' and ', { code: 'Task' }],
    },
    langgraph: {
      text: 'Complex and dynamic workflows',
      example: [
        { code: 'StateGraph' },
        ', nodes, edges, loops, and execution resumption',
      ],
    },
  },
]
</script>

<style scoped>
.stage {
  position: relative;
  width: 100%;
  height: 100%;
  overflow: hidden;
  box-sizing: border-box;
  background: #fbfaf7;
  font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif;
  color: #7a7872;
}

.canvas {
  position: absolute;
  left: 50%;
  top: 50%;
  width: 940px;
  transform-origin: center;
}

.panel {
  background: #ecebe6;
  border-radius: 24px;
  padding: 28px 32px 32px;
  box-sizing: border-box;
}

/* CrewAI-only variant: same layout, everything but the CrewAI column is hidden */
.crewai-only .panel {
  background: transparent;
}

.crewai-only .caption,
.crewai-only .corner,
.crewai-only .label,
.crewai-only .is-langgraph {
  visibility: hidden;
}

.caption {
  margin: 0 0 20px;
  text-align: center;
  font-size: 26px;
  font-weight: 600;
  line-height: 1.1;
  color: #7a7872;
}

.table-wrap {
  overflow-x: auto;
}

.compare {
  width: 100%;
  border-collapse: separate;
  border-spacing: 20px 6px;
  table-layout: fixed;
}

.compare th,
.compare td {
  vertical-align: top;
  text-align: left;
}

/* Headers */
.corner {
  padding: 0 10px;
  font-size: 16px;
  font-weight: 700;
  color: #7a7872;
  text-align: left;
}

.head {
  background: #b8d3f1;
  color: #3b4d68;
  font-size: 16px;
  font-weight: 700;
  text-align: center !important;
  padding: 10px 14px;
  border-radius: 6px;
}

/* Row labels */
.label {
  padding: 12px 10px;
  font-size: 16px;
  font-weight: 700;
  color: #7a7872;
  width: 170px;
  text-align: center;
  vertical-align: middle;
}

/* Cells */
.cell {
  padding: 12px 16px;
  background: #e4e2dc;
  border-radius: 6px;
}

.main {
  margin: 0 0 6px;
  font-size: 15px;
  font-weight: 600;
  line-height: 1.25;
  color: #6f6d67;
}

.example {
  margin: 0;
  font-size: 13px;
  line-height: 1.4;
  color: #8f8c85;
}

.ex-label {
  font-style: italic;
  margin-right: 4px;
}

code {
  font-family: 'SFMono-Regular', Menlo, Consolas, monospace;
  font-size: 12px;
  background: #d5e4f7;
  color: #3b4d68;
  padding: 1px 6px;
  border-radius: 4px;
}
</style>

<style>
/* View transition between the full table and the CrewAI-only slide */
::view-transition-group(*) {
  animation-duration: 700ms;
  animation-timing-function: cubic-bezier(0.65, 0, 0.35, 1);
}

::view-transition-old(root) {
  animation-duration: 350ms;
}
</style>
