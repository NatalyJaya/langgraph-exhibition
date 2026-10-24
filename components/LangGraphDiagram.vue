
<template>
  <section class="diagram-shell">
    <div class="diagram-canvas">
      <svg
        class="diagram-svg"
        viewBox="0 0 940 850"
        preserveAspectRatio="xMidYMid meet"
        role="img"
        aria-labelledby="diagram-title diagram-description"
        xmlns="http://www.w3.org/2000/svg"
      >
        <title id="diagram-title">LangGraph Human-in-the-loop Flow</title>
        <desc id="diagram-description">
          The graph researches a topic, writes a draft, and pauses for human review.
          A decision node either sends the draft back for revision or publishes it.
          State is a TypedDict persisted by a MemorySaver checkpointer.
        </desc>

        <defs>
          <marker
            id="arrow"
            markerWidth="8"
            markerHeight="8"
            refX="6"
            refY="3"
            orient="auto"
          >
            <path
              d="M0,0 L6,3 L0,6"
              fill="none"
              stroke="#aca69d"
              stroke-width="1.1"
              stroke-linecap="round"
              stroke-linejoin="round"
            />
          </marker>

          <clipPath id="clip-state">
            <rect x="60" y="60" width="320" height="232" rx="12" />
          </clipPath>
          <clipPath id="clip-persist">
            <rect x="60" y="330" width="320" height="160" rx="12" />
          </clipPath>
          <clipPath id="clip-research">
            <rect x="580" y="90" width="240" height="64" rx="12" />
          </clipPath>
          <clipPath id="clip-write">
            <rect x="580" y="180" width="240" height="64" rx="12" />
          </clipPath>
          <clipPath id="clip-review">
            <rect x="580" y="326" width="240" height="64" rx="12" />
          </clipPath>
        </defs>

        <!-- STATE -->
        <g class="node">
          <rect x="60" y="60" width="320" height="232" rx="12" class="agent-card" />
          <rect x="60" y="60" width="4" height="232" class="accent-amber" clip-path="url(#clip-state)" />

          <circle cx="88" cy="91" r="13" class="icon-bg-amber" />
          <path
            d="M85 85c-3 0-3 2-3 4s-1 2-2 2c1 0 2 0 2 2s0 4 3 4 M91 85c3 0 3 2 3 4s1 2 2 2c-1 0-2 0-2 2s0 4-3 4"
            class="icon-stroke-amber"
          />

          <text x="112" y="96" class="role-title">State</text>
          <rect x="294" y="80" width="66" height="22" rx="5" class="task-pill-amber" />
          <text x="327" y="91" class="task-label task-label-amber">TYPEDDICT</text>

          <text x="84" y="131" class="description">Shared state passed between nodes.</text>

          <rect x="84" y="148" width="272" height="26" rx="6" class="field-row" />
          <text x="96" y="161" class="field-key">topic</text>
          <text x="344" y="161" class="field-type">str</text>

          <rect x="84" y="180" width="272" height="26" rx="6" class="field-row" />
          <text x="96" y="193" class="field-key">findings</text>
          <text x="344" y="193" class="field-type">List[str]</text>

          <rect x="84" y="212" width="272" height="26" rx="6" class="field-row" />
          <text x="96" y="225" class="field-key">draft</text>
          <text x="344" y="225" class="field-type">str</text>

          <rect x="84" y="244" width="272" height="26" rx="6" class="field-row" />
          <text x="96" y="257" class="field-key">approved</text>
          <text x="344" y="257" class="field-type">bool</text>
        </g>

        <!-- PERSISTENCE LAYER -->
        <g class="node">
          <rect x="60" y="330" width="320" height="160" rx="12" class="agent-card" />
          <rect x="60" y="330" width="4" height="160" class="accent-violet" clip-path="url(#clip-persist)" />

          <circle cx="88" cy="361" r="13" class="icon-bg-violet" />
          <ellipse cx="88" cy="356" rx="6" ry="2.5" class="icon-stroke-violet" />
          <path
            d="M82 356v10c0 1.4 2.7 2.5 6 2.5s6-1.1 6-2.5v-10 M82 361c0 1.4 2.7 2.5 6 2.5s6-1.1 6-2.5"
            class="icon-stroke-violet"
          />

          <text x="112" y="366" class="role-title">Persistence Layer</text>
          <text x="84" y="401" class="description">Stores state for each execution thread.</text>

          <rect x="84" y="418" width="272" height="26" rx="6" class="field-row" />
          <text x="96" y="431" class="field-key">MemorySaver</text>
          <text x="344" y="431" class="field-type">checkpointer</text>

          <rect x="84" y="450" width="272" height="26" rx="6" class="field-row" />
          <text x="96" y="463" class="field-key">thread_id</text>
          <text x="344" y="463" class="field-type">solar-article-1</text>
        </g>

        <!-- FLOW CONNECTORS -->
        <g class="connector">
          <path d="M700 62 V87" />
          <path d="M700 154 V177" />
          <path d="M700 244 V323" />
          <path d="M700 390 V409" />
          <path d="M700 448 V487" />
          <path d="M700 630 V697" />
          <path d="M700 764 V787" />
          <path d="M770 560 H880 V212 H823" />
        </g>

        <!-- EDGE LABELS -->
        <g class="edge-label">
          <rect x="684" y="652" width="32" height="22" rx="6" />
          <text x="700" y="663">yes</text>

          <rect x="864" y="379" width="32" height="22" rx="6" />
          <text x="880" y="390">no</text>
        </g>

        <!-- START -->
        <g class="node">
          <rect x="650" y="30" width="100" height="32" rx="16" class="terminal" />
          <text x="700" y="46" class="terminal-label">START</text>
        </g>

        <!-- RESEARCH -->
        <g class="node">
          <rect x="580" y="90" width="240" height="64" rx="12" class="agent-card" />
          <rect x="580" y="90" width="4" height="64" class="accent-blue" clip-path="url(#clip-research)" />

          <circle cx="606" cy="122" r="13" class="icon-bg-blue" />
          <circle cx="604.5" cy="120.5" r="4" class="icon-stroke-blue" />
          <path d="M607.5 123.5l4 4" class="icon-stroke-blue" />

          <text x="630" y="117" class="node-title">research(state)</text>
          <text x="630" y="136" class="description">Collects research findings.</text>
        </g>

        <!-- WRITE DRAFT -->
        <g class="node">
          <rect x="580" y="180" width="240" height="64" rx="12" class="agent-card" />
          <rect x="580" y="180" width="4" height="64" class="accent-amber" clip-path="url(#clip-write)" />

          <circle cx="606" cy="212" r="13" class="icon-bg-amber" />
          <path d="M601 217l2-5 7-7 3 3-7 7z" class="icon-stroke-amber" />

          <text x="630" y="207" class="node-title">write_draft(state)</text>
          <text x="630" y="226" class="description">Writes the draft.</text>
        </g>

        <!-- HUMAN-IN-THE-LOOP -->
        <g class="node">
          <rect x="560" y="290" width="280" height="180" rx="12" class="human-card" />
          <circle cx="580" cy="308" r="3.5" class="human-dot" />
          <text x="590" y="312" class="human-title">Human-in-the-loop</text>

          <!-- HUMAN REVIEW -->
          <rect x="580" y="326" width="240" height="64" rx="12" class="agent-card" />
          <rect x="580" y="326" width="4" height="64" class="accent-rose" clip-path="url(#clip-review)" />

          <circle cx="606" cy="358" r="13" class="icon-bg-rose" />
          <circle cx="606" cy="354" r="3" class="icon-stroke-rose" />
          <path d="M600 364c1-3 3.5-4.5 6-4.5s5 1.5 6 4.5" class="icon-stroke-rose" />

          <text x="630" y="353" class="node-title">review_human(state)</text>
          <text x="630" y="372" class="description">Human review of the draft.</text>

          <!-- INTERRUPT -->
          <rect x="630" y="412" width="140" height="36" rx="8" class="interrupt-card" />
          <text x="700" y="430" class="interrupt-label">interrupt()</text>
        </g>

        <!-- DECISION -->
        <g class="node">
          <path d="M700 490 L770 560 L700 630 L630 560 Z" class="decision" />
          <text x="700" y="560" class="decision-label">choose_next</text>
        </g>

        <!-- PUBLISH -->
        <g class="node">
          <rect x="580" y="700" width="240" height="64" rx="12" class="output-card" />

          <circle cx="606" cy="732" r="13" class="icon-bg-green" />
          <path d="M600 732l4 4 8-9" class="icon-stroke-green" />

          <text x="630" y="727" class="node-title output-title">publish(state)</text>
          <text x="630" y="746" class="output-description">Publishes the article.</text>
        </g>

        <!-- END -->
        <g class="node">
          <rect x="650" y="790" width="100" height="32" rx="16" class="terminal" />
          <text x="700" y="806" class="terminal-label">END</text>
        </g>
      </svg>
    </div>
  </section>
</template>

<style scoped>
.diagram-shell {
  --text: #5f5c56;
  --muted: #8f8c85;
  --border: #e6e2da;
  --surface: #fbfaf7;
  --canvas: #fbfaf7;

  box-sizing: border-box;
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  overflow: hidden;
  color: var(--text);
  background: var(--surface);
  border: 1px solid #ecebe6;
  border-radius: 24px;
  font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif;
}

.diagram-canvas {
  flex: 1 1 auto;
  min-height: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 10px 14px;
  box-sizing: border-box;
  background-color: var(--canvas);
}

.diagram-svg {
  display: block;
  width: 100%;
  height: 100%;
  max-height: 100%;
  overflow: hidden;
}

/* Connectors */
.connector {
  fill: none;
  stroke: #aca69d;
  stroke-width: 1.4;
  stroke-linecap: round;
  stroke-linejoin: round;
  marker-end: url(#arrow);
}

.edge-label rect {
  fill: #fbfaf7;
  stroke: #e6e2da;
  stroke-width: 1;
}

.edge-label text {
  fill: #8f8c85;
  font-size: 11px;
  font-weight: 500;
  text-anchor: middle;
  dominant-baseline: central;
}

/* Cards */
.agent-card {
  fill: #ecebe6;
  stroke: none;
}

.output-card {
  fill: #dde3d5;
  stroke: none;
}

.human-card {
  fill: #fbfaf7;
  stroke: #cdb3ab;
  stroke-width: 1.2;
  stroke-dasharray: 5 4;
}

.terminal {
  fill: #fbfaf7;
  stroke: #d8d3cb;
  stroke-width: 1.2;
}

.terminal-label {
  fill: #5f5c56;
  font-size: 10px;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-anchor: middle;
  dominant-baseline: central;
}

.interrupt-card {
  fill: #f5ecea;
  stroke: #cdb3ab;
  stroke-width: 1;
}

.interrupt-label {
  fill: #865757;
  font-family: 'SF Mono', Menlo, Consolas, monospace;
  font-size: 12px;
  font-weight: 600;
  text-anchor: middle;
  dominant-baseline: central;
}

.decision {
  fill: #efe6d3;
  stroke: #d8c6a2;
  stroke-width: 1.2;
  stroke-linejoin: round;
}

.decision-label {
  fill: #7b6749;
  font-family: 'SF Mono', Menlo, Consolas, monospace;
  font-size: 10.5px;
  font-weight: 600;
  text-anchor: middle;
  dominant-baseline: central;
}

/* State and persistence fields */
.field-row {
  fill: #fbfaf7;
}

.field-key {
  fill: #3f3d38;
  font-family: 'SF Mono', Menlo, Consolas, monospace;
  font-size: 11px;
  font-weight: 600;
  dominant-baseline: central;
}

.field-type {
  fill: #8f8c85;
  font-family: 'SF Mono', Menlo, Consolas, monospace;
  font-size: 11px;
  text-anchor: end;
  dominant-baseline: central;
}

/* Accent colors */
.accent-blue { fill: #b8d3f1; }
.accent-violet { fill: #c9adb2; }
.accent-amber { fill: #d8c6a2; }
.accent-rose { fill: #cdb3ab; }

/* Icon backgrounds */
.icon-bg-blue { fill: #dbe8f8; }
.icon-bg-violet { fill: #eddfe1; }
.icon-bg-amber { fill: #efe6d3; }
.icon-bg-rose { fill: #f5ecea; }
.icon-bg-green { fill: #e4e9dc; }

/* Icon strokes */
.icon-stroke-blue,
.icon-stroke-violet,
.icon-stroke-amber,
.icon-stroke-rose,
.icon-stroke-green {
  fill: none;
  stroke-width: 1.5;
  stroke-linecap: round;
  stroke-linejoin: round;
}

.icon-stroke-blue { stroke: #3b4d68; }
.icon-stroke-violet { stroke: #7a5560; }
.icon-stroke-amber { stroke: #7b6749; }
.icon-stroke-rose { stroke: #865757; }
.icon-stroke-green { stroke: #56614b; }

/* Typography */
.role-title {
  fill: #3f3d38;
  font-size: 15px;
  font-weight: 650;
}

.node-title {
  fill: #3f3d38;
  font-family: 'SF Mono', Menlo, Consolas, monospace;
  font-size: 13px;
  font-weight: 600;
}

.description {
  fill: #8f8c85;
  font-size: 11.5px;
  font-weight: 400;
}

/* Task pills */
.task-pill-amber { fill: #efe6d3; }

.task-label {
  font-size: 8px;
  font-weight: 700;
  text-anchor: middle;
  dominant-baseline: central;
  letter-spacing: 0.07em;
}

.task-label-amber { fill: #7b6749; }

/* Human-in-the-loop */
.human-dot {
  fill: #865757;
}

.human-title {
  fill: #865757;
  font-size: 12px;
  font-weight: 650;
}

/* Output */
.output-title {
  fill: #56614b;
}

.output-description {
  fill: #6b7462;
  font-size: 11px;
}
</style>
