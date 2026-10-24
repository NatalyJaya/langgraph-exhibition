<script setup lang="ts">
import { computed } from 'vue'

const props = defineProps<{
  steps: string[]
  click?: number
}>()

const ORDER = [
  'b-state',
  'b-nodes',
  'b-edges',
  'b-compile',
  'b-args',
  'checkpointer',
  'serde-collect',
  'serde',
  'serde-apply',
  'interrupts',
  'validate',
  'output-ch',
  'stream-ch',
  'h-read',
  'h-default',
  'h-loop',
  'h-error',
  'h-retry',
  'h-cache',
  'h-timeout',
  'errormap',
  'instance',
  'instance-cfg',
  'start',
  'nodes',
  'mappers',
  'edges',
  'waiting',
  'branches',
  'done',
] as const

const CAPTIONS: Record<string, string> = {
  'b-state': 'State schema: every key becomes a channel',
  'b-nodes': 'add_node(): three agents stored as node specs',
  'b-edges': 'Edges are stored as plain tuples. Nothing runs yet',
  'b-compile': 'compile() turns this blueprint into an executable graph',
  'b-args': 'Our call: a checkpointer + pause before "writer"',
  'checkpointer': 'ensure_valid_checkpointer(): InMemorySaver accepted',
  'serde-collect': 'Strict msgpack: gather every schema the graph uses',
  'serde': 'Build an allowlist of types that are safe to deserialize',
  'serde-apply': 'The checkpointer will only restore allow-listed types',
  'interrupts': 'None → []: interrupt lists get their defaults',
  'validate': 'validate(): nodes, edges and interrupt names all exist',
  'output-ch': 'Output channels = State keys that are not managed values',
  'stream-ch': 'Stream channels = every non-managed channel',
  'h-read': 'Read the builder-level node defaults',
  'h-default': 'Optional: auto-add a default error-handler node',
  'h-loop': 'Walk every node spec. Per-node values always win',
  'h-error': 'Regular nodes route their failures to the handler',
  'h-retry': 'Retry default → every node, the handler included',
  'h-cache': 'Cache default → regular nodes only',
  'h-timeout': 'Timeout default → every node',
  'errormap': 'node_error_handler_map: which node falls back where',
  'instance': 'CompiledStateGraph (Pregel) is created with nodes={}',
  'instance-cfg': 'Checkpointer, interrupts and stream config are wired in',
  'start': 'attach_node(START): entry point fed by the __start__ channel',
  'nodes': 'Each agent becomes a PregelNode subscribed to channels',
  'mappers': 'Mappers turn raw channel values back into the State type',
  'edges': 'attach_edge(): each edge becomes a channel trigger',
  'waiting': 'waiting_edges: many → one joins (none in our graph)',
  'branches': 'attach_branch(): route() picks the next agent at runtime',
  'done': 'compiled.validate() → a runnable graph: invoke · stream',
}

const current = computed(() => {
  const last = props.steps.length - 1
  const i = Math.min(Math.max(props.click ?? 0, 0), last)
  return props.steps[i]
})
const idx = computed(() => ORDER.indexOf(current.value as (typeof ORDER)[number]))
const pos = (s: string) => ORDER.indexOf(s as (typeof ORDER)[number])
const at = (s: string) => idx.value >= pos(s)
const hl = (...s: string[]) => s.includes(current.value)

const W = 150
const H = 44
const agents = [
  { id: 'supervisor', x: 225, y: 118, accent: 'blue' },
  { id: 'researcher', x: 90, y: 230, accent: 'amber' },
  { id: 'writer', x: 360, y: 230, accent: 'rose' },
]

const plainEdges = [
  'M300 96 V114',
  'M140 230 C140 172, 180 140, 221 140',
  'M460 230 C460 172, 420 140, 379 140',
]
const condEdges = [
  'M255 162 C255 200, 200 205, 182 226',
  'M345 162 C345 200, 400 205, 418 226',
  'M300 162 V298',
]
const handlerLinks = [
  'M205 274 C205 372, 300 372, 376 372',
  'M455 274 V348',
  'M379 150 C540 150, 548 372, 534 372',
]

const header = computed(() => {
  if (at('done')) return 'CompiledStateGraph · ready'
  if (at('instance')) return 'CompiledStateGraph · Pregel'
  return 'StateGraph · builder'
})
</script>

<template>
  <section class="cd-shell">
    <svg
      class="cd-svg"
      viewBox="0 0 560 530"
      preserveAspectRatio="xMidYMid meet"
      role="img"
      aria-label="How compile() transforms a supervisor multi-agent graph"
      xmlns="http://www.w3.org/2000/svg"
    >
      <defs>
        <marker id="cd-arrow" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto">
          <path d="M0,0 L6,3 L0,6" fill="none" stroke="#b9b3aa" stroke-width="1.1" stroke-linecap="round" stroke-linejoin="round" />
        </marker>
        <marker id="cd-arrow-live" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto">
          <path d="M0,0 L6,3 L0,6" fill="none" stroke="#5f5c56" stroke-width="1.1" stroke-linecap="round" stroke-linejoin="round" />
        </marker>
        <marker id="cd-arrow-branch" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto">
          <path d="M0,0 L6,3 L0,6" fill="none" stroke="#a07f45" stroke-width="1.1" stroke-linecap="round" stroke-linejoin="round" />
        </marker>
      </defs>

      <!-- HEADER -->
      <g>
        <rect
          x="170" y="12" width="220" height="28" rx="14"
          :class="['header-pill', at('done') ? 'header-done' : at('instance') ? 'header-live' : 'header-draft', { hl: hl('b-compile', 'instance', 'done') }]"
        />
        <text x="280" y="26" class="header-label">{{ header }}</text>
        <text x="548" y="26" :class="['stamp', { hidden: !at('validate') }]">✓ validated</text>
      </g>

      <!-- FRAME -->
      <rect
        x="10" y="50" width="540" height="380" rx="16"
        :class="['frame', at('done') ? 'frame-done' : at('instance') ? 'frame-live' : 'frame-draft']"
      />

      <!-- STATE -->
      <g :class="['fade', { hidden: !at('b-state') }]">
        <rect x="26" y="62" width="140" height="74" rx="10" :class="['card', 'card-state', { hl: hl('b-state', 'serde-collect') }]" />
        <text x="40" y="81" class="card-title">State</text>
        <text x="40" y="102" class="field-key">messages</text>
        <text x="152" y="102" class="field-type">list</text>
        <text x="40" y="121" class="field-key">next</text>
        <text x="152" y="121" class="field-type">str</text>
      </g>

      <!-- SERDE ALLOWLIST (before it is attached to the checkpointer) -->
      <g :class="['fade', { hidden: !at('serde') || at('serde-apply') }]">
        <rect x="26" y="146" width="112" height="22" rx="6" :class="['chip-lock', { hl: hl('serde') }]" />
        <text x="82" y="157" class="chip-lock-label">serde allowlist</text>
      </g>

      <!-- EDGES -->
      <g :class="['fade', { hidden: !at('b-edges') }]">
        <path
          v-for="d in plainEdges" :key="d" :d="d"
          :class="['edge', at('edges') ? 'edge-live' : 'edge-draft', { flow: at('done'), 'edge-hl': hl('edges') }]"
          :marker-end="at('edges') ? 'url(#cd-arrow-live)' : 'url(#cd-arrow)'"
        />
        <path
          v-for="d in condEdges" :key="d" :d="d"
          :class="['edge', at('branches') ? 'edge-branch' : 'edge-draft', { flow: at('done'), 'edge-hl': hl('branches') }]"
          :marker-end="at('branches') ? 'url(#cd-arrow-branch)' : 'url(#cd-arrow)'"
        />
        <rect x="270" y="196" width="60" height="20" rx="6" :class="['route-chip', { 'route-live': at('branches'), hl: hl('serde-collect', 'branches') }]" />
        <text x="300" y="206" class="route-label">route()</text>
      </g>

      <!-- START / END -->
      <g :class="['fade', { hidden: !at('b-edges') }]">
        <rect x="255" y="68" width="90" height="26" rx="13" :class="['terminal', at('start') ? 'terminal-live' : 'terminal-draft', { hl: hl('start') }]" />
        <text x="300" y="81" class="terminal-label">START</text>
        <rect x="260" y="300" width="80" height="26" rx="13" :class="['terminal', at('branches') ? 'terminal-live' : 'terminal-draft']" />
        <text x="300" y="313" class="terminal-label">END</text>
      </g>

      <!-- ERROR HANDLER LINKS -->
      <g :class="['fade', { hidden: !at('errormap') }]">
        <path v-for="d in handlerLinks" :key="d" :d="d" :class="['handler-link', { hl: hl('errormap') }]" />
      </g>

      <!-- AGENT NODES -->
      <g v-for="a in agents" :key="a.id" :class="['fade', { hidden: !at('b-nodes') }]">
        <rect
          :x="a.x" :y="a.y" :width="W" :height="H" rx="10"
          :class="['card', at('nodes') ? `card-live live-${a.accent}` : 'card-draft', { hl: hl('b-nodes', 'serde-collect', 'h-loop', 'nodes'), dim: at('instance') && !at('nodes') }]"
        />
        <text :x="a.x + 14" :y="a.y + 19" class="node-title">{{ a.id }}</text>
        <text :x="a.x + 14" :y="a.y + 34" class="node-sub">{{ at('nodes') ? 'PregelNode' : 'StateNodeSpec' }}</text>
        <text :x="a.x + W - 12" :y="a.y + 24" :class="['check', { hidden: !at('validate') || at('instance') }]">✓</text>

        <!-- policy badges -->
        <g :class="['fade', { hidden: !at('h-error') }]">
          <circle :cx="a.x + W - 70" :cy="a.y" r="8" :class="['badge', { hl: hl('h-error') }]" />
          <text :x="a.x + W - 70" :y="a.y" class="badge-label">E</text>
        </g>
        <g :class="['fade', { hidden: !at('h-retry') }]">
          <circle :cx="a.x + W - 52" :cy="a.y" r="8" :class="['badge', { hl: hl('h-retry') }]" />
          <text :x="a.x + W - 52" :y="a.y" class="badge-label">R</text>
        </g>
        <g :class="['fade', { hidden: !at('h-cache') }]">
          <circle :cx="a.x + W - 34" :cy="a.y" r="8" :class="['badge', { hl: hl('h-cache') }]" />
          <text :x="a.x + W - 34" :y="a.y" class="badge-label">C</text>
        </g>
        <g :class="['fade', { hidden: !at('h-timeout') }]">
          <circle :cx="a.x + W - 16" :cy="a.y" r="8" :class="['badge', { hl: hl('h-timeout') }]" />
          <text :x="a.x + W - 16" :y="a.y" class="badge-label">T</text>
        </g>
      </g>

      <!-- INTERRUPT BEFORE WRITER -->
      <g :class="['fade', { hidden: !at('b-args') }]">
        <rect
          x="348" y="219" width="22" height="22" rx="6"
          :class="['pause', at('interrupts') ? 'pause-live' : 'pause-draft', { hl: hl('b-args', 'interrupts', 'validate', 'instance-cfg') }]"
        />
        <rect x="354.5" y="225" width="3" height="10" rx="1" class="pause-bar" />
        <rect x="360.5" y="225" width="3" height="10" rx="1" class="pause-bar" />
      </g>

      <!-- DEFAULT ERROR HANDLER -->
      <g :class="['fade', { hidden: !at('h-default') }]">
        <rect
          x="380" y="352" width="150" height="40" rx="10"
          :class="['card', at('nodes') ? 'card-live live-violet' : 'card-optional', { hl: hl('h-default', 'h-loop', 'nodes') }]"
        />
        <text x="394" y="369" class="node-title">error handler</text>
        <text x="394" y="383" class="node-sub">{{ at('nodes') ? 'PregelNode' : 'only if a default is set' }}</text>
        <g :class="['fade', { hidden: !at('h-retry') }]">
          <circle cx="478" cy="352" r="8" :class="['badge', { hl: hl('h-retry') }]" />
          <text x="478" y="352" class="badge-label">R</text>
        </g>
        <g :class="['fade', { hidden: !at('h-timeout') }]">
          <circle cx="514" cy="352" r="8" :class="['badge', { hl: hl('h-timeout') }]" />
          <text x="514" y="352" class="badge-label">T</text>
        </g>
      </g>

      <!-- CHECKPOINTER -->
      <g :class="['fade', { hidden: !at('b-args') }]">
        <rect
          x="26" y="352" width="150" height="64" rx="10"
          :class="['card', at('checkpointer') ? 'card-live live-violet' : 'card-draft', { hl: hl('b-args', 'checkpointer', 'serde-apply', 'instance-cfg') }]"
        />
        <text x="40" y="371" class="node-title">InMemorySaver</text>
        <text x="40" y="386" class="node-sub">checkpointer</text>
        <g :class="['fade', { hidden: !at('serde-apply') }]">
          <rect x="40" y="394" width="96" height="16" rx="5" class="chip-lock" />
          <text x="88" y="402" class="chip-lock-label">allowlist applied</text>
        </g>
      </g>

      <!-- CHANNELS -->
      <g :class="['fade', { hidden: !at('b-state') }]">
        <text x="14" y="457" class="row-label">channels</text>
        <g>
          <rect x="78" y="444" width="104" height="26" rx="7" :class="['channel', { hl: hl('b-state', 'output-ch', 'stream-ch') }]" />
          <text x="130" y="457" class="channel-label">messages</text>
          <text x="130" y="482" :class="['channel-tag', { hidden: !at('output-ch') }]">{{ at('stream-ch') ? 'output · stream' : 'output' }}</text>
        </g>
        <g>
          <rect x="192" y="444" width="104" height="26" rx="7" :class="['channel', { hl: hl('b-state', 'output-ch', 'stream-ch') }]" />
          <text x="244" y="457" class="channel-label">next</text>
          <text x="244" y="482" :class="['channel-tag', { hidden: !at('output-ch') }]">{{ at('stream-ch') ? 'output · stream' : 'output' }}</text>
        </g>
        <g :class="['fade', { hidden: !at('instance') }]">
          <rect x="306" y="444" width="104" height="26" rx="7" :class="['channel', 'channel-start', { hl: hl('instance', 'start') }]" />
          <text x="358" y="457" class="channel-label">__start__</text>
          <text x="358" y="482" class="channel-tag">{{ at('instance-cfg') ? 'input · Ephemeral' : 'EphemeralValue' }}</text>
        </g>
        <g :class="['fade', { hidden: !at('mappers') }]">
          <rect x="430" y="444" width="104" height="26" rx="7" :class="['chip-mapper', { hl: hl('mappers') }]" />
          <text x="482" y="457" class="channel-label">mappers</text>
        </g>
      </g>

      <!-- CAPTION -->
      <text x="280" y="514" class="caption">{{ CAPTIONS[current] }}</text>
    </svg>
  </section>
</template>

<style scoped>
.cd-shell {
  box-sizing: border-box;
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 8px 10px;
  background: #fbfaf7;
  border: 1px solid #ecebe6;
  border-radius: 20px;
  font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif;
}

.cd-svg {
  display: block;
  width: 100%;
  height: 100%;
  overflow: visible;
}

.cd-svg * {
  transition: opacity 0.35s ease, fill 0.35s ease, stroke 0.35s ease, stroke-width 0.35s ease;
}

.fade { opacity: 1; }
.hidden { opacity: 0; }
.dim { opacity: 0.55; }

/* Highlight for what changed on this click */
.hl {
  stroke: #146b8c !important;
  stroke-width: 2 !important;
  stroke-dasharray: none !important;
  filter: drop-shadow(0 0 5px rgba(43, 144, 182, 0.45));
}

/* Header */
.header-pill { stroke-width: 1.2; }
.header-draft { fill: #fbfaf7; stroke: #c9c3b9; stroke-dasharray: 4 3; }
.header-live { fill: #dbe8f8; stroke: #b8d3f1; }
.header-done { fill: #e4e9dc; stroke: #b9c6a8; }
.header-label {
  fill: #3f3d38;
  font-family: 'JetBrains Mono', 'SF Mono', Menlo, monospace;
  font-size: 11.5px;
  font-weight: 600;
  text-anchor: middle;
  dominant-baseline: central;
}
.stamp {
  fill: #56614b;
  font-size: 11px;
  font-weight: 600;
  text-anchor: end;
  dominant-baseline: central;
}

/* Frame */
.frame { fill: none; stroke-width: 1.3; }
.frame-draft { stroke: #d8d3cb; stroke-dasharray: 6 5; }
.frame-live { stroke: #b8d3f1; stroke-width: 2; }
.frame-done { stroke: #9fb38a; stroke-width: 2.4; }

/* Cards */
.card { stroke-width: 1.2; }
.card-draft { fill: #fbfaf7; stroke: #c9c3b9; stroke-dasharray: 5 4; }
.card-optional { fill: #fbfaf7; stroke: #c9adb2; stroke-dasharray: 2 3; }
.card-state { fill: #efe6d3; stroke: #d8c6a2; }
.card-live { stroke-dasharray: none; }
.live-blue { fill: #dbe8f8; stroke: #b8d3f1; }
.live-amber { fill: #efe6d3; stroke: #d8c6a2; }
.live-rose { fill: #f5ecea; stroke: #cdb3ab; }
.live-violet { fill: #eddfe1; stroke: #c9adb2; }

.card-title {
  fill: #3f3d38;
  font-size: 13px;
  font-weight: 650;
  dominant-baseline: central;
}
.node-title {
  fill: #3f3d38;
  font-family: 'JetBrains Mono', 'SF Mono', Menlo, monospace;
  font-size: 12px;
  font-weight: 600;
  dominant-baseline: central;
}
.node-sub {
  fill: #8f8c85;
  font-size: 10px;
  dominant-baseline: central;
}
.field-key {
  fill: #3f3d38;
  font-family: 'JetBrains Mono', 'SF Mono', Menlo, monospace;
  font-size: 10.5px;
  dominant-baseline: central;
}
.field-type {
  fill: #8f8c85;
  font-family: 'JetBrains Mono', 'SF Mono', Menlo, monospace;
  font-size: 10.5px;
  text-anchor: end;
  dominant-baseline: central;
}
.check {
  fill: #56614b;
  font-size: 13px;
  font-weight: 700;
  text-anchor: end;
  dominant-baseline: central;
}

/* Edges */
.edge { fill: none; stroke-width: 1.4; stroke-linecap: round; stroke-linejoin: round; }
.edge-draft { stroke: #c9c3b9; stroke-dasharray: 5 4; }
.edge-live { stroke: #5f5c56; }
.edge-branch { stroke: #a07f45; stroke-width: 1.6; }
.edge-hl { stroke-width: 2.2; filter: drop-shadow(0 0 4px rgba(43, 144, 182, 0.45)); }
.flow {
  stroke-dasharray: 7 5;
  animation: cd-flow 1s linear infinite;
}
@keyframes cd-flow {
  to { stroke-dashoffset: -24; }
}

.route-chip { fill: #fbfaf7; stroke: #d8c6a2; stroke-width: 1; stroke-dasharray: 3 3; }
.route-live { fill: #efe6d3; stroke-dasharray: none; }
.route-label {
  fill: #7b6749;
  font-family: 'JetBrains Mono', 'SF Mono', Menlo, monospace;
  font-size: 10px;
  font-weight: 600;
  text-anchor: middle;
  dominant-baseline: central;
}

/* Terminals */
.terminal { stroke-width: 1.2; }
.terminal-draft { fill: #fbfaf7; stroke: #c9c3b9; stroke-dasharray: 4 3; }
.terminal-live { fill: #fbfaf7; stroke: #8f8c85; }
.terminal-label {
  fill: #5f5c56;
  font-size: 10px;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-anchor: middle;
  dominant-baseline: central;
}

/* Error handler */
.handler-link {
  fill: none;
  stroke: #c9adb2;
  stroke-width: 1.3;
  stroke-dasharray: 2 4;
  stroke-linecap: round;
}

/* Policy badges */
.badge { fill: #fbfaf7; stroke: #a9a49b; stroke-width: 1; stroke-dasharray: 2 2; }
.badge-label {
  fill: #5f5c56;
  font-size: 8.5px;
  font-weight: 700;
  text-anchor: middle;
  dominant-baseline: central;
}

/* Interrupt */
.pause { stroke-width: 1.2; }
.pause-draft { fill: #fbfaf7; stroke: #cdb3ab; stroke-dasharray: 3 2; }
.pause-live { fill: #f5ecea; stroke: #cdb3ab; }
.pause-bar { fill: #865757; }

/* Serde */
.chip-lock { fill: #eddfe1; stroke: #c9adb2; stroke-width: 1; }
.chip-lock-label {
  fill: #7a5560;
  font-size: 9px;
  font-weight: 600;
  text-anchor: middle;
  dominant-baseline: central;
}

/* Channels */
.row-label {
  fill: #8f8c85;
  font-size: 10px;
  font-weight: 700;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  dominant-baseline: central;
}
.channel { fill: #ecebe6; stroke: #e0dcd4; stroke-width: 1; }
.channel-start { fill: #dbe8f8; stroke: #b8d3f1; }
.chip-mapper { fill: #e4e9dc; stroke: #b9c6a8; stroke-width: 1; }
.channel-label {
  fill: #3f3d38;
  font-family: 'JetBrains Mono', 'SF Mono', Menlo, monospace;
  font-size: 10.5px;
  font-weight: 600;
  text-anchor: middle;
  dominant-baseline: central;
}
.channel-tag {
  fill: #8f8c85;
  font-size: 9.5px;
  text-anchor: middle;
  dominant-baseline: central;
}

/* Caption */
.caption {
  fill: #3f3d38;
  font-size: 13.5px;
  font-weight: 600;
  text-anchor: middle;
  dominant-baseline: central;
}
</style>
