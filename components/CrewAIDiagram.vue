<template>
  <section class="diagram-shell">
    <div class="diagram-canvas">
      <svg
        class="diagram-svg"
        viewBox="0 0 1000 620"
        preserveAspectRatio="xMidYMid meet"
        role="img"
        aria-labelledby="diagram-title diagram-description"
        xmlns="http://www.w3.org/2000/svg"
      >
        <title id="diagram-title">CrewAI Sequential Process</title>
        <desc id="diagram-description">
          Technology and market researchers provide context to a writer.
          Human input can guide the writing task, which produces the final output.
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

          <!-- Clips so the accent bars follow the rounded corners of each card -->
          <clipPath id="clip-tech">
            <rect x="80" y="60" width="340" height="144" rx="12" />
          </clipPath>
          <clipPath id="clip-market">
            <rect x="580" y="60" width="340" height="144" rx="12" />
          </clipPath>
          <clipPath id="clip-writer">
            <rect x="330" y="295" width="340" height="144" rx="12" />
          </clipPath>
        </defs>

        <!-- Connections (all end 3px before the target box) -->
        <g class="connector">
          <path d="M250 204 V250 H440 V292" />
          <path d="M750 204 V250 H560 V292" />
          <path d="M750 367 H673" />
          <path d="M500 439 V497" />
        </g>

        <!-- Context labels (centered on the horizontal segments) -->
        <g class="edge-label">
          <rect x="306" y="239" width="78" height="22" rx="6" />
          <text x="345" y="250">Context</text>

          <rect x="616" y="239" width="78" height="22" rx="6" />
          <text x="655" y="250">Context</text>
        </g>

        <!-- Agent 1: Technology Researcher -->
        <g class="node">
          <rect x="80" y="60" width="340" height="144" rx="12" class="agent-card" />
          <rect x="80" y="60" width="4" height="144" class="accent-blue" clip-path="url(#clip-tech)" />

          <circle cx="108" cy="91" r="13" class="icon-bg-blue" />
          <path d="M102 91h12 M108 85v12" class="icon-stroke-blue" />

          <text x="132" y="96" class="role-title">Technology Researcher</text>
          <text x="104" y="131" class="description">Investigates technical aspects</text>
          <text x="104" y="150" class="description">and relevant technologies.</text>

          <rect x="104" y="167" width="116" height="22" rx="5" class="task-pill-blue" />
          <text x="162" y="178" class="task-label task-label-blue">RESEARCH TASK</text>
        </g>

        <!-- Agent 2: Market Researcher -->
        <g class="node">
          <rect x="580" y="60" width="340" height="144" rx="12" class="agent-card" />
          <rect x="580" y="60" width="4" height="144" class="accent-violet" clip-path="url(#clip-market)" />

          <circle cx="608" cy="91" r="13" class="icon-bg-violet" />
          <path d="M602 95v-5 M608 95v-10 M614 95v-7" class="icon-stroke-violet" />

          <text x="632" y="96" class="role-title">Market Researcher</text>
          <text x="604" y="131" class="description">Investigates the market,</text>
          <text x="604" y="150" class="description">competitors and trends.</text>

          <rect x="604" y="167" width="116" height="22" rx="5" class="task-pill-violet" />
          <text x="662" y="178" class="task-label task-label-violet">RESEARCH TASK</text>
        </g>

        <!-- Agent 3: Writer (same card + task pill pattern as the researchers) -->
        <g class="node">
          <rect x="330" y="295" width="340" height="144" rx="12" class="agent-card" />
          <rect x="330" y="295" width="4" height="144" class="accent-amber" clip-path="url(#clip-writer)" />

          <circle cx="358" cy="326" r="13" class="icon-bg-amber" />
          <path d="M353 331l2-5 7-7 3 3-7 7z" class="icon-stroke-amber" />

          <text x="382" y="331" class="role-title">Writer</text>
          <text x="354" y="366" class="description">Synthesizes the research inputs</text>
          <text x="354" y="385" class="description">into a clear, combined summary.</text>

          <rect x="354" y="402" width="140" height="22" rx="5" class="task-pill-amber" />
          <text x="424" y="413" class="task-label task-label-amber">WRITE SUMMARY TASK</text>
        </g>

        <!-- Human Input -->
        <g class="node">
          <rect x="750" y="333" width="180" height="68" rx="10" class="human-card" />
          <circle cx="773" cy="355" r="4" class="human-dot" />
          <circle cx="773" cy="367" r="4" class="human-dot" />
          <circle cx="773" cy="379" r="4" class="human-dot" />

          <text x="789" y="361" class="human-title">Human Input</text>
          <text x="789" y="381" class="human-description">Guidance &amp; feedback</text>
        </g>

        <!-- Final Output -->
        <g class="node">
          <rect x="350" y="500" width="300" height="70" rx="12" class="output-card" />
          <circle cx="382" cy="535" r="13" class="icon-bg-green" />
          <path d="M376 535l4 4 8-9" class="icon-stroke-green" />

          <text x="407" y="531" class="output-title">Final Output</text>
          <text x="407" y="550" class="output-description">Generated summary</text>
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
  overflow: hidden; /* nothing can spill outside the canvas */
}

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

.agent-card {
  fill: #ecebe6;
  stroke: none;
}

.human-card {
  fill: #fbfaf7;
  stroke: #cdb3ab;
  stroke-width: 1.2;
  stroke-dasharray: 5 4;
}

.output-card {
  fill: #dde3d5;
  stroke: none;
}

/* Accent colors */
.accent-blue { fill: #b8d3f1; }
.accent-violet { fill: #c9adb2; }
.accent-amber { fill: #d8c6a2; }

/* Icon backgrounds */
.icon-bg-blue { fill: #dbe8f8; }
.icon-bg-violet { fill: #eddfe1; }
.icon-bg-amber { fill: #efe6d3; }
.icon-bg-green { fill: #e4e9dc; }

/* Icon strokes */
.icon-stroke-blue,
.icon-stroke-violet,
.icon-stroke-amber,
.icon-stroke-green {
  fill: none;
  stroke-width: 1.5;
  stroke-linecap: round;
  stroke-linejoin: round;
}

.icon-stroke-blue { stroke: #3b4d68; }
.icon-stroke-violet { stroke: #7a5560; }
.icon-stroke-amber { stroke: #7b6749; }
.icon-stroke-green { stroke: #56614b; }

.role-title {
  fill: #3f3d38;
  font-size: 15px;
  font-weight: 650;
}

.description {
  fill: #8f8c85;
  font-size: 11.5px;
  font-weight: 400;
}

/* Task pills — one shared style for every task */
.task-pill-blue { fill: #dbe8f8; }
.task-pill-violet { fill: #eddfe1; }
.task-pill-amber { fill: #efe6d3; }

.task-label {
  font-size: 8px;
  font-weight: 700;
  text-anchor: middle;
  dominant-baseline: central;
  letter-spacing: 0.07em;
}

.task-label-blue { fill: #3b4d68; }
.task-label-violet { fill: #7a5560; }
.task-label-amber { fill: #7b6749; }

.human-dot {
  fill: #865757;
}

.human-title {
  fill: #865757;
  font-size: 12px;
  font-weight: 650;
}

.human-description {
  fill: #9a7370;
  font-size: 10px;
}

.output-title {
  fill: #56614b;
  font-size: 14px;
  font-weight: 650;
}

.output-description {
  fill: #6b7462;
  font-size: 11px;
}
</style>