<template>
  <div ref="stage" class="stage">
    <div
      ref="canvas"
      :class="['canvas', { 'crewai-only': isCrewAIOnly, 'langgraph-only': isLangGraphOnly }]"
      :style="{ transform: canvasTransform }"
    >
      <div class="panel">
        <h2 class="caption">CrewAI vs. LangGraph Comparison</h2>

        <div class="table-wrap">
          <table ref="table" class="compare">
            <thead>
              <tr>
                <th class="corner">Aspect</th>
                <th ref="crewHead" class="head is-crewai" :style="vtName(0, 'head')">CrewAI</th>
                <th ref="langHead" class="head is-langgraph" :style="vtName(1, 'head')">LangGraph</th>
              </tr>
            </thead>

            <tbody>
              <tr v-for="(row, r) in rows" :key="row.label">
                <th scope="row" class="label">{{ row.label }}</th>

                <td
                  v-for="(cell, i) in [row.crewai, row.langgraph]"
                  :key="i"
                  :class="['cell', i === 0 ? 'is-crewai' : 'is-langgraph']"
                  :style="vtName(i, `cell-${r}`)"
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

    <!-- Content shown next to the remaining column (right of CrewAI / left of LangGraph) -->
    <div
      v-if="(isCrewAIOnly || isLangGraphOnly) && $slots.default"
      :class="['side', isLangGraphOnly ? 'side--left' : 'side--right']"
      :style="sideStyle"
    >
      <slot />
    </div>
  </div>
</template>

<script>
import { ref } from 'vue'

// Shared between instances: the next slide is mounted hidden (size 0) by Slidev,
// so it reuses the geometry measured by whichever instance is visible.
const sharedScale = ref(0.9)
const sharedShift = ref(0)
const sharedShiftLang = ref(0)
const sharedColWidth = ref(0)
const sharedSideTop = ref(0)
const sharedSideHeight = ref(0)
</script>

<script setup>
import { computed, onBeforeUnmount, onMounted, watch } from 'vue'

const props = defineProps({
  escala: {
    type: Number,
    default: 1,
  },
  // 'full' shows the whole table; 'crewai' keeps only the CrewAI column, moved to the left;
  // 'langgraph' keeps only the LangGraph column, moved to the right
  variant: {
    type: String,
    default: 'full',
  },
})

// Distance (px) from the slide edge to the remaining column in the single-column variants
const LEFT_MARGIN = 48
const RIGHT_MARGIN = 48
// Space (px) between the CrewAI column and the side content
const SIDE_GAP = 32

const isCrewAIOnly = computed(() => props.variant === 'crewai')
const isLangGraphOnly = computed(() => props.variant === 'langgraph')

// view-transition names pair the same column across slides so it slides instead of fading.
// A column hidden in this variant gets no name, so it just fades out/in in place.
const COLUMN_KEYS = ['crew', 'lang']
function vtName(col, part) {
  const hidden = col === 0 ? isLangGraphOnly.value : isCrewAIOnly.value
  return hidden ? undefined : { viewTransitionName: `${COLUMN_KEYS[col]}-${part}` }
}

const stage = ref(null)
const canvas = ref(null)
const table = ref(null)
const crewHead = ref(null)
const langHead = ref(null)
let observer

const sideStyle = computed(() => {
  const columnSpace = sharedColWidth.value * sharedScale.value + SIDE_GAP
  return {
    top: `${sharedSideTop.value}px`,
    height: `${sharedSideHeight.value}px`,
    ...(isLangGraphOnly.value
      ? { right: `${RIGHT_MARGIN + columnSpace}px` }
      : { left: `${LEFT_MARGIN + columnSpace}px` }),
  }
})

const canvasTransform = computed(() => {
  const shift = isCrewAIOnly.value ? sharedShift.value : isLangGraphOnly.value ? sharedShiftLang.value : 0
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
  // Same for the LangGraph column, aligning its right edge to the right margin
  const langX = table.value.offsetLeft + langHead.value.offsetLeft + langHead.value.offsetWidth
  const langScreenX = width / 2 - (canvasWidth * scale) / 2 + langX * scale
  sharedShiftLang.value = width - RIGHT_MARGIN - langScreenX
  sharedColWidth.value = crewHead.value.offsetWidth

  // Vertical bounds of the CrewAI column, in unscaled stage coordinates
  // (rects include Slidev's outer scale, so normalize before using them in CSS)
  const stageRect = stage.value.getBoundingClientRect()
  const tableRect = table.value.getBoundingClientRect()
  const outerScale = stageRect.width / width
  if (tableRect.height && outerScale) {
    sharedSideTop.value = (tableRect.top - stageRect.top) / outerScale
    sharedSideHeight.value = tableRect.height / outerScale
  }
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

/* Single-column variants: same layout, everything but the remaining column is hidden */
.crewai-only .panel,
.langgraph-only .panel {
  background: transparent;
}

.crewai-only .caption,
.crewai-only .corner,
.crewai-only .label,
.crewai-only .is-langgraph,
.langgraph-only .caption,
.langgraph-only .corner,
.langgraph-only .label,
.langgraph-only .is-crewai {
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

/* Side content: enters from the right once the CrewAI column has moved */
.side {
  position: absolute;
  display: flex;
  align-items: stretch;
  animation: side-in 600ms cubic-bezier(0.22, 1, 0.36, 1) 550ms both;
}

.side--right {
  right: 24px;
  --side-from: 48px;
}

.side--left {
  left: 24px;
  --side-from: -48px;
}

.side > :deep(*) {
  width: 100%;
  height: 100%;
}

@keyframes side-in {
  from {
    opacity: 0;
    transform: translateX(var(--side-from));
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}

@media (prefers-reduced-motion: reduce) {
  .side {
    animation: none;
  }
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
