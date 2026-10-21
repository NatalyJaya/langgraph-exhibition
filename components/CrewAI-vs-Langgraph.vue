<template>
  <div ref="stage" class="stage">
    <div ref="canvas" class="canvas" :style="{ transform: `translate(-50%, -50%) scale(${scale})` }">
      <div class="panel">
        <h2 class="caption">Comparativa entre CrewAI i LangGraph</h2>

        <div class="table-wrap">
          <table class="compare">
            <thead>
              <tr>
                <th class="corner">Aspecte</th>
                <th class="head">CrewAI</th>
                <th class="head">LangGraph</th>
              </tr>
            </thead>

            <tbody>
              <tr v-for="row in rows" :key="row.label">
                <th scope="row" class="label">{{ row.label }}</th>

                <td class="cell" v-for="(cell, i) in [row.crewai, row.langgraph]" :key="i">
                  <p class="main">{{ cell.text }}</p>
                  <p class="example">
                    <span class="ex-label">Exemple:</span>
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

<script setup>
import { onBeforeUnmount, onMounted, ref, watch } from 'vue'

const props = defineProps({
  escala: {
    type: Number,
    default: 1,
  },
})

const stage = ref(null)
const canvas = ref(null)
const scale = ref(0.9)
let observer

function updateScale() {
  if (!stage.value || !canvas.value) return
  const width = stage.value.clientWidth
  const height = stage.value.clientHeight
  const canvasWidth = canvas.value.offsetWidth
  const canvasHeight = canvas.value.offsetHeight
  if (!width || !height || !canvasWidth || !canvasHeight) return
  scale.value = Math.min(width / canvasWidth, height / canvasHeight) * 0.96 * props.escala
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
    label: 'Enfocament',
    crewai: {
      text: 'Col·laboració entre múltiples agents',
      example: ['agents especialitzats que treballen conjuntament'],
    },
    langgraph: {
      text: 'Fluxos de treball multiagent flexibles',
      example: ['fluxos complexos i dinàmics'],
    },
  },
  {
    label: 'Execució',
    crewai: {
      text: 'Execució paral·lela de tasques',
      example: [{ code: 'async_execution=True' }],
    },
    langgraph: {
      text: 'Execució basada en grafs',
      example: ['branques paral·leles mitjançant arestes (fan-out)'],
    },
  },
  {
    label: "Gestió de l'estat",
    crewai: {
      text: 'Context compartit per l\'equip',
      example: [{ code: 'context=[...]' }],
    },
    langgraph: {
      text: 'Gestió explícita i persistent de l\'estat',
      example: [{ code: 'TypedDict' }, ' i estat persistent'],
    },
  },
  {
    label: 'Intervenció humana',
    crewai: {
      text: 'Possible mitjançant interaccions estructurades',
      example: [{ code: 'human_input=True' }],
    },
    langgraph: {
      text: 'Punts de control per a la revisió humana',
      example: [{ code: 'interrupt()' }, ' i ', 'checkpointer'],
    },
  },
  {
    label: 'Cas d\'ús ideal',
    crewai: {
      text: 'Equips d\'agents amb rols especialitzats',
      example: ['disseny basat en ', { code: 'Agent' }, ' i ', { code: 'Task' }],
    },
    langgraph: {
      text: 'Fluxos complexos i dinàmics',
      example: [
        { code: 'StateGraph' },
        ', nodes, arestes, bucles i represa de l\'execució',
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
  border-spacing: 0 8px;
  table-layout: fixed;
}

.compare th,
.compare td {
  vertical-align: top;
  text-align: left;
}

/* Cabeceras */
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

/* Etiquetas de fila */
.label {
  padding: 12px 10px;
  font-size: 16px;
  font-weight: 700;
  color: #7a7872;
  width: 170px;
}

/* Celdas */
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