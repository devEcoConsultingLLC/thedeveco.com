<script setup lang="ts">
import { ref, onMounted, onUnmounted, nextTick } from 'vue'

// Section list drives nav dot rendering and IntersectionObserver targeting.
const sectionList = [
  { id: 'hero', label: 'Hero' },
  { id: 'problem', label: 'The Problem' },
  { id: 'pipeline', label: 'DevEx Pipeline' },
  { id: 'scale', label: 'DevXRL Scale' },
  { id: 'journey', label: 'Journey Graph' },
  { id: 'deliverables', label: 'Deliverables' },
  { id: 'methodology', label: 'Methodology' },
  { id: 'process', label: 'Process' },
  { id: 'trl', label: 'What Comes After' },
  { id: 'cta', label: 'Get Started' },
] as const

interface ProblemCard {
  title: string
  body: string
}

interface Deliverable {
  num: string
  title: string
  body: string
}

interface MethodCard {
  letter: string
  title: string
  body: string
}

interface TrlBadge {
  label: string
  desc: string
}

interface ScaleRow {
  level: number
  phase: string
  def: string
  criteria: string
  product: string
  numStyle: string
}

interface TrlRow {
  level: number
  phase: string
  status: string
}

interface ProcessStep {
  num: number
  title: string
  desc: string
}

const problemCards: ProblemCard[] = [
  {
    title: 'Silent Drop-off',
    body: 'Developers who bounce never file a complaint. They choose your competitor. You only see those who stayed long enough to get frustrated publicly.',
  },
  {
    title: 'No Clear Path',
    body: 'Documentation exists but the journey between sections is undefined. Developers are expected to find their own way. Most do not.',
  },
  {
    title: 'No Feedback Loop',
    body: 'Developer pain never reaches product teams. Issues compound. Docs drift further from reality with each release.',
  },
  {
    title: 'Unmeasured Impact',
    body: 'Without metrics tied to pipeline activity, it is impossible to know if your developer program is working or where to invest.',
  },
]

const deliverables: Deliverable[] = [
  {
    num: '01',
    title: 'Pipeline & Channel Review',
    body: 'Assessment of all developer pipelines, content channels, guides, and campaigns with effectiveness ratings.',
  },
  {
    num: '02',
    title: 'Journey Map',
    body: 'Full walkthrough of the developer experience from discovery through production, every touchpoint documented.',
  },
  {
    num: '03',
    title: 'Pain Point Report',
    body: 'Every blocker, dead end, and drop-off identified with severity and impact assessment.',
  },
  {
    num: '04',
    title: 'Docs Evaluation',
    body: 'Assessment against the Diataxis framework (tutorials, how-to, reference, explanation) with restructuring recommendations.',
  },
  {
    num: '05',
    title: 'Tiered Roadmap',
    body: 'Improvements sorted by difficulty. What you can fix by restructuring vs. what requires new builds.',
  },
  {
    num: '06',
    title: 'Staff Coaching',
    body: 'One-on-one discussions with your team, paired with coaching from a 10+ year DevRel practitioner.',
  },
]

const methodologyCards: MethodCard[] = [
  {
    letter: 'A',
    title: 'Diataxis Documentation',
    body: 'Docs evaluated against four modes: tutorials, how-to guides, reference, and explanation. Most docs fail because they mix these modes without intention.',
  },
  {
    letter: 'B',
    title: 'Persona Journey Mapping',
    body: 'We identify your actual developer personas and walk each journey independently. A firmware engineer and cloud developer hit your docs differently. Both paths need to work.',
  },
  {
    letter: 'C',
    title: 'DevXRL Readiness Scoring',
    body: 'Our proprietary metric that evaluates your ability to serve developers, collect feedback, and convert activity into pipeline. Scored across community, content, onboarding, and engagement dimensions.',
  },
  {
    letter: 'D',
    title: 'Feedback Loop Analysis',
    body: 'We trace how developer input reaches (or fails to reach) product and engineering. Then design the shortest path from community signal to product decision.',
  },
]

const trlBadges: TrlBadge[] = [
  { label: 'TRL', desc: 'Technology Maturity' },
  { label: 'DevXRL', desc: 'Developer Experience' },
  { label: 'G2MRL', desc: 'Go-to-Market' },
]

const scaleData: ScaleRow[] = [
  { level: 9, phase: 'Transformative', def: 'Redefines how developers think and work. Elevates the entire ecosystem.', criteria: 'Technology sets a new industry standard. Developers build things previously unimaginable. Adoption becomes movement-like.', product: 'Paradigm Shift', numStyle: 'background:rgba(77,179,168,0.25);color:var(--dxrl-teal)' },
  { level: 8, phase: 'Magical', def: 'Experience feels effortless. Developers focus on goals, not the tool.', criteria: 'Setup is instant and invisible. APIs/workflows intuitive to the point of self-discovery. Errors rare, recovery automatic.', product: 'Best-in-Class', numStyle: 'background:rgba(77,179,168,0.2);color:var(--dxrl-teal)' },
  { level: 7, phase: 'Empowering', def: 'Accelerates developer productivity. Technology feels like an ally.', criteria: 'Advanced docs with guides and recipes. Rich tooling, IDE integration, debugging support. High-quality examples across use cases.', product: 'Ecosystem Stage', numStyle: 'background:rgba(77,179,168,0.16);color:var(--dxrl-teal)' },
  { level: 6, phase: 'Predictable', def: 'System behaves consistently, enabling trust and confidence.', criteria: 'APIs stable, versioned, backward-compatible. Errors consistent across environments. Best practices reflect real-world behavior.', product: 'Mature Product', numStyle: 'background:rgba(77,179,168,0.12);color:var(--dxrl-teal)' },
  { level: 5, phase: 'Seamless', def: 'Zero to running code with minimal effort. It just works.', criteria: 'Setup automated and consistent. Docs and examples accurate and complete. Errors meaningful and actionable.', product: 'Production-Ready', numStyle: 'background:rgba(212,168,67,0.2);color:var(--dxrl-gold)' },
  { level: 4, phase: 'Working', def: 'Basics succeed. Adoptable with patience, but still rough.', criteria: 'End-to-end tutorial works as advertised. Docs cover core APIs and concepts. Setup reproducible with effort.', product: 'Beta', numStyle: 'background:rgba(212,168,67,0.15);color:var(--dxrl-gold)' },
  { level: 3, phase: 'Confusing', def: 'Developers move forward but signals are mixed. Never sure if doing it right.', criteria: 'Multiple paths with no clear guidance. Inconsistent terminology across docs/APIs. Contradictory community vs. official advice.', product: 'Alpha', numStyle: 'background:rgba(196,91,91,0.2);color:var(--dxrl-red)' },
  { level: 2, phase: 'Frustrating', def: 'No progress without brute force. Every attempt feels blocked.', criteria: 'Docs/tutorials exist but too shallow or wrong. Examples fail without hacks. Setup fails with unclear fixes.', product: 'Early Alpha', numStyle: 'background:rgba(196,91,91,0.15);color:var(--dxrl-red)' },
  { level: 1, phase: 'Broken', def: 'Unusable. Developers cannot get started or make progress.', criteria: "Docs missing, outdated, or contradictory. Examples don't run or are absent. Setup impossible without insider help.", product: 'Prototype', numStyle: 'background:rgba(196,91,91,0.1);color:var(--dxrl-red)' },
]

const trlData: TrlRow[] = [
  { level: 9, phase: 'Market Introduction', status: 'System demonstrated in user environment. Ready for mass production.' },
  { level: 8, phase: 'Complete & Operational', status: 'Meets certifications, legal requirements, safety and privacy.' },
  { level: 7, phase: 'Operational Demo', status: 'Prototype demonstrated in client environment.' },
  { level: 6, phase: 'Test Demo', status: 'Proof of technology in controlled test setup.' },
  { level: 5, phase: 'Validation', status: 'Technology validated in real environment (pilot).' },
  { level: 4, phase: 'Lab Prototype', status: 'Design, development and validation in lab.' },
  { level: 3, phase: 'Proof of Concept', status: 'Experimental proof. Evidence for unique selling points.' },
  { level: 2, phase: 'Applied Research', status: 'Defining the technical concept. What applications are possible?' },
  { level: 1, phase: 'Fundamental Research', status: 'Research of basic principles.' },
]

const processSteps: ProcessStep[] = [
  { num: 1, title: 'Discovery', desc: 'We analyze your goals, audience, and current state to understand what you actually need.' },
  { num: 2, title: 'Audit', desc: 'Deep evaluation using DevXRL, Diataxis, and persona journey mapping across all channels.' },
  { num: 3, title: 'Deliver', desc: 'Complete report with scored findings, journey maps, and tiered improvement roadmap.' },
  { num: 4, title: 'Support', desc: 'Optional follow-on for implementation. Systems that grow, not dependencies that require us forever.' },
]

// Nav dots + section tracking
const sectionEls = ref<HTMLElement[]>([])
const activeSectionIndex = ref(0)

function collectSections() {
  sectionEls.value = Array.from(
    document.querySelectorAll<HTMLElement>('.devxrl .devxrl-section')
  )
}

function scrollToSection(index: number) {
  sectionEls.value[index]?.scrollIntoView({ behavior: 'smooth' })
}

// Observers
let sectionObserver: IntersectionObserver | null = null
let fadeObserver: IntersectionObserver | null = null
let graphObserver: IntersectionObserver | null = null

function setupObservers() {
  sectionObserver = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          const idx = sectionEls.value.indexOf(entry.target as HTMLElement)
          if (idx !== -1) activeSectionIndex.value = idx
        }
      })
    },
    { threshold: 0.5 }
  )
  sectionEls.value.forEach((s) => sectionObserver!.observe(s))

  fadeObserver = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) entry.target.classList.add('visible')
      })
    },
    { threshold: 0.15 }
  )
  document
    .querySelectorAll<HTMLElement>('.devxrl .fade-in')
    .forEach((el) => fadeObserver!.observe(el))
}

// Scale table toggle (single row expand)
const highlightedLevel = ref<number | null>(null)
function toggleLevel(level: number) {
  highlightedLevel.value = highlightedLevel.value === level ? null : level
}

// Journey graph
const journeyCanvas = ref<HTMLCanvasElement | null>(null)
const activeGraphView = ref<'both' | 'current' | 'improved'>('both')
let showCurrent = true
let showImproved = true
let animProgress = 0
let animating = false

const stages = [
  'Discovery',
  'Landing\nPage',
  'Docs\nHome',
  'Getting\nStarted',
  'First\nBuild',
  'Debug',
  'Integration',
  'Production',
  'Advocate',
]
const currentData = [65, 55, 40, 25, 30, 12, 20, 35, 28]
const improvedData = [70, 72, 75, 78, 82, 76, 85, 90, 92]

const W = 860
const H = 320
const pad = { top: 35, right: 25, bottom: 45, left: 40 }
const plotW = W - pad.left - pad.right
const plotH = H - pad.top - pad.bottom

function valToY(v: number) {
  return pad.top + plotH - (v / 100) * plotH
}
function idxToX(i: number) {
  return pad.left + (i / (stages.length - 1)) * plotW
}

function drawGraph(p: number) {
  const canvas = journeyCanvas.value
  if (!canvas) return
  const ctx = canvas.getContext('2d')
  if (!ctx) return

  const dpr = window.devicePixelRatio || 1
  canvas.width = W * dpr
  canvas.height = H * dpr
  canvas.style.width = '100%'
  canvas.style.height = 'auto'
  canvas.style.maxHeight = '320px'
  ctx.scale(dpr, dpr)
  ctx.clearRect(0, 0, W, H)

  // grid
  ctx.strokeStyle = 'rgba(30,58,58,0.06)'
  ctx.lineWidth = 1
  for (let v = 0; v <= 100; v += 25) {
    const y = valToY(v)
    ctx.beginPath()
    ctx.moveTo(pad.left, y)
    ctx.lineTo(W - pad.right, y)
    ctx.stroke()
  }

  // axis labels
  ctx.fillStyle = 'rgba(30,58,58,0.3)'
  ctx.font = "9px 'JetBrains Mono',monospace"
  ctx.textAlign = 'right'
  ctx.fillText('HIGH', pad.left - 8, pad.top + 4)
  ctx.fillText('LOW', pad.left - 8, pad.top + plotH + 4)

  // stage labels
  ctx.textAlign = 'center'
  ctx.fillStyle = 'rgba(30,58,58,0.5)'
  stages.forEach((l, i) => {
    const x = idxToX(i)
    l.split('\n').forEach((ln, li) =>
      ctx.fillText(ln, x, H - pad.bottom + 14 + li * 12)
    )
  })

  function drawCurve(data: number[], color: string, show: boolean) {
    if (!show || !ctx) return
    const pts = data.map((v, i) => ({ x: idxToX(i), y: valToY(v * p) }))
    ctx.beginPath()
    ctx.moveTo(pts[0]!.x, pts[0]!.y)
    for (let i = 0; i < pts.length - 1; i++) {
      const c1 = pts[i]!.x + (pts[i + 1]!.x - pts[i]!.x) * 0.4
      const c2 = pts[i + 1]!.x - (pts[i + 1]!.x - pts[i]!.x) * 0.4
      ctx.bezierCurveTo(c1, pts[i]!.y, c2, pts[i + 1]!.y, pts[i + 1]!.x, pts[i + 1]!.y)
    }
    ctx.strokeStyle = color
    ctx.lineWidth = 2.5
    ctx.stroke()

    ctx.lineTo(pts[pts.length - 1]!.x, valToY(0))
    ctx.lineTo(pts[0]!.x, valToY(0))
    ctx.closePath()
    const grad = ctx.createLinearGradient(0, pad.top, 0, pad.top + plotH)
    const r = parseInt(color.slice(1, 3), 16)
    const g = parseInt(color.slice(3, 5), 16)
    const b = parseInt(color.slice(5, 7), 16)
    grad.addColorStop(0, `rgba(${r},${g},${b},0.1)`)
    grad.addColorStop(1, `rgba(${r},${g},${b},0.01)`)
    ctx.fillStyle = grad
    ctx.fill()

    pts.forEach((pt) => {
      ctx.beginPath()
      ctx.arc(pt.x, pt.y, 3.5, 0, Math.PI * 2)
      ctx.fillStyle = color
      ctx.fill()
      ctx.strokeStyle = 'rgba(244,241,232,0.8)'
      ctx.lineWidth = 1.5
      ctx.stroke()
    })
  }

  drawCurve(currentData, '#c45b5b', showCurrent)
  drawCurve(improvedData, '#4db3a8', showImproved)

  if (showCurrent && p > 0.8) {
    const a = Math.min(1, (p - 0.8) / 0.2)
    ctx.fillStyle = `rgba(196,91,91,${a * 0.8})`
    ctx.font = "8px 'JetBrains Mono',monospace"
    ctx.textAlign = 'center'
    const dx = idxToX(5)
    const dy = valToY(currentData[5]! * p)
    ctx.fillText('drop-off', dx, dy - 12)
    ctx.strokeStyle = `rgba(196,91,91,${a * 0.5})`
    ctx.lineWidth = 1
    ctx.setLineDash([3, 3])
    ctx.beginPath()
    ctx.moveTo(dx, dy - 9)
    ctx.lineTo(dx, dy - 5)
    ctx.stroke()
    ctx.setLineDash([])
  }
}

function setupGraphObserver() {
  const canvas = journeyCanvas.value
  if (!canvas) return
  graphObserver = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting && !animating) {
          animating = true
          animProgress = 0
          function tick() {
            animProgress += 0.03
            if (animProgress > 1) animProgress = 1
            drawGraph(animProgress)
            if (animProgress < 1) requestAnimationFrame(tick)
          }
          tick()
        }
      })
    },
    { threshold: 0.3 }
  )
  graphObserver.observe(canvas)
}

function setGraphView(view: 'both' | 'current' | 'improved') {
  activeGraphView.value = view
  showCurrent = view === 'both' || view === 'current'
  showImproved = view === 'both' || view === 'improved'
  drawGraph(animProgress)
}

onMounted(async () => {
  document.documentElement.classList.add('devxrl-page')
  await nextTick()
  collectSections()
  setupObservers()
  setupGraphObserver()
  drawGraph(0)
})

onUnmounted(() => {
  document.documentElement.classList.remove('devxrl-page')
  sectionObserver?.disconnect()
  fadeObserver?.disconnect()
  graphObserver?.disconnect()
})
</script>

<template>
  <div class="devxrl">
    <!-- Nav dots -->
    <nav class="devxrl-nav-dots" aria-label="Section navigation">
      <button
        v-for="(section, i) in sectionList"
        :key="section.id"
        type="button"
        class="devxrl-nav-dot"
        :class="{ active: activeSectionIndex === i }"
        :aria-label="`Jump to ${section.label}`"
        @click="scrollToSection(i)"
      ></button>
    </nav>

    <!-- HERO -->
    <section id="hero" class="devxrl-section devxrl-hero">
      <div class="devxrl-container">
        <div class="devxrl-hero-logo">
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 380 100" width="260">
            <g transform="translate(5,5)">
              <circle cx="39.3" cy="39.5" r="30.3" fill="none" stroke="#4db3a8" stroke-width="2.5" />
              <line x1="39.29" y1="39.5" x2="39.15" y2="18.63" stroke="#4db3a8" stroke-width="2" stroke-linecap="round" />
              <line x1="39.29" y1="39.5" x2="49.69" y2="29.78" stroke="#4db3a8" stroke-width="2" stroke-linecap="round" />
              <line x1="39.29" y1="39.5" x2="59.84" y2="39.5" stroke="#4db3a8" stroke-width="2" stroke-linecap="round" />
              <line x1="39.29" y1="39.5" x2="49.69" y2="49.22" stroke="#4db3a8" stroke-width="2" stroke-linecap="round" />
              <line x1="39.29" y1="39.5" x2="39.15" y2="60.37" stroke="#4db3a8" stroke-width="2" stroke-linecap="round" />
              <line x1="39.29" y1="39.5" x2="28.31" y2="49.22" stroke="#4db3a8" stroke-width="2" stroke-linecap="round" />
              <line x1="39.29" y1="39.5" x2="18.16" y2="39.5" stroke="#4db3a8" stroke-width="2" stroke-linecap="round" />
              <line x1="39.29" y1="39.5" x2="28.31" y2="29.78" stroke="#4db3a8" stroke-width="2" stroke-linecap="round" />
              <circle cx="39.15" cy="18.63" r="3.5" fill="#3d9991" stroke="#1e3a3a" stroke-width="1.2" />
              <circle cx="59.84" cy="39.5" r="3.5" fill="#3d9991" stroke="#1e3a3a" stroke-width="1.2" />
              <circle cx="39.15" cy="60.37" r="3.5" fill="#3d9991" stroke="#1e3a3a" stroke-width="1.2" />
              <circle cx="18.16" cy="39.5" r="3.5" fill="#3d9991" stroke="#1e3a3a" stroke-width="1.2" />
              <circle cx="49.69" cy="29.78" r="3.5" fill="#3d9991" stroke="#1e3a3a" stroke-width="1.2" />
              <circle cx="49.69" cy="49.22" r="3.5" fill="#3d9991" stroke="#1e3a3a" stroke-width="1.2" />
              <circle cx="28.31" cy="49.22" r="3.5" fill="#3d9991" stroke="#1e3a3a" stroke-width="1.2" />
              <circle cx="28.31" cy="29.78" r="3.5" fill="#3d9991" stroke="#1e3a3a" stroke-width="1.2" />
              <circle cx="39.29" cy="39.5" r="2" fill="#3d9991" stroke="#1e3a3a" stroke-width="1.2" />
            </g>
            <text x="95" y="48" font-family="'JetBrains Mono',monospace" font-size="36" font-weight="bold" fill="#fff">DEV</text>
            <text x="165" y="48" font-family="'JetBrains Mono',monospace" font-size="36" font-weight="bold" fill="#4db3a8">ECO</text>
            <text x="97" y="68" font-family="'JetBrains Mono',monospace" font-size="11" letter-spacing="0.2em" fill="#fff">CONSULTING LLC</text>
          </svg>
        </div>
        <h3>Developer Relations Consulting</h3>
        <h1 class="devxrl-hero-title">Dev<span class="devxrl-accent">XRL</span> Audit</h1>
        <p class="devxrl-hero-sub">Developer Experience and Readiness Levels</p>
        <p class="devxrl-hero-tagline">
          A structured evaluation of your developer journey, documentation, content pipelines, and
          community engagement. Where things break, where developers drop off, and what to do about
          it.
        </p>
      </div>
      <div class="devxrl-scroll-hint">
        <svg viewBox="0 0 24 24">
          <path d="M12 5v14M19 12l-7 7-7-7" />
        </svg>
        <span class="devxrl-small">scroll</span>
      </div>
    </section>

    <!-- PROBLEM -->
    <section id="problem" class="devxrl-section devxrl-problem">
      <div class="devxrl-container">
        <span class="devxrl-tag devxrl-tag-red">THE PROBLEM</span>
        <h2>Developers Quit Before They Start</h2>
        <p>
          Most companies lose developers in the first 30 minutes. Broken getting-started guides,
          scattered docs, unclear paths. You never hear about it because they leave silently.
        </p>
        <div class="devxrl-problem-grid fade-in">
          <div class="devxrl-problem-card">
            <div class="devxrl-icon-box">
              <svg viewBox="0 0 24 24">
                <path d="M9 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h4" />
                <polyline points="16 17 21 12 16 7" />
                <line x1="21" y1="12" x2="9" y2="12" />
              </svg>
            </div>
            <h4>{{ problemCards[0]!.title }}</h4>
            <p>{{ problemCards[0]!.body }}</p>
          </div>
          <div class="devxrl-problem-card">
            <div class="devxrl-icon-box">
              <svg viewBox="0 0 24 24">
                <circle cx="12" cy="12" r="10" />
                <line x1="12" y1="8" x2="12" y2="12" />
                <line x1="12" y1="16" x2="12.01" y2="16" />
              </svg>
            </div>
            <h4>{{ problemCards[1]!.title }}</h4>
            <p>{{ problemCards[1]!.body }}</p>
          </div>
          <div class="devxrl-problem-card">
            <div class="devxrl-icon-box">
              <svg viewBox="0 0 24 24">
                <path d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z" />
              </svg>
            </div>
            <h4>{{ problemCards[2]!.title }}</h4>
            <p>{{ problemCards[2]!.body }}</p>
          </div>
          <div class="devxrl-problem-card">
            <div class="devxrl-icon-box">
              <svg viewBox="0 0 24 24">
                <polyline points="22 12 18 12 15 21 9 3 6 12 2 12" />
              </svg>
            </div>
            <h4>{{ problemCards[3]!.title }}</h4>
            <p>{{ problemCards[3]!.body }}</p>
          </div>
        </div>
      </div>
    </section>

    <!-- PIPELINE -->
    <section id="pipeline" class="devxrl-section devxrl-pipeline">
      <div class="devxrl-container">
        <span class="devxrl-tag devxrl-tag-gold">THE DEVEX PIPELINE</span>
        <h2>A Data-Driven Approach</h2>
        <div class="devxrl-pipeline-layout fade-in">
          <div class="devxrl-pipeline-diagram">
            <div class="devxrl-pipe-node devxrl-pipe-rect-wide">TARGET AUDIENCE</div>
            <div class="devxrl-pipe-arrow"></div>
            <div class="devxrl-pipe-node devxrl-pipe-rect">MARKETING CAMPAIGNS</div>
            <div class="devxrl-pipe-arrow"></div>
            <div class="devxrl-pipe-node devxrl-pipe-rect">COLLECTION CTA</div>
            <div class="devxrl-pipe-arrow"></div>
            <div class="devxrl-pipe-group-gold">
              <div class="devxrl-pipe-node devxrl-pipe-inner">DATA COLLECTION</div>
              <div class="devxrl-pipe-label">ACTIVATION FUNNEL</div>
              <div class="devxrl-pipe-arrow-inner"></div>
              <div class="devxrl-pipe-node devxrl-pipe-inner">LANDING CTA</div>
            </div>
            <div class="devxrl-pipe-arrow"></div>
            <div class="devxrl-pipe-node devxrl-pipe-rect">PORTFOLIO</div>
            <div class="devxrl-pipe-arrow"></div>
            <div class="devxrl-pipe-branch">
              <div class="devxrl-pipe-branch-item">Social</div>
              <div class="devxrl-pipe-branch-item">Discord</div>
              <div class="devxrl-pipe-branch-item">etc.</div>
            </div>
            <div class="devxrl-pipe-arrow"></div>
            <div class="devxrl-pipe-group-teal">
              <div class="devxrl-pipe-node devxrl-pipe-inner-teal">OBSERVE / NURTURE / ENABLE</div>
              <div class="devxrl-pipe-arrow-inner"></div>
              <div class="devxrl-pipe-node devxrl-pipe-inner-teal">OPPORTUNITY</div>
              <div class="devxrl-pipe-label">CONVERSION FUNNEL</div>
              <div class="devxrl-pipe-arrow-inner"></div>
              <div class="devxrl-pipe-node devxrl-pipe-inner-teal">WIN</div>
            </div>
          </div>
          <div class="devxrl-pipeline-text">
            <div>
              <h4>Where Pipelines Break</h4>
              <p>
                Most companies divide the pipeline across teams and departments. Strategies align to
                KPIs at the top but drift as they move through stages. Limited resources, gaps in
                expertise, and weak cross-stage connections cause initiatives to stall. High
                turnover forces organizations to restart or abandon programs entirely.
              </p>
            </div>
            <div>
              <h4>What We Do About It</h4>
              <p>
                devEco™ bridges these gaps with the knowledge, skills, network, and process required
                to move initiatives beyond the difficult conversion layer. This creates more
                opportunities, strengthens developer engagement, and brings initiatives closer to
                measurable business outcomes.
              </p>
            </div>
            <div class="devxrl-pipeline-callout">
              The industry treats developer initiatives as cost centers. We design solutions that
              unlock value at every stage of the funnel, ensuring every dollar drives measurable
              impact.
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- SCALE -->
    <section id="scale" class="devxrl-section devxrl-scale">
      <div class="devxrl-container">
        <span class="devxrl-tag devxrl-tag-teal" style="color:var(--dxrl-teal);border-color:var(--dxrl-teal)">THE FRAMEWORK</span>
        <h2 style="color:#fff">DevXRL Readiness Levels</h2>
        <p>Where does your developer experience sit? Click any level to see the evaluation criteria.</p>
        <div class="devxrl-scale-wrapper fade-in">
          <table class="devxrl-scale-table">
            <thead>
              <tr>
                <th></th>
                <th>Phase</th>
                <th>Definition</th>
                <th>Product Status</th>
              </tr>
            </thead>
            <tbody>
              <tr
                v-for="row in scaleData"
                :key="row.level"
                :class="{ highlighted: highlightedLevel === row.level }"
                @click="toggleLevel(row.level)"
              >
                <td>
                  <span class="devxrl-scale-level-num" :style="row.numStyle">{{ row.level }}</span>
                </td>
                <td class="devxrl-scale-phase">{{ row.phase }}</td>
                <td class="devxrl-scale-def">
                  {{ row.def }}
                  <div class="devxrl-scale-criteria">{{ row.criteria }}</div>
                </td>
                <td class="devxrl-scale-product">{{ row.product }}</td>
              </tr>
            </tbody>
          </table>
        </div>
        <div class="devxrl-scale-footer">
          Many companies will never truly succeed in building a developer community. Technologies
          are never frictionless, especially early on.
          <strong>DevXRL helps organizations understand where they sit and what it takes to advance.</strong>
          We deconstruct outdated practices into building blocks, then rebuild with methods that
          drive progress.
        </div>
      </div>
    </section>

    <!-- JOURNEY -->
    <section id="journey" class="devxrl-section devxrl-journey">
      <div class="devxrl-container">
        <span class="devxrl-tag devxrl-tag-teal">EXAMPLE AUDIT OUTPUT</span>
        <h2>Developer Journey Analysis</h2>
        <p>
          We map the full experience from first contact through production. This is what we
          typically find versus what the path should look like.
        </p>
        <div class="devxrl-graph-container fade-in">
          <canvas
            ref="journeyCanvas"
            class="devxrl-graph-canvas"
            width="860"
            height="320"
          ></canvas>
          <div class="devxrl-graph-toggle">
            <button
              type="button"
              :class="{ active: activeGraphView === 'both' }"
              @click="setGraphView('both')"
            >BOTH</button>
            <button
              type="button"
              :class="{ active: activeGraphView === 'current' }"
              @click="setGraphView('current')"
            >CURRENT STATE</button>
            <button
              type="button"
              :class="{ active: activeGraphView === 'improved' }"
              @click="setGraphView('improved')"
            >AFTER AUDIT</button>
          </div>
          <div class="devxrl-graph-legend">
            <div class="devxrl-legend-item">
              <div class="devxrl-legend-swatch" style="background:#c45b5b"></div>
              <span>Current developer experience</span>
            </div>
            <div class="devxrl-legend-item">
              <div class="devxrl-legend-swatch" style="background:#4db3a8"></div>
              <span>Post-audit improvement</span>
            </div>
          </div>
          <p class="devxrl-graph-note">
            Composite of real findings. Y-axis = developer confidence at each stage. Results vary
            per engagement.
          </p>
        </div>
      </div>
    </section>

    <!-- DELIVERABLES -->
    <section id="deliverables" class="devxrl-section devxrl-deliverables">
      <div class="devxrl-container">
        <span class="devxrl-tag devxrl-tag-teal">WHAT YOU GET</span>
        <h2>Audit Deliverables</h2>
        <p>Concrete outputs. No filler decks, no hypothetical roadmaps disconnected from your stack.</p>
        <div class="devxrl-deliverables-list stagger fade-in">
          <div
            v-for="item in deliverables"
            :key="item.num"
            class="devxrl-deliverable-item"
          >
            <span class="devxrl-deliverable-num">{{ item.num }}</span>
            <span class="devxrl-deliverable-text">
              <strong>{{ item.title }}</strong> : {{ item.body }}
            </span>
          </div>
        </div>
      </div>
    </section>

    <!-- METHODOLOGY -->
    <section id="methodology" class="devxrl-section devxrl-methodology">
      <div class="devxrl-container">
        <span class="devxrl-tag devxrl-tag-gold">METHODOLOGY</span>
        <h2>How We Evaluate</h2>
        <p>
          Frameworks applied by people who have run developer programs at AWS, SORACOM, SignalWire,
          and in hackerspaces.
        </p>
        <div class="devxrl-method-grid fade-in">
          <div
            v-for="card in methodologyCards"
            :key="card.letter"
            class="devxrl-method-card"
          >
            <span class="devxrl-method-num">{{ card.letter }}</span>
            <h4>{{ card.title }}</h4>
            <p>{{ card.body }}</p>
          </div>
        </div>
      </div>
    </section>

    <!-- PROCESS -->
    <section id="process" class="devxrl-section devxrl-process">
      <div class="devxrl-container">
        <span class="devxrl-tag devxrl-tag-teal">OUR PROCESS</span>
        <h2>From Discovery to Delivery</h2>
        <p>Embedded partner, not external auditor. We work alongside your team.</p>
        <div class="devxrl-process-steps fade-in">
          <div
            v-for="step in processSteps"
            :key="step.num"
            class="devxrl-process-step"
          >
            <div class="devxrl-step-hex">
              <svg viewBox="0 0 56 56">
                <polygon
                  points="28,2 52,15 52,41 28,54 4,41 4,15"
                  fill="var(--dxrl-teal)"
                  stroke="var(--dxrl-teal-deep)"
                  stroke-width="1.5"
                />
                <text
                  x="28"
                  y="33"
                  text-anchor="middle"
                  font-family="'JetBrains Mono',monospace"
                  font-size="18"
                  font-weight="700"
                  fill="var(--dxrl-teal-deep)"
                >{{ step.num }}</text>
              </svg>
            </div>
            <h4>{{ step.title }}</h4>
            <p>{{ step.desc }}</p>
          </div>
        </div>
      </div>
    </section>

    <!-- TRL / WHAT COMES AFTER -->
    <section id="trl" class="devxrl-section devxrl-trl">
      <div class="devxrl-container">
        <span class="devxrl-tag devxrl-tag-gold">WHAT COMES AFTER THE AUDIT</span>
        <h2>Additional Readiness Assessments</h2>
        <p>
          The DevXRL audit covers developer experience. For organizations that need a broader view,
          we offer two additional frameworks as separate engagements.
        </p>
        <div class="devxrl-trl-layout fade-in">
          <div class="devxrl-trl-text">
            <h4>Technology Readiness Levels (TRL)</h4>
            <p>
              Adapted from NASA and widely used in hardware and deep-tech, TRL provides a clear path
              from fundamental research to market-ready product. We map your technology's maturity
              and build a stage-by-stage roadmap with focused time-to-market objectives.
            </p>
            <table class="devxrl-trl-mini-table" style="margin-top:0.75rem">
              <thead>
                <tr>
                  <th>TRL</th>
                  <th>Phase</th>
                  <th>Status</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="row in trlData" :key="row.level">
                  <td>{{ row.level }}</td>
                  <td class="devxrl-trl-phase">{{ row.phase }}</td>
                  <td>{{ row.status }}</td>
                </tr>
              </tbody>
            </table>
          </div>
          <div class="devxrl-trl-text">
            <h4>Go-to-Market Readiness (G2MRL)</h4>
            <p>
              A custom framework we build per engagement that assesses your sales, marketing, and
              partnership readiness alongside technical maturity. G2MRL maps how prepared your
              organization is to convert developer interest into pipeline activity and revenue.
            </p>
            <p style="margin-top:0.75rem">
              Technology can be mature while developer experience lags behind. Marketing can be
              strong while onboarding is broken. These gaps are where adoption stalls. By combining
              all three frameworks, we give you a complete picture.
            </p>
            <div class="devxrl-trl-badges">
              <div
                v-for="badge in trlBadges"
                :key="badge.label"
                class="devxrl-trl-badge"
              >
                <div class="devxrl-trl-badge-label">{{ badge.label }}</div>
                <div class="devxrl-trl-badge-desc">{{ badge.desc }}</div>
              </div>
            </div>
            <p style="margin-top:0.75rem;font-size:0.72rem;color:var(--dxrl-gray)">
              Each uses a 1-9 scale. These are separate engagements we recommend after the DevXRL
              audit surfaces where your organization needs to focus next.
            </p>
          </div>
        </div>
      </div>
    </section>

    <!-- CTA -->
    <section id="cta" class="devxrl-section devxrl-cta">
      <div class="devxrl-container">
        <span class="devxrl-tag devxrl-tag-teal" style="color:var(--dxrl-teal);border-color:var(--dxrl-teal)">GET STARTED</span>
        <h2 style="color:#fff">Your developers deserve a better experience.</h2>
        <p>
          This is our general offering. Upon discussion we tailor the investigation to your specific
          needs, product, and developer audience.
        </p>
        <div class="devxrl-cta-row">
          <a href="mailto:hello@thedeveco.com" class="devxrl-btn devxrl-btn-primary">Contact Us</a>
          <a
            href="https://thedeveco.com"
            target="_blank"
            rel="noopener noreferrer"
            class="devxrl-btn devxrl-btn-outline"
          >thedeveco.com</a>
        </div>
        <div class="devxrl-cta-footer">
          <div>
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 380 100" width="140">
              <g transform="translate(5,5)">
                <circle cx="39.3" cy="39.5" r="30.3" fill="none" stroke="#4db3a8" stroke-width="2.5" />
                <line x1="39.29" y1="39.5" x2="39.15" y2="18.63" stroke="#4db3a8" stroke-width="2" stroke-linecap="round" />
                <line x1="39.29" y1="39.5" x2="49.69" y2="29.78" stroke="#4db3a8" stroke-width="2" stroke-linecap="round" />
                <line x1="39.29" y1="39.5" x2="59.84" y2="39.5" stroke="#4db3a8" stroke-width="2" stroke-linecap="round" />
                <line x1="39.29" y1="39.5" x2="49.69" y2="49.22" stroke="#4db3a8" stroke-width="2" stroke-linecap="round" />
                <line x1="39.29" y1="39.5" x2="39.15" y2="60.37" stroke="#4db3a8" stroke-width="2" stroke-linecap="round" />
                <line x1="39.29" y1="39.5" x2="28.31" y2="49.22" stroke="#4db3a8" stroke-width="2" stroke-linecap="round" />
                <line x1="39.29" y1="39.5" x2="18.16" y2="39.5" stroke="#4db3a8" stroke-width="2" stroke-linecap="round" />
                <line x1="39.29" y1="39.5" x2="28.31" y2="29.78" stroke="#4db3a8" stroke-width="2" stroke-linecap="round" />
                <circle cx="39.15" cy="18.63" r="3.5" fill="#3d9991" stroke="#1e3a3a" stroke-width="1.2" />
                <circle cx="59.84" cy="39.5" r="3.5" fill="#3d9991" stroke="#1e3a3a" stroke-width="1.2" />
                <circle cx="39.15" cy="60.37" r="3.5" fill="#3d9991" stroke="#1e3a3a" stroke-width="1.2" />
                <circle cx="18.16" cy="39.5" r="3.5" fill="#3d9991" stroke="#1e3a3a" stroke-width="1.2" />
                <circle cx="49.69" cy="29.78" r="3.5" fill="#3d9991" stroke="#1e3a3a" stroke-width="1.2" />
                <circle cx="49.69" cy="49.22" r="3.5" fill="#3d9991" stroke="#1e3a3a" stroke-width="1.2" />
                <circle cx="28.31" cy="49.22" r="3.5" fill="#3d9991" stroke="#1e3a3a" stroke-width="1.2" />
                <circle cx="28.31" cy="29.78" r="3.5" fill="#3d9991" stroke="#1e3a3a" stroke-width="1.2" />
                <circle cx="39.29" cy="39.5" r="2" fill="#3d9991" stroke="#1e3a3a" stroke-width="1.2" />
              </g>
              <text x="95" y="48" font-family="'JetBrains Mono',monospace" font-size="36" font-weight="bold" fill="#fff">DEV</text>
              <text x="165" y="48" font-family="'JetBrains Mono',monospace" font-size="36" font-weight="bold" fill="#4db3a8">ECO</text>
              <text x="97" y="68" font-family="'JetBrains Mono',monospace" font-size="11" letter-spacing="0.2em" fill="#fff">CONSULTING LLC</text>
            </svg>
          </div>
          <span class="devxrl-small" style="color:rgba(255,255,255,0.25)">Est. 2024</span>
        </div>
      </div>
    </section>
  </div>
</template>

<style>
/* Scroll-snap applies only when this page is mounted. */
html.devxrl-page {
  scroll-snap-type: y mandatory;
  scroll-behavior: smooth;
}

/* Noise overlay scoped to the DevXRL page only. */
html.devxrl-page body::after {
  content: '';
  position: fixed;
  inset: 0;
  pointer-events: none;
  z-index: 9999;
  opacity: 0.025;
  background: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
}

/* Everything below is namespaced under .devxrl to prevent leakage. */
.devxrl {
  /* Local palette (prefixed to avoid collision with global tokens in App.vue) */
  --dxrl-bg: #f0ede4;
  --dxrl-bg-alt: #e8e5dc;
  --dxrl-ink: #1e3a3a;
  --dxrl-teal: #4db3a8;
  --dxrl-teal-dark: #3d9991;
  --dxrl-teal-deep: #1e3a3a;
  --dxrl-gold: #d4a843;
  --dxrl-gold-dim: #c49a3a;
  --dxrl-paper: #f4f1e8;
  --dxrl-red: #c45b5b;
  --dxrl-gray: #8a9090;
  --dxrl-gray-light: #c8c5bc;

  font-family: 'JetBrains Mono', monospace;
  background: var(--dxrl-bg);
  color: var(--dxrl-ink);
  line-height: 1.6;
  overflow-x: hidden;
}

.devxrl *,
.devxrl *::before,
.devxrl *::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.devxrl .devxrl-section {
  min-height: 100vh;
  scroll-snap-align: start;
  scroll-snap-stop: always;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 2.5rem 2rem;
  position: relative;
}

.devxrl .devxrl-container {
  max-width: 960px;
  width: 100%;
}

/* Nav dots */
.devxrl-nav-dots {
  position: fixed;
  right: 1.5rem;
  top: 50%;
  transform: translateY(-50%);
  z-index: 100;
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.devxrl-nav-dot {
  width: 8px;
  height: 8px;
  border: 1.5px solid var(--dxrl-teal-dark);
  border-radius: 50%;
  background: transparent;
  cursor: pointer;
  transition: all 0.3s ease;
  padding: 0;
}

.devxrl-nav-dot.active {
  background: var(--dxrl-teal);
  border-color: var(--dxrl-teal);
  transform: scale(1.4);
}

.devxrl-nav-dot:hover {
  border-color: var(--dxrl-teal);
}

/* Typography */
.devxrl h1 {
  font-size: clamp(2rem, 5vw, 3.4rem);
  font-weight: 800;
  letter-spacing: -0.02em;
  line-height: 1.1;
}

.devxrl h2 {
  font-size: clamp(1.3rem, 2.8vw, 1.8rem);
  font-weight: 700;
  line-height: 1.25;
  margin-bottom: 0.6rem;
}

.devxrl h3 {
  font-size: 0.7rem;
  font-weight: 600;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: var(--dxrl-teal-dark);
  margin-bottom: 0.5rem;
}

.devxrl p {
  font-size: 0.82rem;
  font-weight: 400;
  max-width: 580px;
  color: #4a5555;
  line-height: 1.6;
}

.devxrl .devxrl-small {
  font-size: 0.7rem;
  color: var(--dxrl-gray);
}

/* Tags */
.devxrl .devxrl-tag {
  display: inline-block;
  font-size: 0.6rem;
  font-weight: 600;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  padding: 2px 8px;
  border: 1.5px dashed;
  margin-bottom: 0.75rem;
}

.devxrl .devxrl-tag-teal {
  color: var(--dxrl-teal-dark);
  border-color: var(--dxrl-teal);
}

.devxrl .devxrl-tag-gold {
  color: var(--dxrl-gold-dim);
  border-color: var(--dxrl-gold);
}

.devxrl .devxrl-tag-red {
  color: var(--dxrl-red);
  border-color: var(--dxrl-red);
}

/* HERO */
.devxrl .devxrl-hero {
  background: var(--dxrl-teal-deep);
  color: #fff;
}

.devxrl .devxrl-hero p {
  color: rgba(255, 255, 255, 0.6);
}

.devxrl .devxrl-hero h3 {
  color: var(--dxrl-teal);
}

.devxrl .devxrl-hero-logo {
  margin-bottom: 2rem;
}

.devxrl .devxrl-hero-title {
  color: #fff;
  margin-bottom: 0.4rem;
}

.devxrl .devxrl-accent {
  color: var(--dxrl-teal);
}

.devxrl .devxrl-hero-sub {
  font-size: clamp(0.95rem, 2vw, 1.2rem);
  font-weight: 400;
  color: rgba(255, 255, 255, 0.45);
  margin-bottom: 1.5rem;
}

.devxrl .devxrl-hero-tagline {
  font-size: 0.8rem;
  color: rgba(255, 255, 255, 0.5);
  max-width: 520px;
  line-height: 1.65;
}

.devxrl .devxrl-scroll-hint {
  position: absolute;
  bottom: 1.5rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 4px;
  color: rgba(255, 255, 255, 0.25);
  animation: devxrl-nudge 2s ease infinite;
}

.devxrl .devxrl-scroll-hint svg {
  width: 18px;
  height: 18px;
  stroke: currentColor;
  fill: none;
  stroke-width: 2;
  stroke-linecap: round;
  stroke-linejoin: round;
}

@keyframes devxrl-nudge {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(5px); }
}

/* PROBLEM */
.devxrl .devxrl-problem {
  background: var(--dxrl-bg);
}

.devxrl .devxrl-problem-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1.25rem;
  margin-top: 1.25rem;
}

.devxrl .devxrl-problem-card {
  background: var(--dxrl-paper);
  border: 1px solid var(--dxrl-gray-light);
  padding: 1rem 1.15rem;
}

.devxrl .devxrl-problem-card h4 {
  font-size: 0.78rem;
  font-weight: 700;
  margin-bottom: 0.4rem;
}

.devxrl .devxrl-problem-card p {
  font-size: 0.72rem;
  line-height: 1.55;
}

.devxrl .devxrl-icon-box {
  width: 28px;
  height: 28px;
  margin-bottom: 0.6rem;
  color: var(--dxrl-teal-dark);
}

.devxrl .devxrl-icon-box svg {
  width: 100%;
  height: 100%;
  stroke: currentColor;
  fill: none;
  stroke-width: 1.5;
  stroke-linecap: round;
  stroke-linejoin: round;
}

/* PIPELINE */
.devxrl .devxrl-pipeline {
  background: var(--dxrl-bg-alt);
}

.devxrl .devxrl-pipeline-layout {
  display: grid;
  grid-template-columns: 240px 1fr;
  gap: 2.5rem;
  margin-top: 1.25rem;
  align-items: start;
}

.devxrl .devxrl-pipeline-diagram {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.devxrl .devxrl-pipe-node {
  text-align: center;
  font-size: 0.5rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  padding: 5px 12px;
  min-width: 140px;
  position: relative;
  z-index: 1;
}

.devxrl .devxrl-pipe-rect {
  background: var(--dxrl-paper);
  border: 1.5px solid var(--dxrl-gray-light);
  color: var(--dxrl-ink);
  border-radius: 3px;
}

.devxrl .devxrl-pipe-rect-wide {
  background: var(--dxrl-paper);
  border: 1.5px solid var(--dxrl-gray-light);
  color: var(--dxrl-ink);
  border-radius: 3px;
  min-width: 200px;
  padding: 14px 12px;
}

.devxrl .devxrl-pipe-inner {
  padding: 4px 10px;
  background: rgba(255, 255, 255, 0.45);
  border-radius: 3px;
  border: 1px solid rgba(255, 255, 255, 0.3);
}

.devxrl .devxrl-pipe-inner-teal {
  padding: 4px 10px;
  background: rgba(255, 255, 255, 0.2);
  border-radius: 3px;
  border: 1px solid rgba(255, 255, 255, 0.15);
}

.devxrl .devxrl-pipe-group-gold {
  background: var(--dxrl-gold);
  padding: 10px 14px 14px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0;
  clip-path: polygon(0% 0%, 100% 0%, 75% 100%, 25% 100%);
  min-width: 210px;
}

.devxrl .devxrl-pipe-group-gold .devxrl-pipe-node {
  background: none;
  border: none;
  color: var(--dxrl-teal-deep);
  min-width: auto;
  padding: 3px 8px;
}

.devxrl .devxrl-pipe-group-gold .devxrl-pipe-label {
  font-size: 0.42rem;
  font-weight: 600;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--dxrl-teal-deep);
  opacity: 0.7;
  margin-top: 2px;
}

.devxrl .devxrl-pipe-group-teal {
  background: var(--dxrl-teal);
  padding: 12px 14px 14px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0;
  clip-path: polygon(5% 0%, 95% 0%, 80% 100%, 20% 100%);
  min-width: 210px;
}

.devxrl .devxrl-pipe-group-teal .devxrl-pipe-node {
  background: none;
  border: none;
  color: var(--dxrl-teal-deep);
  min-width: auto;
  padding: 3px 8px;
}

.devxrl .devxrl-pipe-group-teal .devxrl-pipe-label {
  font-size: 0.42rem;
  font-weight: 600;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--dxrl-teal-deep);
  opacity: 0.7;
  margin-top: 2px;
}

.devxrl .devxrl-pipe-arrow {
  width: 1.5px;
  height: 10px;
  background: var(--dxrl-red);
  position: relative;
}

.devxrl .devxrl-pipe-arrow::after {
  content: '';
  position: absolute;
  bottom: -3px;
  left: 50%;
  transform: translateX(-50%);
  border-left: 3.5px solid transparent;
  border-right: 3.5px solid transparent;
  border-top: 5px solid var(--dxrl-red);
}

.devxrl .devxrl-pipe-arrow-inner {
  width: 1.5px;
  height: 6px;
  background: var(--dxrl-red);
  position: relative;
}

.devxrl .devxrl-pipe-arrow-inner::after {
  content: '';
  position: absolute;
  bottom: -2px;
  left: 50%;
  transform: translateX(-50%);
  border-left: 3px solid transparent;
  border-right: 3px solid transparent;
  border-top: 4px solid var(--dxrl-red);
}

.devxrl .devxrl-pipe-branch {
  display: flex;
  gap: 4px;
  position: relative;
}

.devxrl .devxrl-pipe-branch-item {
  font-size: 0.45rem;
  font-weight: 600;
  padding: 2px 6px;
  border: 1px solid var(--dxrl-red);
  color: var(--dxrl-red);
  text-transform: uppercase;
  border-radius: 2px;
}

.devxrl .devxrl-pipeline-text {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.devxrl .devxrl-pipeline-text h4 {
  font-size: 0.88rem;
  font-weight: 700;
  margin-bottom: 0.3rem;
}

.devxrl .devxrl-pipeline-text p {
  font-size: 0.75rem;
  line-height: 1.6;
  max-width: none;
}

.devxrl .devxrl-pipeline-callout {
  font-size: 0.72rem;
  line-height: 1.6;
  color: var(--dxrl-ink);
  padding: 0.75rem 1rem;
  border-left: 3px solid var(--dxrl-teal);
  background: rgba(77, 179, 168, 0.06);
}

/* SCALE */
.devxrl .devxrl-scale {
  background: var(--dxrl-teal-deep);
  color: #fff;
}

.devxrl .devxrl-scale h3 {
  color: var(--dxrl-teal);
}

.devxrl .devxrl-scale p {
  color: rgba(255, 255, 255, 0.55);
}

.devxrl .devxrl-scale-wrapper {
  margin-top: 1rem;
  width: 100%;
}

.devxrl .devxrl-scale-table {
  width: 100%;
  border-collapse: collapse;
  font-size: 0.68rem;
}

.devxrl .devxrl-scale-table thead th {
  font-size: 0.55rem;
  font-weight: 700;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--dxrl-teal);
  padding: 6px 8px;
  border-bottom: 1.5px solid rgba(255, 255, 255, 0.1);
  text-align: left;
}

.devxrl .devxrl-scale-table thead th:first-child {
  text-align: center;
  width: 44px;
}

.devxrl .devxrl-scale-table tbody tr {
  border-bottom: 1px solid rgba(255, 255, 255, 0.05);
  cursor: pointer;
  transition: all 0.2s ease;
}

.devxrl .devxrl-scale-table tbody tr:hover {
  background: rgba(77, 179, 168, 0.05);
}

.devxrl .devxrl-scale-table tbody tr.highlighted {
  background: rgba(77, 179, 168, 0.1);
}

.devxrl .devxrl-scale-table tbody td {
  padding: 5px 8px;
  vertical-align: top;
  line-height: 1.4;
}

.devxrl .devxrl-scale-table tbody td:first-child {
  text-align: center;
  font-weight: 800;
}

.devxrl .devxrl-scale-level-num {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 26px;
  height: 26px;
  font-size: 0.8rem;
  border-radius: 2px;
}

.devxrl .devxrl-scale-phase {
  font-weight: 600;
  color: #fff;
  white-space: nowrap;
}

.devxrl .devxrl-scale-def {
  color: rgba(255, 255, 255, 0.5);
}

.devxrl .devxrl-scale-product {
  color: rgba(255, 255, 255, 0.35);
  font-size: 0.62rem;
  font-style: italic;
}

.devxrl .devxrl-scale-criteria {
  display: none;
  color: rgba(255, 255, 255, 0.5);
  font-size: 0.62rem;
  padding-top: 4px;
  border-top: 1px solid rgba(255, 255, 255, 0.06);
  margin-top: 4px;
}

.devxrl .devxrl-scale-table tbody tr.highlighted .devxrl-scale-criteria {
  display: block;
}

.devxrl .devxrl-scale-footer {
  margin-top: 1rem;
  font-size: 0.72rem;
  color: rgba(255, 255, 255, 0.4);
  line-height: 1.6;
  max-width: 680px;
}

.devxrl .devxrl-scale-footer strong {
  color: rgba(255, 255, 255, 0.65);
  font-weight: 600;
}

/* JOURNEY */
.devxrl .devxrl-journey {
  background: var(--dxrl-bg);
}

.devxrl .devxrl-graph-container {
  width: 100%;
  max-width: 860px;
  margin-top: 1.25rem;
}

.devxrl .devxrl-graph-canvas {
  width: 100%;
  background: var(--dxrl-paper);
  border: 1px solid var(--dxrl-gray-light);
  border-radius: 2px;
}

.devxrl .devxrl-graph-legend {
  display: flex;
  gap: 1.5rem;
  margin-top: 0.6rem;
  font-size: 0.68rem;
}

.devxrl .devxrl-legend-item {
  display: flex;
  align-items: center;
  gap: 6px;
  color: var(--dxrl-gray);
}

.devxrl .devxrl-legend-swatch {
  width: 20px;
  height: 2px;
  border-radius: 1px;
}

.devxrl .devxrl-graph-toggle {
  display: flex;
  gap: 0.4rem;
  margin-top: 0.8rem;
}

.devxrl .devxrl-graph-toggle button {
  font-family: 'JetBrains Mono', monospace;
  font-size: 0.62rem;
  font-weight: 600;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  padding: 4px 10px;
  border: 1.5px solid var(--dxrl-gray-light);
  background: transparent;
  color: var(--dxrl-gray);
  cursor: pointer;
  transition: all 0.3s ease;
}

.devxrl .devxrl-graph-toggle button.active {
  border-color: var(--dxrl-teal);
  color: var(--dxrl-teal-dark);
  background: rgba(77, 179, 168, 0.08);
}

.devxrl .devxrl-graph-toggle button:hover {
  border-color: var(--dxrl-teal);
  color: var(--dxrl-teal-dark);
}

.devxrl .devxrl-graph-note {
  font-size: 0.62rem;
  color: var(--dxrl-gray);
  margin-top: 0.6rem;
  font-style: italic;
}

/* DELIVERABLES */
.devxrl .devxrl-deliverables {
  background: var(--dxrl-bg-alt);
}

.devxrl .devxrl-deliverables-list {
  margin-top: 1.25rem;
}

.devxrl .devxrl-deliverable-item {
  display: grid;
  grid-template-columns: 28px 1fr;
  gap: 0.75rem;
  align-items: start;
  padding: 0.8rem 0;
  border-bottom: 1px solid var(--dxrl-gray-light);
}

.devxrl .devxrl-deliverable-item:first-child {
  border-top: 1px solid var(--dxrl-gray-light);
}

.devxrl .devxrl-deliverable-num {
  font-size: 0.65rem;
  font-weight: 700;
  color: var(--dxrl-teal-dark);
  padding-top: 1px;
}

.devxrl .devxrl-deliverable-text {
  font-size: 0.78rem;
  color: var(--dxrl-ink);
  line-height: 1.5;
}

.devxrl .devxrl-deliverable-text strong {
  font-weight: 700;
}

/* METHODOLOGY */
.devxrl .devxrl-methodology {
  background: var(--dxrl-bg);
}

.devxrl .devxrl-method-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1.25rem;
  margin-top: 1.25rem;
}

.devxrl .devxrl-method-card {
  background: var(--dxrl-paper);
  border: 1px solid var(--dxrl-gray-light);
  padding: 1rem 1.15rem;
}

.devxrl .devxrl-method-card h4 {
  font-size: 0.78rem;
  font-weight: 700;
  margin-bottom: 0.35rem;
}

.devxrl .devxrl-method-card p {
  font-size: 0.72rem;
  line-height: 1.55;
  max-width: none;
}

.devxrl .devxrl-method-num {
  font-size: 0.6rem;
  font-weight: 700;
  color: var(--dxrl-teal);
  margin-bottom: 0.35rem;
  display: block;
}

/* PROCESS */
.devxrl .devxrl-process {
  background: var(--dxrl-bg-alt);
}

.devxrl .devxrl-process-steps {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 1.25rem;
  margin-top: 1.25rem;
}

.devxrl .devxrl-process-step {
  text-align: center;
  position: relative;
}

.devxrl .devxrl-process-step::after {
  content: '';
  position: absolute;
  top: 20px;
  right: -0.625rem;
  width: 1.25rem;
  height: 1.5px;
  background: var(--dxrl-gray-light);
}

.devxrl .devxrl-process-step:last-child::after {
  display: none;
}

.devxrl .devxrl-step-hex {
  width: 40px;
  height: 40px;
  margin: 0 auto 0.6rem;
}

.devxrl .devxrl-step-hex svg {
  width: 100%;
  height: 100%;
}

.devxrl .devxrl-process-step h4 {
  font-size: 0.78rem;
  font-weight: 700;
  margin-bottom: 0.3rem;
}

.devxrl .devxrl-process-step p {
  font-size: 0.68rem;
  color: #5a6868;
  max-width: none;
  line-height: 1.5;
}

/* TRL */
.devxrl .devxrl-trl {
  background: var(--dxrl-bg-alt);
}

.devxrl .devxrl-trl-layout {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 2rem;
  margin-top: 1.25rem;
  align-items: start;
}

.devxrl .devxrl-trl-text p {
  font-size: 0.78rem;
  line-height: 1.6;
  max-width: none;
}

.devxrl .devxrl-trl-text h4 {
  font-size: 0.88rem;
  font-weight: 700;
  margin-bottom: 0.4rem;
}

.devxrl .devxrl-trl-mini-table {
  width: 100%;
  border-collapse: collapse;
  font-size: 0.65rem;
}

.devxrl .devxrl-trl-mini-table th {
  font-size: 0.55rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--dxrl-teal-dark);
  padding: 5px 6px;
  border-bottom: 1.5px solid var(--dxrl-gray-light);
  text-align: left;
}

.devxrl .devxrl-trl-mini-table th:first-child {
  width: 36px;
  text-align: center;
}

.devxrl .devxrl-trl-mini-table td {
  padding: 4px 6px;
  border-bottom: 1px solid rgba(0, 0, 0, 0.04);
  vertical-align: top;
  line-height: 1.4;
}

.devxrl .devxrl-trl-mini-table td:first-child {
  text-align: center;
  font-weight: 700;
  color: var(--dxrl-teal-dark);
}

.devxrl .devxrl-trl-phase {
  font-weight: 600;
}

.devxrl .devxrl-trl-badges {
  display: flex;
  gap: 0.75rem;
  margin-top: 1.25rem;
  flex-wrap: wrap;
}

.devxrl .devxrl-trl-badge {
  padding: 0.5rem 0.75rem;
  border: 1.5px solid var(--dxrl-gray-light);
  background: var(--dxrl-paper);
  text-align: center;
}

.devxrl .devxrl-trl-badge-label {
  font-size: 0.55rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--dxrl-teal-dark);
  margin-bottom: 2px;
}

.devxrl .devxrl-trl-badge-desc {
  font-size: 0.62rem;
  color: var(--dxrl-gray);
}

/* CTA */
.devxrl .devxrl-cta {
  background: var(--dxrl-teal-deep);
  color: #fff;
}

.devxrl .devxrl-cta h3 {
  color: var(--dxrl-teal);
}

.devxrl .devxrl-cta p {
  color: rgba(255, 255, 255, 0.5);
}

.devxrl .devxrl-cta-row {
  display: flex;
  gap: 1rem;
  margin-top: 1.5rem;
  align-items: center;
}

.devxrl .devxrl-btn {
  font-family: 'JetBrains Mono', monospace;
  font-size: 0.75rem;
  font-weight: 700;
  letter-spacing: 0.05em;
  padding: 10px 20px;
  text-decoration: none;
  display: inline-block;
  cursor: pointer;
  transition: all 0.2s ease;
  border: none;
}

.devxrl .devxrl-btn-primary {
  background: var(--dxrl-teal);
  color: var(--dxrl-teal-deep);
}

.devxrl .devxrl-btn-primary:hover {
  background: var(--dxrl-teal-dark);
}

.devxrl .devxrl-btn-outline {
  background: transparent;
  color: rgba(255, 255, 255, 0.5);
  border: 1.5px solid rgba(255, 255, 255, 0.2);
}

.devxrl .devxrl-btn-outline:hover {
  border-color: var(--dxrl-teal);
  color: var(--dxrl-teal);
}

.devxrl .devxrl-cta-footer {
  margin-top: 2rem;
  padding-top: 1.5rem;
  border-top: 1px solid rgba(255, 255, 255, 0.07);
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
}

/* Animations */
.devxrl .fade-in {
  opacity: 0;
  transform: translateY(14px);
  transition: opacity 0.5s ease, transform 0.5s ease;
}

.devxrl .fade-in.visible {
  opacity: 1;
  transform: translateY(0);
}

.devxrl .stagger > * {
  opacity: 0;
  transform: translateY(10px);
}

.devxrl .stagger.visible > *:nth-child(1) {
  animation: devxrl-fadeUp 0.4s ease forwards 0.05s;
}

.devxrl .stagger.visible > *:nth-child(2) {
  animation: devxrl-fadeUp 0.4s ease forwards 0.1s;
}

.devxrl .stagger.visible > *:nth-child(3) {
  animation: devxrl-fadeUp 0.4s ease forwards 0.15s;
}

.devxrl .stagger.visible > *:nth-child(4) {
  animation: devxrl-fadeUp 0.4s ease forwards 0.2s;
}

.devxrl .stagger.visible > *:nth-child(5) {
  animation: devxrl-fadeUp 0.4s ease forwards 0.25s;
}

.devxrl .stagger.visible > *:nth-child(6) {
  animation: devxrl-fadeUp 0.4s ease forwards 0.3s;
}

@keyframes devxrl-fadeUp {
  to { opacity: 1; transform: translateY(0); }
}

/* Responsive */
@media (max-width: 768px) {
  .devxrl .devxrl-section {
    padding: 1.5rem 1.25rem;
  }
  .devxrl .devxrl-problem-grid,
  .devxrl .devxrl-method-grid {
    grid-template-columns: 1fr;
  }
  .devxrl .devxrl-process-steps {
    grid-template-columns: 1fr 1fr;
    gap: 1.5rem;
  }
  .devxrl .devxrl-process-step::after {
    display: none;
  }
  .devxrl .devxrl-cta-footer {
    flex-direction: column;
    gap: 1rem;
    text-align: center;
  }
  .devxrl-nav-dots {
    right: 0.75rem;
  }
  .devxrl .devxrl-graph-toggle {
    flex-wrap: wrap;
  }
  .devxrl .devxrl-pipeline-layout,
  .devxrl .devxrl-trl-layout {
    grid-template-columns: 1fr;
  }
  .devxrl .devxrl-scale-table {
    font-size: 0.6rem;
  }
  .devxrl .devxrl-scale-table thead th {
    padding: 5px 4px;
    font-size: 0.5rem;
  }
  .devxrl .devxrl-scale-table tbody td {
    padding: 4px;
  }
  .devxrl .devxrl-scale-level-num {
    width: 22px;
    height: 22px;
    font-size: 0.7rem;
  }
}

@media (max-width: 480px) {
  .devxrl .devxrl-process-steps {
    grid-template-columns: 1fr;
  }
  .devxrl .devxrl-cta-row {
    flex-direction: column;
  }
  .devxrl .devxrl-pipe-node {
    min-width: 110px;
    font-size: 0.45rem;
  }
}
</style>
