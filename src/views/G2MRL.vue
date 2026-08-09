<script setup lang="ts">
import { ref, onMounted, onUnmounted, nextTick } from 'vue'

// Section list drives nav dot rendering and IntersectionObserver targeting.
const sectionList = [
  { id: 'hero', label: 'Hero' },
  { id: 'problem', label: 'The Problem' },
  { id: 'pipeline', label: 'GTM Pipeline' },
  { id: 'scale', label: 'G2MRL Scale' },
  { id: 'kb', label: 'Data Foundation' },
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

interface ReadinessBadge {
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

interface DevXrlRow {
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
    title: 'Undefined ICP',
    body: 'You are selling to "everyone." Every deal starts from scratch. Sales cycles are unpredictable because the buyer profile is.',
  },
  {
    title: 'Messaging Dissonance',
    body: 'What marketing says, what the website says, what reps say, and what the product does all tell different stories. Decision makers check all four.',
  },
  {
    title: 'Broken Handoffs',
    body: 'Marketing passes leads sales does not believe in. Sales ignores the CRM. Customer success inherits fires. Nothing compounds across the motion.',
  },
  {
    title: 'Unrepeatable Wins',
    body: 'Your best reps win on hero effort and relationships. Every hire starts over. The motion lives in people, not the system they can hand down.',
  },
  {
    title: 'Invisible Ecosystem',
    body: 'Decision makers commit when internal advocates push. Without a strategy for advocacy and ecosystem, you are selling against gravity the whole way.',
  },
  {
    title: 'GTM Data Chaos',
    body: 'Win stories, objections, competitive intel, and pricing rationale live in decks, threads, and CRMs nobody reads. Sales AI tools cannot score what they cannot find.',
  },
]

const deliverables: Deliverable[] = [
  {
    num: '01',
    title: 'ICP & Positioning Audit',
    body: 'Buyer profiles tested against your win/loss data. Message clarity scored across website, sales decks, and rep talk tracks. Gaps identified and rewritten.',
  },
  {
    num: '02',
    title: 'Motion Map',
    body: 'Full walkthrough of your GTM motion from first touch through expansion. Every handoff documented with failure modes, leverage points, and owner ambiguity flagged.',
  },
  {
    num: '03',
    title: 'Pipeline & Conversion Analysis',
    body: 'Funnel stages modeled with real data. Where deals stall, where spend is wasted, where hero effort is masking systemic gaps. Scored 1-9 per stage.',
  },
  {
    num: '04',
    title: 'Ecosystem & Partnership Assessment',
    body: 'Current partnerships evaluated for economic and strategic return. Where ecosystem leverage is real pipeline and where it is theater.',
  },
  {
    num: '05',
    title: 'Tiered 12-Month Roadmap',
    body: 'Prioritized by effort and impact. Quick wins, structural fixes, and long-term investments, sized to your actual team and budget.',
  },
  {
    num: '06',
    title: 'GTM Intelligence Blueprint',
    body: 'Structured plan for your GTM knowledge base. What sales, customer, and competitive data exists, what needs capturing, and how it connects to your sales AI stack. First seed of an ongoing KB that devEco™ builds and maintains.',
  },
  {
    num: '07',
    title: 'Leadership Coaching',
    body: 'Sessions with founders, sales leaders, and GTM operators. Paired with coaching from practitioners who have built GTM motions inside Arm, Linaro, AWS, and as founders of dev-tool companies.',
  },
]

const methodologyCards: MethodCard[] = [
  {
    letter: 'A',
    title: 'ICP & Positioning Analysis',
    body: 'Who you sell to and what you say to them, tested against market reality. Most growth-stage companies position for the buyer they wish existed, not the one their pipeline actually closes.',
  },
  {
    letter: 'B',
    title: 'Demand & Conversion Motion Audit',
    body: 'How qualified interest becomes committed pipeline and revenue. Every handoff, every stage transition, every metric that tells a real story instead of a flattering one.',
  },
  {
    letter: 'C',
    title: 'G2MRL Readiness Scoring',
    body: 'Proprietary metric evaluating positioning, pipeline, ecosystem leverage, revenue intelligence, and motion scalability. Scored 1-9 across dimensions to identify where focus will compound.',
  },
  {
    letter: 'D',
    title: 'Ecosystem & Revenue Intelligence',
    body: 'How partnerships, advocates, and sales data compound. GTM knowledge that feeds modern sales AI and separates repeatable motion from tribal knowledge stuck in three people\'s heads.',
  },
]

const readinessBadges: ReadinessBadge[] = [
  { label: 'DevXRL', desc: 'Developer Experience' },
  { label: 'SMRL', desc: 'Social Media' },
  { label: 'TRL', desc: 'Technology Maturity' },
]

const scaleData: ScaleRow[] = [
  { level: 9, phase: 'Category-Defining', def: 'Your GTM sets the industry benchmark. Analysts cite your motion. Competitors mirror your playbook.', criteria: 'Category narrative belongs to you. Customers buy before evaluating alternatives. Ecosystem compounds without direct GTM spend. Your sales data trains the AI tools the industry uses.', product: 'Market Maker', numStyle: 'background:rgba(77,179,168,0.25);color:var(--g2mrl-teal)' },
  { level: 8, phase: 'Dominant', def: 'You win the deals you target. Ecosystem effects are real and defensible.', criteria: 'High win rates in ICP segments. Measurable pipeline from partners and advocates. Sales AI tools trained on proprietary GTM data augment every rep. Forecasts near deterministic.', product: 'Category Leader', numStyle: 'background:rgba(77,179,168,0.2);color:var(--g2mrl-teal)' },
  { level: 7, phase: 'Scalable', def: 'Motion extends across segments, regions, and products without breaking.', criteria: 'Each new rep yields predictable contribution. Partnerships multiply direct motion. Expansion revenue exceeds new logo revenue. Motion scales faster than headcount.', product: 'Expanding', numStyle: 'background:rgba(77,179,168,0.16);color:var(--g2mrl-teal)' },
  { level: 6, phase: 'Repeatable', def: 'Documented motion. New reps ramp in weeks. Forecasts track reality.', criteria: 'Playbooks exist and are used. Handoffs measurable. Forecast accuracy within 10%. Wins attributable to system, not heroics. Data captured systematically.', product: 'Predictable', numStyle: 'background:rgba(77,179,168,0.12);color:var(--g2mrl-teal)' },
  { level: 5, phase: 'Aligned', def: 'Marketing, sales, and partnerships share the ICP, the message, and the handoff.', criteria: 'Single source of truth on who the buyer is. Consistent message across website, decks, and talk tracks. Predictable pipeline from multiple sources. Reporting tells a coherent story.', product: 'Coordinated', numStyle: 'background:rgba(212,168,67,0.2);color:var(--g2mrl-gold)' },
  { level: 4, phase: 'Functional', def: 'ICP is defined. Basic sales playbook exists. Some repeatability emerging.', criteria: 'Pipeline forecastable within wide bands. Marketing generates leads sales sometimes accepts. Deals close through mix of system and hero effort.', product: 'Operational', numStyle: 'background:rgba(212,168,67,0.15);color:var(--g2mrl-gold)' },
  { level: 3, phase: 'Leaking', def: 'Spend is happening but conversion is low. Funnel has holes at handoffs.', criteria: 'CAC rising, win rates falling. Deals won on hero effort, not systemic strength. Data fragmented across tools and teams. Reporting contradicts itself.', product: 'Inefficient', numStyle: 'background:rgba(196,91,91,0.2);color:var(--g2mrl-red)' },
  { level: 2, phase: 'Reactive', def: 'Activity without plan. Sales responds to whoever shows up. Marketing improvises.', criteria: 'Many channels, no measurement, no qualification. Team burns out chasing unqualified leads. No ICP discipline. Strategy changes with every leadership mood.', product: 'Ad Hoc', numStyle: 'background:rgba(196,91,91,0.15);color:var(--g2mrl-red)' },
  { level: 1, phase: 'Undefined', def: 'No GTM strategy. Hoping for inbound. Sales reactive to whoever shows up.', criteria: 'No ICP, no message discipline, no funnel. Founders close every deal. Partnerships are LinkedIn logos, not pipeline. No instrumentation.', product: 'Dormant', numStyle: 'background:rgba(196,91,91,0.1);color:var(--g2mrl-red)' },
]

const devxrlData: DevXrlRow[] = [
  { level: 9, phase: 'Transformative', status: 'Paradigm shift. Redefines how developers think and work.' },
  { level: 8, phase: 'Magical', status: 'Experience feels effortless. Developers focus on goals.' },
  { level: 7, phase: 'Empowering', status: 'Accelerates productivity. Technology feels like an ally.' },
  { level: 6, phase: 'Predictable', status: 'System behaves consistently. Trust is earned.' },
  { level: 5, phase: 'Seamless', status: 'Zero to running code with minimal effort.' },
  { level: 4, phase: 'Working', status: 'Basics succeed. Adoptable with patience.' },
  { level: 3, phase: 'Confusing', status: 'Signals are mixed. Never sure if doing it right.' },
  { level: 2, phase: 'Frustrating', status: 'No progress without brute force.' },
  { level: 1, phase: 'Broken', status: 'Unusable. Developers cannot get started.' },
]

const processSteps: ProcessStep[] = [
  { num: 1, title: 'Discovery', desc: 'We analyze your market, ICP, motion, and current pipeline to understand what you actually need.' },
  { num: 2, title: 'Audit', desc: 'Deep evaluation using G2MRL scoring, motion mapping, and pipeline analysis across every stage from first touch to expansion.' },
  { num: 3, title: 'Deliver', desc: 'Complete report with scored findings, motion redesigns, and a tiered 12-month roadmap sized to your team and budget.' },
  { num: 4, title: 'Support', desc: 'Optional follow-on for implementation. Motion that grows, not dependencies that require us forever.' },
]

const upstreamPills = ['Win Stories', 'Objections', 'Competitive Intel', 'Pricing Signals']
const downstreamPills = ['Sales AI Tools', 'Rep Onboarding', 'Forecast Scoring', 'CS Playbooks']

// Nav dots + section tracking
const sectionEls = ref<HTMLElement[]>([])
const activeSectionIndex = ref(0)

function collectSections() {
  sectionEls.value = Array.from(
    document.querySelectorAll<HTMLElement>('.g2mrl .g2mrl-section')
  )
}

function scrollToSection(index: number) {
  sectionEls.value[index]?.scrollIntoView({ behavior: 'smooth' })
}

// Observers
let sectionObserver: IntersectionObserver | null = null
let fadeObserver: IntersectionObserver | null = null

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
    .querySelectorAll<HTMLElement>('.g2mrl .fade-in')
    .forEach((el) => fadeObserver!.observe(el))
}

// Scale table toggle (single row expand)
const highlightedLevel = ref<number | null>(null)
function toggleLevel(level: number) {
  highlightedLevel.value = highlightedLevel.value === level ? null : level
}

onMounted(async () => {
  document.documentElement.classList.add('g2mrl-page')
  await nextTick()
  collectSections()
  setupObservers()
})

onUnmounted(() => {
  document.documentElement.classList.remove('g2mrl-page')
  sectionObserver?.disconnect()
  fadeObserver?.disconnect()
})
</script>

<template>
  <div class="g2mrl">
    <!-- Nav dots -->
    <nav class="g2mrl-nav-dots" aria-label="Section navigation">
      <button
        v-for="(section, i) in sectionList"
        :key="section.id"
        type="button"
        class="g2mrl-nav-dot"
        :class="{ active: activeSectionIndex === i }"
        :aria-label="`Jump to ${section.label}`"
        @click="scrollToSection(i)"
      ></button>
    </nav>

    <!-- HERO -->
    <section id="hero" class="g2mrl-section g2mrl-hero">
      <div class="g2mrl-container">
        <div class="g2mrl-hero-logo">
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
        <h1 class="g2mrl-hero-title">G2M<span class="g2mrl-accent">RL</span> Audit</h1>
        <p class="g2mrl-hero-sub">Go-to-Market Readiness Levels</p>
        <p class="g2mrl-hero-tagline">
          A structured evaluation of your go-to-market motion, from ICP through expansion. Where
          pipeline leaks, where ecosystems fail to form, and how to build the top-down motion that
          meets the bottom-up advocacy you need to win.
        </p>
      </div>
      <div class="g2mrl-scroll-hint">
        <svg viewBox="0 0 24 24">
          <path d="M12 5v14M19 12l-7 7-7-7" />
        </svg>
        <span class="g2mrl-small">scroll</span>
      </div>
    </section>

    <!-- PROBLEM -->
    <section id="problem" class="g2mrl-section g2mrl-problem">
      <div class="g2mrl-container">
        <span class="g2mrl-tag g2mrl-tag-red">THE PROBLEM</span>
        <h2>Pipeline Without Motion Is Just Hope</h2>
        <p>
          Most growth-stage companies have activity without alignment. Marketing produces leads
          sales does not trust. Sales wins deals customer success cannot keep. Partnerships exist
          on slides, not in pipeline. Decision makers feel the incoherence and hesitate.
        </p>
        <div class="g2mrl-problem-grid fade-in">
          <div class="g2mrl-problem-card">
            <div class="g2mrl-icon-box">
              <svg viewBox="0 0 24 24">
                <circle cx="12" cy="12" r="10" stroke-dasharray="3 2" />
                <circle cx="12" cy="12" r="6" stroke-dasharray="2 2" />
                <circle cx="12" cy="12" r="2" />
              </svg>
            </div>
            <h4>{{ problemCards[0]!.title }}</h4>
            <p>{{ problemCards[0]!.body }}</p>
          </div>
          <div class="g2mrl-problem-card">
            <div class="g2mrl-icon-box">
              <svg viewBox="0 0 24 24">
                <polyline points="2 12 6 8 10 14 14 6 18 12 22 8" />
              </svg>
            </div>
            <h4>{{ problemCards[1]!.title }}</h4>
            <p>{{ problemCards[1]!.body }}</p>
          </div>
          <div class="g2mrl-problem-card">
            <div class="g2mrl-icon-box">
              <svg viewBox="0 0 24 24">
                <line x1="3" y1="12" x2="9" y2="12" />
                <polyline points="7 9 10 12 7 15" />
                <line x1="13" y1="12" x2="15" y2="12" />
                <line x1="18" y1="12" x2="21" y2="12" />
              </svg>
            </div>
            <h4>{{ problemCards[2]!.title }}</h4>
            <p>{{ problemCards[2]!.body }}</p>
          </div>
          <div class="g2mrl-problem-card">
            <div class="g2mrl-icon-box">
              <svg viewBox="0 0 24 24">
                <path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2" />
                <circle cx="12" cy="7" r="4" />
              </svg>
            </div>
            <h4>{{ problemCards[3]!.title }}</h4>
            <p>{{ problemCards[3]!.body }}</p>
          </div>
          <div class="g2mrl-problem-card">
            <div class="g2mrl-icon-box">
              <svg viewBox="0 0 24 24">
                <circle cx="12" cy="12" r="3" />
                <circle cx="5" cy="5" r="2" stroke-dasharray="2 2" />
                <circle cx="19" cy="5" r="2" />
                <circle cx="5" cy="19" r="2" />
                <circle cx="19" cy="19" r="2" stroke-dasharray="2 2" />
                <line x1="7" y1="7" x2="10" y2="10" stroke-dasharray="2 2" />
                <line x1="14" y1="14" x2="17" y2="17" />
              </svg>
            </div>
            <h4>{{ problemCards[4]!.title }}</h4>
            <p>{{ problemCards[4]!.body }}</p>
          </div>
          <div class="g2mrl-problem-card">
            <div class="g2mrl-icon-box">
              <svg viewBox="0 0 24 24">
                <circle cx="5" cy="5" r="1.5" fill="currentColor" />
                <circle cx="11" cy="3" r="1" fill="currentColor" />
                <circle cx="19" cy="6" r="1.5" fill="currentColor" />
                <circle cx="7" cy="11" r="1" fill="currentColor" />
                <circle cx="15" cy="10" r="1.5" fill="currentColor" />
                <circle cx="4" cy="17" r="1.5" fill="currentColor" />
                <circle cx="12" cy="16" r="1" fill="currentColor" />
                <circle cx="20" cy="14" r="1" fill="currentColor" />
                <circle cx="16" cy="20" r="1.5" fill="currentColor" />
              </svg>
            </div>
            <h4>{{ problemCards[5]!.title }}</h4>
            <p>{{ problemCards[5]!.body }}</p>
          </div>
        </div>
      </div>
    </section>

    <!-- PIPELINE -->
    <section id="pipeline" class="g2mrl-section g2mrl-pipeline">
      <div class="g2mrl-container">
        <span class="g2mrl-tag g2mrl-tag-gold">THE GTM PIPELINE</span>
        <h2>Motion Over Activity</h2>
        <div class="g2mrl-pipeline-layout fade-in">
          <div class="g2mrl-pipeline-diagram">
            <div class="g2mrl-pipe-node g2mrl-pipe-rect-wide">TARGET MARKET</div>
            <div class="g2mrl-pipe-arrow"></div>
            <div class="g2mrl-pipe-node g2mrl-pipe-rect">ICP DEFINITION</div>
            <div class="g2mrl-pipe-arrow"></div>
            <div class="g2mrl-pipe-node g2mrl-pipe-rect">POSITIONING &amp; MESSAGE</div>
            <div class="g2mrl-pipe-arrow"></div>
            <div class="g2mrl-pipe-group-gold">
              <div class="g2mrl-pipe-node g2mrl-pipe-inner">DEMAND GENERATION</div>
              <div class="g2mrl-pipe-label">ACQUISITION FUNNEL</div>
              <div class="g2mrl-pipe-arrow-inner"></div>
              <div class="g2mrl-pipe-node g2mrl-pipe-inner">QUALIFICATION</div>
            </div>
            <div class="g2mrl-pipe-arrow"></div>
            <div class="g2mrl-pipe-node g2mrl-pipe-rect">OPPORTUNITY</div>
            <div class="g2mrl-pipe-arrow"></div>
            <div class="g2mrl-pipe-branch">
              <div class="g2mrl-pipe-branch-item">Direct</div>
              <div class="g2mrl-pipe-branch-item">Channel</div>
              <div class="g2mrl-pipe-branch-item">Ecosystem</div>
            </div>
            <div class="g2mrl-pipe-arrow"></div>
            <div class="g2mrl-pipe-group-teal">
              <div class="g2mrl-pipe-node g2mrl-pipe-inner-teal">COMMIT / CLOSE</div>
              <div class="g2mrl-pipe-arrow-inner"></div>
              <div class="g2mrl-pipe-node g2mrl-pipe-inner-teal">EXPAND</div>
              <div class="g2mrl-pipe-label">REVENUE FUNNEL</div>
              <div class="g2mrl-pipe-arrow-inner"></div>
              <div class="g2mrl-pipe-node g2mrl-pipe-inner-teal">ADVOCATE</div>
            </div>
          </div>
          <div class="g2mrl-pipeline-text">
            <div>
              <h4>Where Motion Breaks</h4>
              <p>
                GTM motion breaks at handoffs. Marketing, sales, partnerships, and customer success
                each optimize their own piece while the buyer experiences incoherence. Decision
                makers shop for alignment. They buy from the company that looks like one thing, not
                four.
              </p>
            </div>
            <div>
              <h4>What We Do About It</h4>
              <p>
                devEco™ audits every handoff in the motion, from first touch through expansion. Lead
                quality, sales acceptance, opportunity progression, contract terms, onboarding,
                expansion triggers. We rebuild the connective tissue so the motion scales without
                adding bodies at every gap.
              </p>
            </div>
            <div class="g2mrl-pipeline-callout">
              Top-down motion needs bottom-up advocacy to win ecosystems. G2MRL maps the top.
              DevXRL maps the bottom. Together they close the loop on adoption.
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- SCALE -->
    <section id="scale" class="g2mrl-section g2mrl-scale">
      <div class="g2mrl-container">
        <span class="g2mrl-tag g2mrl-tag-teal" style="color:var(--g2mrl-teal);border-color:var(--g2mrl-teal)">THE FRAMEWORK</span>
        <h2 style="color:#fff">G2MRL Readiness Levels</h2>
        <p>Where does your GTM motion sit? Click any level to see the evaluation criteria.</p>
        <div class="g2mrl-scale-wrapper fade-in">
          <table class="g2mrl-scale-table">
            <thead>
              <tr>
                <th></th>
                <th>Phase</th>
                <th>Definition</th>
                <th>GTM Status</th>
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
                  <span class="g2mrl-scale-level-num" :style="row.numStyle">{{ row.level }}</span>
                </td>
                <td class="g2mrl-scale-phase">{{ row.phase }}</td>
                <td class="g2mrl-scale-def">
                  {{ row.def }}
                  <div class="g2mrl-scale-criteria">{{ row.criteria }}</div>
                </td>
                <td class="g2mrl-scale-product">{{ row.product }}</td>
              </tr>
            </tbody>
          </table>
        </div>
        <div class="g2mrl-scale-footer">
          Most growth-stage companies sit between level 3 and level 5. The friction is rarely
          talent. It is the motion itself, how the pieces connect, how data moves, how decision
          makers meet advocates.
          <strong>G2MRL shows where your motion sits and what it takes to build a GTM that compounds into ecosystem.</strong>
          We close the gaps between the teams that are already trying.
        </div>
      </div>
    </section>

    <!-- KB / DATA FOUNDATION -->
    <section id="kb" class="g2mrl-section g2mrl-kb">
      <div class="g2mrl-container">
        <span class="g2mrl-tag g2mrl-tag-gold">THE DATA FOUNDATION</span>
        <h2>Your GTM Runs on Data You Do Not Have</h2>
        <p>
          Modern sales AI scores, forecasts, and coaches based on the data you feed it. Most GTM
          teams feed it noise: scattered CRM notes, tribal knowledge, and whatever the last rep
          typed at 5pm on Friday. The result is AI that guesses as badly as a new hire, and a
          motion that never compounds.
        </p>
        <div class="g2mrl-kb-layout fade-in">
          <div class="g2mrl-kb-text">
            <div>
              <h4>The Audit Surfaces What Is Hidden</h4>
              <p>
                G2MRL surfaces every piece of GTM intelligence your organization has scattered
                across CRMs, decks, Slack, and people's heads. Win stories, objection patterns,
                competitive edge cases, pricing rationale, successful onboarding paths, expansion
                signals. Almost none of it is useless. Almost none of it is accessible.
              </p>
            </div>
            <div>
              <h4>devEco™ Builds the Intelligence Layer</h4>
              <p>
                From there, we build and maintain a structured GTM knowledge base that feeds your
                sales AI stack. Lead scoring with real criteria. Rep onboarding that graduates
                humans in weeks. Forecasts that trust the underlying data. Customer success
                playbooks that catch churn before it hits the scorecard. White-glove, maintained by
                people who speak GTM.
              </p>
            </div>
            <div class="g2mrl-kb-callout">
              Sales AI is only as smart as the ground truth you feed it. G2MRL is how you build the
              ground truth before you scale the tools.
            </div>
          </div>
          <div class="g2mrl-kb-diagram">
            <div class="g2mrl-kb-tier-label">Upstream Sources</div>
            <div class="g2mrl-kb-pills">
              <span v-for="pill in upstreamPills" :key="pill" class="g2mrl-kb-pill">{{ pill }}</span>
            </div>
            <div class="g2mrl-kb-arrow-v"></div>
            <div class="g2mrl-kb-core">
              <div class="g2mrl-kb-core-title">GTM INTELLIGENCE</div>
              <div class="g2mrl-kb-core-sub">Source of Truth</div>
              <div class="g2mrl-kb-core-meta">Built and Maintained by devEco™</div>
            </div>
            <div class="g2mrl-kb-arrow-v"></div>
            <div class="g2mrl-kb-tier-label">Downstream Surfaces</div>
            <div class="g2mrl-kb-pills">
              <span
                v-for="pill in downstreamPills"
                :key="pill"
                class="g2mrl-kb-pill g2mrl-kb-pill-out"
              >{{ pill }}</span>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- DELIVERABLES -->
    <section id="deliverables" class="g2mrl-section g2mrl-deliverables">
      <div class="g2mrl-container">
        <span class="g2mrl-tag g2mrl-tag-teal">WHAT YOU GET</span>
        <h2>Audit Deliverables</h2>
        <p>
          Concrete outputs. Not a 60-slide strategy deck, not a hypothetical org chart, not
          benchmarks disconnected from your actual motion.
        </p>
        <div class="g2mrl-deliverables-list stagger fade-in">
          <div
            v-for="item in deliverables"
            :key="item.num"
            class="g2mrl-deliverable-item"
          >
            <span class="g2mrl-deliverable-num">{{ item.num }}</span>
            <span class="g2mrl-deliverable-text">
              <strong>{{ item.title }}</strong> : {{ item.body }}
            </span>
          </div>
        </div>
      </div>
    </section>

    <!-- METHODOLOGY -->
    <section id="methodology" class="g2mrl-section g2mrl-methodology">
      <div class="g2mrl-container">
        <span class="g2mrl-tag g2mrl-tag-gold">METHODOLOGY</span>
        <h2>How We Evaluate</h2>
        <p>
          Frameworks applied by practitioners who have built go-to-market motions inside Arm,
          Linaro, AWS, and as founders of dev-tool companies.
        </p>
        <div class="g2mrl-method-grid fade-in">
          <div
            v-for="card in methodologyCards"
            :key="card.letter"
            class="g2mrl-method-card"
          >
            <span class="g2mrl-method-num">{{ card.letter }}</span>
            <h4>{{ card.title }}</h4>
            <p>{{ card.body }}</p>
          </div>
        </div>
      </div>
    </section>

    <!-- PROCESS -->
    <section id="process" class="g2mrl-section g2mrl-process">
      <div class="g2mrl-container">
        <span class="g2mrl-tag g2mrl-tag-teal">OUR PROCESS</span>
        <h2>From Discovery to Delivery</h2>
        <p>Embedded partner, not external auditor. We work alongside your team.</p>
        <div class="g2mrl-process-steps fade-in">
          <div
            v-for="step in processSteps"
            :key="step.num"
            class="g2mrl-process-step"
          >
            <div class="g2mrl-step-hex">
              <svg viewBox="0 0 56 56">
                <polygon
                  points="28,2 52,15 52,41 28,54 4,41 4,15"
                  fill="var(--g2mrl-teal)"
                  stroke="var(--g2mrl-teal-deep)"
                  stroke-width="1.5"
                />
                <text
                  x="28"
                  y="33"
                  text-anchor="middle"
                  font-family="'JetBrains Mono',monospace"
                  font-size="18"
                  font-weight="700"
                  fill="var(--g2mrl-teal-deep)"
                >{{ step.num }}</text>
              </svg>
            </div>
            <h4>{{ step.title }}</h4>
            <p>{{ step.desc }}</p>
          </div>
        </div>
      </div>
    </section>

    <!-- WHAT COMES AFTER -->
    <section id="trl" class="g2mrl-section g2mrl-trl">
      <div class="g2mrl-container">
        <span class="g2mrl-tag g2mrl-tag-gold">WHAT COMES NEXT</span>
        <h2>Additional Readiness Assessments</h2>
        <p>
          G2MRL covers the top-down motion. For a complete view of your organization's readiness to
          drive adoption, we offer three additional frameworks as separate engagements.
        </p>
        <div class="g2mrl-trl-layout fade-in">
          <div class="g2mrl-trl-text">
            <h4>Developer Experience Readiness (DevXRL)</h4>
            <p>
              The natural complement to G2MRL. Where G2MRL maps the top-down motion that earns
              decision maker commitment, DevXRL maps the bottom-up motion that creates internal
              advocates. Together they close the loop on adoption: deals get signed because
              builders already believe.
            </p>
            <table class="g2mrl-trl-mini-table" style="margin-top:0.75rem">
              <thead>
                <tr>
                  <th>Lvl</th>
                  <th>Phase</th>
                  <th>Status</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="row in devxrlData" :key="row.level">
                  <td>{{ row.level }}</td>
                  <td class="g2mrl-trl-phase">{{ row.phase }}</td>
                  <td>{{ row.status }}</td>
                </tr>
              </tbody>
            </table>
          </div>
          <div class="g2mrl-trl-text">
            <h4>Social Media Readiness (SMRL)</h4>
            <p>
              Evaluates your brand's social presence, platform fit, and audience leverage. Where
              effort is wasted, where attention is leaking, and how to plant the source of truth
              your brand needs to thrive in the AI era. Often the top-of-funnel complement to
              G2MRL when the motion starts with brand awareness.
            </p>
            <h4 class="g2mrl-sub">Technology Readiness Levels (TRL)</h4>
            <p>
              Adapted from NASA and widely used in hardware and deep-tech, TRL provides a clear
              path from fundamental research to market-ready product. We map your technology's
              maturity and build a stage-by-stage roadmap with focused time-to-market objectives.
            </p>
            <p style="margin-top:0.75rem">
              GTM can be mature while developer experience lags behind. Technology can be ready
              while social presence is absent. These gaps are where adoption stalls. By combining
              frameworks, we give you a complete picture of readiness across every surface that
              matters.
            </p>
            <div class="g2mrl-trl-badges">
              <div
                v-for="badge in readinessBadges"
                :key="badge.label"
                class="g2mrl-trl-badge"
              >
                <div class="g2mrl-trl-badge-label">{{ badge.label }}</div>
                <div class="g2mrl-trl-badge-desc">{{ badge.desc }}</div>
              </div>
            </div>
            <p style="margin-top:0.75rem;font-size:0.72rem;color:var(--g2mrl-gray)">
              Each uses a 1-9 scale. These are separate engagements we recommend after G2MRL
              surfaces where your organization needs to focus next.
            </p>
          </div>
        </div>
      </div>
    </section>

    <!-- CTA -->
    <section id="cta" class="g2mrl-section g2mrl-cta">
      <div class="g2mrl-container">
        <span class="g2mrl-tag g2mrl-tag-teal" style="color:var(--g2mrl-teal);border-color:var(--g2mrl-teal)">GET STARTED</span>
        <h2 style="color:#fff">Your GTM should build an ecosystem, not just chase pipeline.</h2>
        <p>
          This is our general offering. Upon discussion we tailor the audit to your specific
          motion, market, team, and stage of growth.
        </p>
        <div class="g2mrl-cta-row">
          <a href="mailto:hello@thedeveco.com" class="g2mrl-btn g2mrl-btn-primary">Contact Us</a>
          <a
            href="https://thedeveco.com"
            target="_blank"
            rel="noopener noreferrer"
            class="g2mrl-btn g2mrl-btn-outline"
          >thedeveco.com</a>
        </div>
        <div class="g2mrl-cta-footer">
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
          <span class="g2mrl-small" style="color:rgba(255,255,255,0.25)">Est. 2024</span>
        </div>
      </div>
    </section>
  </div>
</template>

<style>
/* Scroll-snap applies only when this page is mounted. */
html.g2mrl-page {
  scroll-snap-type: y mandatory;
  scroll-behavior: smooth;
}

/* Noise overlay scoped to the G2MRL page only. */
html.g2mrl-page body::after {
  content: '';
  position: fixed;
  inset: 0;
  pointer-events: none;
  z-index: 9999;
  opacity: 0.025;
  background: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
}

/* Everything below is namespaced under .g2mrl to prevent leakage. */
.g2mrl {
  /* Local palette (prefixed to avoid collision with global tokens in App.vue) */
  --g2mrl-bg: #f0ede4;
  --g2mrl-bg-alt: #e8e5dc;
  --g2mrl-ink: #1e3a3a;
  --g2mrl-teal: #4db3a8;
  --g2mrl-teal-dark: #3d9991;
  --g2mrl-teal-deep: #1e3a3a;
  --g2mrl-gold: #d4a843;
  --g2mrl-gold-dim: #c49a3a;
  --g2mrl-paper: #f4f1e8;
  --g2mrl-red: #c45b5b;
  --g2mrl-gray: #8a9090;
  --g2mrl-gray-light: #c8c5bc;

  font-family: 'JetBrains Mono', monospace;
  background: var(--g2mrl-bg);
  color: var(--g2mrl-ink);
  line-height: 1.6;
  overflow-x: hidden;
}

.g2mrl *,
.g2mrl *::before,
.g2mrl *::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.g2mrl .g2mrl-section {
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

.g2mrl .g2mrl-container {
  max-width: 960px;
  width: 100%;
}

/* Nav dots */
.g2mrl-nav-dots {
  position: fixed;
  right: 1.5rem;
  top: 50%;
  transform: translateY(-50%);
  z-index: 100;
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.g2mrl-nav-dot {
  width: 8px;
  height: 8px;
  border: 1.5px solid var(--g2mrl-teal-dark, #3d9991);
  border-radius: 50%;
  background: transparent;
  cursor: pointer;
  transition: all 0.3s ease;
  padding: 0;
}

.g2mrl-nav-dot.active {
  background: var(--g2mrl-teal, #4db3a8);
  border-color: var(--g2mrl-teal, #4db3a8);
  transform: scale(1.4);
}

.g2mrl-nav-dot:hover {
  border-color: var(--g2mrl-teal, #4db3a8);
}

/* Typography */
.g2mrl h1 {
  font-size: clamp(2rem, 5vw, 3.4rem);
  font-weight: 800;
  letter-spacing: -0.02em;
  line-height: 1.1;
}

.g2mrl h2 {
  font-size: clamp(1.3rem, 2.8vw, 1.8rem);
  font-weight: 700;
  line-height: 1.25;
  margin-bottom: 0.6rem;
}

.g2mrl h3 {
  font-size: 0.7rem;
  font-weight: 600;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: var(--g2mrl-teal-dark);
  margin-bottom: 0.5rem;
}

.g2mrl p {
  font-size: 0.82rem;
  font-weight: 400;
  max-width: 580px;
  color: #4a5555;
  line-height: 1.6;
}

.g2mrl .g2mrl-small {
  font-size: 0.7rem;
  color: var(--g2mrl-gray);
}

/* Tags */
.g2mrl .g2mrl-tag {
  display: inline-block;
  font-size: 0.6rem;
  font-weight: 600;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  padding: 2px 8px;
  border: 1.5px dashed;
  margin-bottom: 0.75rem;
}

.g2mrl .g2mrl-tag-teal {
  color: var(--g2mrl-teal-dark);
  border-color: var(--g2mrl-teal);
}

.g2mrl .g2mrl-tag-gold {
  color: var(--g2mrl-gold-dim);
  border-color: var(--g2mrl-gold);
}

.g2mrl .g2mrl-tag-red {
  color: var(--g2mrl-red);
  border-color: var(--g2mrl-red);
}

/* HERO */
.g2mrl .g2mrl-hero {
  background: var(--g2mrl-teal-deep);
  color: #fff;
}

.g2mrl .g2mrl-hero p {
  color: rgba(255, 255, 255, 0.6);
}

.g2mrl .g2mrl-hero h3 {
  color: var(--g2mrl-teal);
}

.g2mrl .g2mrl-hero-logo {
  margin-bottom: 2rem;
}

.g2mrl .g2mrl-hero-title {
  color: #fff;
  margin-bottom: 0.4rem;
}

.g2mrl .g2mrl-accent {
  color: var(--g2mrl-teal);
}

.g2mrl .g2mrl-hero-sub {
  font-size: clamp(0.95rem, 2vw, 1.2rem);
  font-weight: 400;
  color: rgba(255, 255, 255, 0.45);
  margin-bottom: 1.5rem;
}

.g2mrl .g2mrl-hero-tagline {
  font-size: 0.8rem;
  color: rgba(255, 255, 255, 0.5);
  max-width: 520px;
  line-height: 1.65;
}

.g2mrl .g2mrl-scroll-hint {
  position: absolute;
  bottom: 1.5rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 4px;
  color: rgba(255, 255, 255, 0.25);
  animation: g2mrl-nudge 2s ease infinite;
}

.g2mrl .g2mrl-scroll-hint svg {
  width: 18px;
  height: 18px;
  stroke: currentColor;
  fill: none;
  stroke-width: 2;
  stroke-linecap: round;
  stroke-linejoin: round;
}

@keyframes g2mrl-nudge {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(5px); }
}

/* PROBLEM */
.g2mrl .g2mrl-problem {
  background: var(--g2mrl-bg);
}

.g2mrl .g2mrl-problem-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1.25rem;
  margin-top: 1.25rem;
}

.g2mrl .g2mrl-problem-card {
  background: var(--g2mrl-paper);
  border: 1px solid var(--g2mrl-gray-light);
  padding: 1rem 1.15rem;
}

.g2mrl .g2mrl-problem-card h4 {
  font-size: 0.78rem;
  font-weight: 700;
  margin-bottom: 0.4rem;
}

.g2mrl .g2mrl-problem-card p {
  font-size: 0.72rem;
  line-height: 1.55;
}

.g2mrl .g2mrl-icon-box {
  width: 28px;
  height: 28px;
  margin-bottom: 0.6rem;
  color: var(--g2mrl-teal-dark);
}

.g2mrl .g2mrl-icon-box svg {
  width: 100%;
  height: 100%;
  stroke: currentColor;
  fill: none;
  stroke-width: 1.5;
  stroke-linecap: round;
  stroke-linejoin: round;
}

/* PIPELINE */
.g2mrl .g2mrl-pipeline {
  background: var(--g2mrl-bg-alt);
}

.g2mrl .g2mrl-pipeline-layout {
  display: grid;
  grid-template-columns: 240px 1fr;
  gap: 2.5rem;
  margin-top: 1.25rem;
  align-items: start;
}

.g2mrl .g2mrl-pipeline-diagram {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.g2mrl .g2mrl-pipe-node {
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

.g2mrl .g2mrl-pipe-rect {
  background: var(--g2mrl-paper);
  border: 1.5px solid var(--g2mrl-gray-light);
  color: var(--g2mrl-ink);
  border-radius: 3px;
}

.g2mrl .g2mrl-pipe-rect-wide {
  background: var(--g2mrl-paper);
  border: 1.5px solid var(--g2mrl-gray-light);
  color: var(--g2mrl-ink);
  border-radius: 3px;
  min-width: 200px;
  padding: 14px 12px;
}

.g2mrl .g2mrl-pipe-inner {
  padding: 4px 10px;
  background: rgba(255, 255, 255, 0.45);
  border-radius: 3px;
  border: 1px solid rgba(255, 255, 255, 0.3);
}

.g2mrl .g2mrl-pipe-inner-teal {
  padding: 4px 10px;
  background: rgba(255, 255, 255, 0.2);
  border-radius: 3px;
  border: 1px solid rgba(255, 255, 255, 0.15);
}

.g2mrl .g2mrl-pipe-group-gold {
  background: var(--g2mrl-gold);
  padding: 10px 14px 14px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0;
  clip-path: polygon(0% 0%, 100% 0%, 75% 100%, 25% 100%);
  min-width: 210px;
}

.g2mrl .g2mrl-pipe-group-gold .g2mrl-pipe-node {
  background: none;
  border: none;
  color: var(--g2mrl-teal-deep);
  min-width: auto;
  padding: 3px 8px;
}

.g2mrl .g2mrl-pipe-group-gold .g2mrl-pipe-label {
  font-size: 0.42rem;
  font-weight: 600;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--g2mrl-teal-deep);
  opacity: 0.7;
  margin-top: 2px;
}

.g2mrl .g2mrl-pipe-group-teal {
  background: var(--g2mrl-teal);
  padding: 12px 14px 14px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0;
  clip-path: polygon(5% 0%, 95% 0%, 80% 100%, 20% 100%);
  min-width: 210px;
}

.g2mrl .g2mrl-pipe-group-teal .g2mrl-pipe-node {
  background: none;
  border: none;
  color: var(--g2mrl-teal-deep);
  min-width: auto;
  padding: 3px 8px;
}

.g2mrl .g2mrl-pipe-group-teal .g2mrl-pipe-label {
  font-size: 0.42rem;
  font-weight: 600;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--g2mrl-teal-deep);
  opacity: 0.7;
  margin-top: 2px;
}

.g2mrl .g2mrl-pipe-arrow {
  width: 1.5px;
  height: 10px;
  background: var(--g2mrl-red);
  position: relative;
}

.g2mrl .g2mrl-pipe-arrow::after {
  content: '';
  position: absolute;
  bottom: -3px;
  left: 50%;
  transform: translateX(-50%);
  border-left: 3.5px solid transparent;
  border-right: 3.5px solid transparent;
  border-top: 5px solid var(--g2mrl-red);
}

.g2mrl .g2mrl-pipe-arrow-inner {
  width: 1.5px;
  height: 6px;
  background: var(--g2mrl-red);
  position: relative;
}

.g2mrl .g2mrl-pipe-arrow-inner::after {
  content: '';
  position: absolute;
  bottom: -2px;
  left: 50%;
  transform: translateX(-50%);
  border-left: 3px solid transparent;
  border-right: 3px solid transparent;
  border-top: 4px solid var(--g2mrl-red);
}

.g2mrl .g2mrl-pipe-branch {
  display: flex;
  gap: 4px;
  position: relative;
}

.g2mrl .g2mrl-pipe-branch-item {
  font-size: 0.45rem;
  font-weight: 600;
  padding: 2px 6px;
  border: 1px solid var(--g2mrl-red);
  color: var(--g2mrl-red);
  text-transform: uppercase;
  border-radius: 2px;
}

.g2mrl .g2mrl-pipeline-text {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.g2mrl .g2mrl-pipeline-text h4 {
  font-size: 0.88rem;
  font-weight: 700;
  margin-bottom: 0.3rem;
}

.g2mrl .g2mrl-pipeline-text p {
  font-size: 0.75rem;
  line-height: 1.6;
  max-width: none;
}

.g2mrl .g2mrl-pipeline-callout {
  font-size: 0.72rem;
  line-height: 1.6;
  color: var(--g2mrl-ink);
  padding: 0.75rem 1rem;
  border-left: 3px solid var(--g2mrl-teal);
  background: rgba(77, 179, 168, 0.06);
}

/* SCALE */
.g2mrl .g2mrl-scale {
  background: var(--g2mrl-teal-deep);
  color: #fff;
}

.g2mrl .g2mrl-scale h3 {
  color: var(--g2mrl-teal);
}

.g2mrl .g2mrl-scale p {
  color: rgba(255, 255, 255, 0.55);
}

.g2mrl .g2mrl-scale-wrapper {
  margin-top: 1rem;
  width: 100%;
}

.g2mrl .g2mrl-scale-table {
  width: 100%;
  border-collapse: collapse;
  font-size: 0.68rem;
}

.g2mrl .g2mrl-scale-table thead th {
  font-size: 0.55rem;
  font-weight: 700;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--g2mrl-teal);
  padding: 6px 8px;
  border-bottom: 1.5px solid rgba(255, 255, 255, 0.1);
  text-align: left;
}

.g2mrl .g2mrl-scale-table thead th:first-child {
  text-align: center;
  width: 44px;
}

.g2mrl .g2mrl-scale-table tbody tr {
  border-bottom: 1px solid rgba(255, 255, 255, 0.05);
  cursor: pointer;
  transition: all 0.2s ease;
}

.g2mrl .g2mrl-scale-table tbody tr:hover {
  background: rgba(77, 179, 168, 0.05);
}

.g2mrl .g2mrl-scale-table tbody tr.highlighted {
  background: rgba(77, 179, 168, 0.1);
}

.g2mrl .g2mrl-scale-table tbody td {
  padding: 5px 8px;
  vertical-align: top;
  line-height: 1.4;
}

.g2mrl .g2mrl-scale-table tbody td:first-child {
  text-align: center;
  font-weight: 800;
}

.g2mrl .g2mrl-scale-level-num {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 26px;
  height: 26px;
  font-size: 0.8rem;
  border-radius: 2px;
}

.g2mrl .g2mrl-scale-phase {
  font-weight: 600;
  color: #fff;
  white-space: nowrap;
}

.g2mrl .g2mrl-scale-def {
  color: rgba(255, 255, 255, 0.5);
}

.g2mrl .g2mrl-scale-product {
  color: rgba(255, 255, 255, 0.35);
  font-size: 0.62rem;
  font-style: italic;
}

.g2mrl .g2mrl-scale-criteria {
  display: none;
  color: rgba(255, 255, 255, 0.5);
  font-size: 0.62rem;
  padding-top: 4px;
  border-top: 1px solid rgba(255, 255, 255, 0.06);
  margin-top: 4px;
}

.g2mrl .g2mrl-scale-table tbody tr.highlighted .g2mrl-scale-criteria {
  display: block;
}

.g2mrl .g2mrl-scale-footer {
  margin-top: 1rem;
  font-size: 0.72rem;
  color: rgba(255, 255, 255, 0.4);
  line-height: 1.6;
  max-width: 680px;
}

.g2mrl .g2mrl-scale-footer strong {
  color: rgba(255, 255, 255, 0.65);
  font-weight: 600;
}

/* KB FOUNDATION */
.g2mrl .g2mrl-kb {
  background: var(--g2mrl-bg);
}

.g2mrl .g2mrl-kb-layout {
  display: grid;
  grid-template-columns: 1.15fr 1fr;
  gap: 2rem;
  margin-top: 1.25rem;
  align-items: center;
}

.g2mrl .g2mrl-kb-text {
  display: flex;
  flex-direction: column;
  gap: 0.9rem;
}

.g2mrl .g2mrl-kb-text p {
  font-size: 0.75rem;
  line-height: 1.6;
  max-width: none;
}

.g2mrl .g2mrl-kb-text h4 {
  font-size: 0.85rem;
  font-weight: 700;
  margin-bottom: 0.3rem;
}

.g2mrl .g2mrl-kb-callout {
  font-size: 0.72rem;
  line-height: 1.6;
  color: var(--g2mrl-ink);
  padding: 0.75rem 1rem;
  border-left: 3px solid var(--g2mrl-gold);
  background: rgba(212, 168, 67, 0.08);
  margin-top: 0.3rem;
}

.g2mrl .g2mrl-kb-diagram {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.3rem;
  padding: 0.5rem 0;
}

.g2mrl .g2mrl-kb-tier-label {
  font-size: 0.5rem;
  font-weight: 700;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  color: var(--g2mrl-teal-dark);
  margin-bottom: 0.25rem;
}

.g2mrl .g2mrl-kb-pills {
  display: flex;
  flex-wrap: wrap;
  gap: 0.4rem;
  justify-content: center;
  max-width: 340px;
}

.g2mrl .g2mrl-kb-pill {
  font-size: 0.55rem;
  font-weight: 600;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  padding: 6px 10px;
  background: var(--g2mrl-paper);
  border: 1.5px solid var(--g2mrl-gray-light);
  color: var(--g2mrl-ink);
  border-radius: 2px;
}

.g2mrl .g2mrl-kb-pill-out {
  border-color: var(--g2mrl-teal);
  color: var(--g2mrl-teal-dark);
  background: rgba(77, 179, 168, 0.08);
}

.g2mrl .g2mrl-kb-arrow-v {
  width: 1.5px;
  height: 18px;
  background: var(--g2mrl-gold);
  position: relative;
  margin: 0.3rem 0;
}

.g2mrl .g2mrl-kb-arrow-v::after {
  content: '';
  position: absolute;
  bottom: -3px;
  left: 50%;
  transform: translateX(-50%);
  border-left: 4px solid transparent;
  border-right: 4px solid transparent;
  border-top: 6px solid var(--g2mrl-gold);
}

.g2mrl .g2mrl-kb-core {
  background: var(--g2mrl-teal);
  color: var(--g2mrl-teal-deep);
  padding: 1.1rem 1.6rem;
  text-align: center;
  clip-path: polygon(4% 0%, 96% 0%, 100% 50%, 96% 100%, 4% 100%, 0% 50%);
  min-width: 280px;
}

.g2mrl .g2mrl-kb-core-title {
  font-size: 0.95rem;
  font-weight: 800;
  letter-spacing: 0.1em;
  margin-bottom: 0.2rem;
}

.g2mrl .g2mrl-kb-core-sub {
  font-size: 0.62rem;
  font-weight: 600;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  opacity: 0.8;
  margin-bottom: 0.35rem;
}

.g2mrl .g2mrl-kb-core-meta {
  font-size: 0.52rem;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  opacity: 0.55;
}

/* DELIVERABLES */
.g2mrl .g2mrl-deliverables {
  background: var(--g2mrl-bg-alt);
}

.g2mrl .g2mrl-deliverables-list {
  margin-top: 1.25rem;
}

.g2mrl .g2mrl-deliverable-item {
  display: grid;
  grid-template-columns: 28px 1fr;
  gap: 0.75rem;
  align-items: start;
  padding: 0.8rem 0;
  border-bottom: 1px solid var(--g2mrl-gray-light);
}

.g2mrl .g2mrl-deliverable-item:first-child {
  border-top: 1px solid var(--g2mrl-gray-light);
}

.g2mrl .g2mrl-deliverable-num {
  font-size: 0.65rem;
  font-weight: 700;
  color: var(--g2mrl-teal-dark);
  padding-top: 1px;
}

.g2mrl .g2mrl-deliverable-text {
  font-size: 0.78rem;
  color: var(--g2mrl-ink);
  line-height: 1.5;
}

.g2mrl .g2mrl-deliverable-text strong {
  font-weight: 700;
}

/* METHODOLOGY */
.g2mrl .g2mrl-methodology {
  background: var(--g2mrl-bg);
}

.g2mrl .g2mrl-method-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1.25rem;
  margin-top: 1.25rem;
}

.g2mrl .g2mrl-method-card {
  background: var(--g2mrl-paper);
  border: 1px solid var(--g2mrl-gray-light);
  padding: 1rem 1.15rem;
}

.g2mrl .g2mrl-method-card h4 {
  font-size: 0.78rem;
  font-weight: 700;
  margin-bottom: 0.35rem;
}

.g2mrl .g2mrl-method-card p {
  font-size: 0.72rem;
  line-height: 1.55;
  max-width: none;
}

.g2mrl .g2mrl-method-num {
  font-size: 0.6rem;
  font-weight: 700;
  color: var(--g2mrl-teal);
  margin-bottom: 0.35rem;
  display: block;
}

/* PROCESS */
.g2mrl .g2mrl-process {
  background: var(--g2mrl-bg-alt);
}

.g2mrl .g2mrl-process-steps {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 1.25rem;
  margin-top: 1.25rem;
}

.g2mrl .g2mrl-process-step {
  text-align: center;
  position: relative;
}

.g2mrl .g2mrl-process-step::after {
  content: '';
  position: absolute;
  top: 20px;
  right: -0.625rem;
  width: 1.25rem;
  height: 1.5px;
  background: var(--g2mrl-gray-light);
}

.g2mrl .g2mrl-process-step:last-child::after {
  display: none;
}

.g2mrl .g2mrl-step-hex {
  width: 40px;
  height: 40px;
  margin: 0 auto 0.6rem;
}

.g2mrl .g2mrl-step-hex svg {
  width: 100%;
  height: 100%;
}

.g2mrl .g2mrl-process-step h4 {
  font-size: 0.78rem;
  font-weight: 700;
  margin-bottom: 0.3rem;
}

.g2mrl .g2mrl-process-step p {
  font-size: 0.68rem;
  color: #5a6868;
  max-width: none;
  line-height: 1.5;
}

/* TRL / WHAT COMES AFTER */
.g2mrl .g2mrl-trl {
  background: var(--g2mrl-bg-alt);
}

.g2mrl .g2mrl-trl-layout {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 2rem;
  margin-top: 1.25rem;
  align-items: start;
}

.g2mrl .g2mrl-trl-text p {
  font-size: 0.78rem;
  line-height: 1.6;
  max-width: none;
}

.g2mrl .g2mrl-trl-text h4 {
  font-size: 0.88rem;
  font-weight: 700;
  margin-bottom: 0.4rem;
}

.g2mrl .g2mrl-trl-text h4.g2mrl-sub {
  margin-top: 1rem;
}

.g2mrl .g2mrl-trl-mini-table {
  width: 100%;
  border-collapse: collapse;
  font-size: 0.65rem;
}

.g2mrl .g2mrl-trl-mini-table th {
  font-size: 0.55rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--g2mrl-teal-dark);
  padding: 5px 6px;
  border-bottom: 1.5px solid var(--g2mrl-gray-light);
  text-align: left;
}

.g2mrl .g2mrl-trl-mini-table th:first-child {
  width: 36px;
  text-align: center;
}

.g2mrl .g2mrl-trl-mini-table td {
  padding: 4px 6px;
  border-bottom: 1px solid rgba(0, 0, 0, 0.04);
  vertical-align: top;
  line-height: 1.4;
}

.g2mrl .g2mrl-trl-mini-table td:first-child {
  text-align: center;
  font-weight: 700;
  color: var(--g2mrl-teal-dark);
}

.g2mrl .g2mrl-trl-phase {
  font-weight: 600;
}

.g2mrl .g2mrl-trl-badges {
  display: flex;
  gap: 0.75rem;
  margin-top: 1.25rem;
  flex-wrap: wrap;
}

.g2mrl .g2mrl-trl-badge {
  padding: 0.5rem 0.75rem;
  border: 1.5px solid var(--g2mrl-gray-light);
  background: var(--g2mrl-paper);
  text-align: center;
}

.g2mrl .g2mrl-trl-badge-label {
  font-size: 0.55rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--g2mrl-teal-dark);
  margin-bottom: 2px;
}

.g2mrl .g2mrl-trl-badge-desc {
  font-size: 0.62rem;
  color: var(--g2mrl-gray);
}

/* CTA */
.g2mrl .g2mrl-cta {
  background: var(--g2mrl-teal-deep);
  color: #fff;
}

.g2mrl .g2mrl-cta h3 {
  color: var(--g2mrl-teal);
}

.g2mrl .g2mrl-cta p {
  color: rgba(255, 255, 255, 0.5);
}

.g2mrl .g2mrl-cta-row {
  display: flex;
  gap: 1rem;
  margin-top: 1.5rem;
  align-items: center;
}

.g2mrl .g2mrl-btn {
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

.g2mrl .g2mrl-btn-primary {
  background: var(--g2mrl-teal);
  color: var(--g2mrl-teal-deep);
}

.g2mrl .g2mrl-btn-primary:hover {
  background: var(--g2mrl-teal-dark);
}

.g2mrl .g2mrl-btn-outline {
  background: transparent;
  color: rgba(255, 255, 255, 0.5);
  border: 1.5px solid rgba(255, 255, 255, 0.2);
}

.g2mrl .g2mrl-btn-outline:hover {
  border-color: var(--g2mrl-teal);
  color: var(--g2mrl-teal);
}

.g2mrl .g2mrl-cta-footer {
  margin-top: 2rem;
  padding-top: 1.5rem;
  border-top: 1px solid rgba(255, 255, 255, 0.07);
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
}

/* Animations */
.g2mrl .fade-in {
  opacity: 0;
  transform: translateY(14px);
  transition: opacity 0.5s ease, transform 0.5s ease;
}

.g2mrl .fade-in.visible {
  opacity: 1;
  transform: translateY(0);
}

.g2mrl .stagger > * {
  opacity: 0;
  transform: translateY(10px);
}

.g2mrl .stagger.visible > *:nth-child(1) {
  animation: g2mrl-fadeUp 0.4s ease forwards 0.05s;
}

.g2mrl .stagger.visible > *:nth-child(2) {
  animation: g2mrl-fadeUp 0.4s ease forwards 0.1s;
}

.g2mrl .stagger.visible > *:nth-child(3) {
  animation: g2mrl-fadeUp 0.4s ease forwards 0.15s;
}

.g2mrl .stagger.visible > *:nth-child(4) {
  animation: g2mrl-fadeUp 0.4s ease forwards 0.2s;
}

.g2mrl .stagger.visible > *:nth-child(5) {
  animation: g2mrl-fadeUp 0.4s ease forwards 0.25s;
}

.g2mrl .stagger.visible > *:nth-child(6) {
  animation: g2mrl-fadeUp 0.4s ease forwards 0.3s;
}

.g2mrl .stagger.visible > *:nth-child(7) {
  animation: g2mrl-fadeUp 0.4s ease forwards 0.35s;
}

@keyframes g2mrl-fadeUp {
  to { opacity: 1; transform: translateY(0); }
}

/* Responsive */
@media (max-width: 768px) {
  .g2mrl .g2mrl-section {
    padding: 1.5rem 1.25rem;
  }
  .g2mrl .g2mrl-problem-grid,
  .g2mrl .g2mrl-method-grid {
    grid-template-columns: 1fr;
  }
  .g2mrl .g2mrl-process-steps {
    grid-template-columns: 1fr 1fr;
    gap: 1.5rem;
  }
  .g2mrl .g2mrl-process-step::after {
    display: none;
  }
  .g2mrl .g2mrl-cta-footer {
    flex-direction: column;
    gap: 1rem;
    text-align: center;
  }
  .g2mrl-nav-dots {
    right: 0.75rem;
  }
  .g2mrl .g2mrl-pipeline-layout,
  .g2mrl .g2mrl-trl-layout,
  .g2mrl .g2mrl-kb-layout {
    grid-template-columns: 1fr;
  }
  .g2mrl .g2mrl-scale-table {
    font-size: 0.6rem;
  }
  .g2mrl .g2mrl-scale-table thead th {
    padding: 5px 4px;
    font-size: 0.5rem;
  }
  .g2mrl .g2mrl-scale-table tbody td {
    padding: 4px;
  }
  .g2mrl .g2mrl-scale-level-num {
    width: 22px;
    height: 22px;
    font-size: 0.7rem;
  }
}

@media (max-width: 480px) {
  .g2mrl .g2mrl-process-steps {
    grid-template-columns: 1fr;
  }
  .g2mrl .g2mrl-cta-row {
    flex-direction: column;
  }
  .g2mrl .g2mrl-pipe-node {
    min-width: 110px;
    font-size: 0.45rem;
  }
}
</style>
