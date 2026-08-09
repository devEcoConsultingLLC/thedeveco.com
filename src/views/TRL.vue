<script setup lang="ts">
import { ref, onMounted, onUnmounted, nextTick } from 'vue'

// Section list drives nav dot rendering and IntersectionObserver targeting.
const sectionList = [
  { id: 'hero', label: 'Hero' },
  { id: 'problem', label: 'The Problem' },
  { id: 'pipeline', label: 'Maturation Flow' },
  { id: 'scale', label: 'TRL Scale' },
  { id: 'kb', label: 'Technical Foundation' },
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

interface NextFrameworkRow {
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
    title: 'Overclaimed Maturity',
    body: 'The deck says TRL 6. The lab notebook says TRL 3. Investors ask for evidence. Grants get rejected. Tax credits get clawed back years after the filing.',
  },
  {
    title: 'Missing Evidence',
    body: 'Validation claims without documented proof. When auditors, reviewers, or acquirers ask to see the data, the paper trail does not exist. You rebuild it under time pressure.',
  },
  {
    title: 'Certification Blind Spots',
    body: 'Regulatory, safety, privacy, and industry compliance requirements discovered during pilot. Six months disappear re-certifying what should have been scoped in year one.',
  },
  {
    title: 'Lab-to-Field Gap',
    body: 'Technology works in the controlled setup. It falls apart in the user environment. No one maps the difference until pilot customers are live and the calls are already coming in.',
  },
  {
    title: 'Commercialization Stall',
    body: 'Technology plateaus at TRL 7. Engineering cannot push it further without market, regulatory, or manufacturing context the engineering team does not have. Progress looks like activity with no advancement.',
  },
  {
    title: 'Tribal Technical Knowledge',
    body: 'Engineering decisions, design tradeoffs, and IP rationale live in Slack, heads, and the senior engineer who is about to leave. Every departure degrades the asset.',
  },
]

const deliverables: Deliverable[] = [
  {
    num: '01',
    title: 'TRL Position Assessment',
    body: 'Current-state TRL level established with evidence. Not where you claim to be. Where the evidence actually supports, with sign-off from the audit team.',
  },
  {
    num: '02',
    title: 'Evidence & Validation Audit',
    body: 'Every validation claim traced to documentation. Gaps in test data, results, and sign-offs identified and prioritized. The material investors, grant reviewers, and acquirers will eventually ask for.',
  },
  {
    num: '03',
    title: 'Certification & Compliance Map',
    body: 'Regulatory, safety, privacy, and industry requirements mapped to your target commercial stage. Blind spots surfaced before pilot rather than during it.',
  },
  {
    num: '04',
    title: 'Technical Gap Analysis',
    body: 'What is needed to advance each level. Engineering work, validation work, compliance work, and commercialization work separated, sized, and sequenced.',
  },
  {
    num: '05',
    title: 'Tiered Commercialization Roadmap',
    body: '12-18 month plan sorted by effort, risk, and capital requirement. What gets you to the next TRL level versus what takes you to market.',
  },
  {
    num: '06',
    title: 'Technical Knowledge Blueprint',
    body: 'Structured plan for capturing engineering decisions, validation data, and IP continuity. First seed of an ongoing KB that devEco™ builds and maintains.',
  },
  {
    num: '07',
    title: 'Engineering Leadership Coaching',
    body: 'Sessions with your technical leadership. Paired with coaching from engineers who have worked across semiconductor, edge AI, and embedded systems programs from research through commercialization.',
  },
]

const methodologyCards: MethodCard[] = [
  {
    letter: 'A',
    title: 'Current-State Maturity Assessment',
    body: 'Where you actually sit on the TRL scale versus where you are claiming. Evidence-based, not narrative-based. Backed by a technical sign-off you can defend in a data room.',
  },
  {
    letter: 'B',
    title: 'Evidence & Validation Audit',
    body: 'Every technical claim traced to documented proof. Gaps surfaced, severity rated, remediation paths scoped. The audit is the dress rehearsal for the real scrutiny coming next.',
  },
  {
    letter: 'C',
    title: 'TRL Readiness Scoring',
    body: 'Applied against the canonical NASA scale with the devEco™ commercial translation layer. Scored 1-9 with specific criteria, evidence review, and the commercial milestone each level unlocks.',
  },
  {
    letter: 'D',
    title: 'Technical Knowledge & Continuity',
    body: 'How engineering decisions, IP rationale, and institutional know-how survive turnover. The difference between a program that ends when the senior engineer leaves and a durable asset the business can underwrite.',
  },
]

const readinessBadges: ReadinessBadge[] = [
  { label: 'G2MRL', desc: 'Go-to-Market' },
  { label: 'DevXRL', desc: 'Developer Experience' },
  { label: 'SMRL', desc: 'Social Media' },
]

const scaleData: ScaleRow[] = [
  { level: 9, phase: 'Market Introduction', def: 'System proven in operational environment. Ready for mass production.', criteria: 'Production-grade manufacturing running. Regulatory clearances in hand. Revenue from non-pilot customers. Support and warranty operations functional. Final system matches every claim in the deck.', product: 'Shipping Product', numStyle: 'background:rgba(77,179,168,0.25);color:var(--trl-teal)' },
  { level: 8, phase: 'Complete & Operational', def: 'System complete and qualified. Meets certifications and requirements.', criteria: 'Final form factor and performance locked. All certifications achieved. Manufacturing process validated. Field trials complete. Contracts ready to activate.', product: 'Pre-Launch', numStyle: 'background:rgba(77,179,168,0.2);color:var(--trl-teal)' },
  { level: 7, phase: 'Operational Demo', def: 'Prototype demonstrated in operational environment with real workloads.', criteria: 'Deployed at pilot customer sites. Performance matches lab claims under real conditions. Customer feedback loop functional. Acquisition-grade evidence available.', product: 'Pilot Customers Live', numStyle: 'background:rgba(77,179,168,0.16);color:var(--trl-teal)' },
  { level: 6, phase: 'Test Demo', def: 'Prototype demonstrated in relevant environment. Technology risk largely retired.', criteria: 'Demonstrated outside the controlled lab. Key performance metrics validated against targets. Major technical risks resolved. Series B evidence threshold.', product: 'Validation Contracts', numStyle: 'background:rgba(212,168,67,0.22);color:var(--trl-gold)' },
  { level: 5, phase: 'Validation', def: 'Critical components validated in relevant environment beyond lab.', criteria: 'Integration challenges surfaced and being addressed. Performance characterized outside ideal conditions. Series A evidence threshold. Grant competitiveness strong.', product: 'Series A Readiness', numStyle: 'background:rgba(212,168,67,0.17);color:var(--trl-gold)' },
  { level: 4, phase: 'Lab Prototype', def: 'Full lab prototype exists. Basic functionality demonstrated end-to-end.', criteria: 'Components integrated in controlled setup. End-to-end function works under ideal conditions. Seed stage evidence threshold. R&D tax credit defensible.', product: 'Pre-Seed / Seed', numStyle: 'background:rgba(212,168,67,0.12);color:var(--trl-gold)' },
  { level: 3, phase: 'Proof of Concept', def: 'Analytical and experimental proof of concept.', criteria: 'Critical functions demonstrated separately. Analytical studies support feasibility. Unique selling points documented. Patent claims defensible.', product: 'Defensible IP', numStyle: 'background:rgba(196,91,91,0.2);color:var(--trl-red)' },
  { level: 2, phase: 'Applied Research', def: 'Technology concept and application formulated.', criteria: 'Specific applications identified. Patent filings possible. Analytical studies beginning. No experimental proof yet.', product: 'IP Filings Begin', numStyle: 'background:rgba(196,91,91,0.15);color:var(--trl-red)' },
  { level: 1, phase: 'Fundamental Research', def: 'Basic principles observed and reported.', criteria: 'Scientific principles being studied. No specific application yet. Published research, academic collaboration, early-stage exploration.', product: 'Academic Phase', numStyle: 'background:rgba(196,91,91,0.1);color:var(--trl-red)' },
]

// Mini-table on the "what comes next" section shows G2MRL levels.
const g2mrlData: NextFrameworkRow[] = [
  { level: 9, phase: 'Category-Defining', status: 'Your GTM sets the industry benchmark.' },
  { level: 8, phase: 'Dominant', status: 'You win the deals you target. Ecosystem compounds.' },
  { level: 7, phase: 'Scalable', status: 'Motion extends across segments and regions.' },
  { level: 6, phase: 'Repeatable', status: 'Documented motion. Forecasts track reality.' },
  { level: 5, phase: 'Aligned', status: 'Sales, marketing, partnerships share the ICP.' },
  { level: 4, phase: 'Functional', status: 'ICP defined. Basic playbook exists.' },
  { level: 3, phase: 'Leaking', status: 'Spend happening, conversion is low.' },
  { level: 2, phase: 'Reactive', status: 'Activity without plan.' },
  { level: 1, phase: 'Undefined', status: 'No strategy. Hoping for inbound.' },
]

const processSteps: ProcessStep[] = [
  { num: 1, title: 'Discovery', desc: 'We analyze your technology, current stage, and commercial objectives to understand what you actually need the audit to defend.' },
  { num: 2, title: 'Audit', desc: 'Deep evaluation using TRL scoring, evidence review, and gap analysis across every level of your maturation path.' },
  { num: 3, title: 'Deliver', desc: 'Complete report with scored current state, evidence audit, gap analysis, and tiered roadmap sized to your next milestone.' },
  { num: 4, title: 'Support', desc: 'Optional follow-on for implementation. Technology programs that compound into durable assets, not dependencies that require us forever.' },
]

const upstreamPills = ['Engineering Decisions', 'Test Results', 'IP Portfolio', 'Validation Data']
const downstreamPills = ['Investor Data Rooms', 'Grant Filings', 'Engineering Onboarding', 'Tax Credit Audits']

// Nav dots + section tracking
const sectionEls = ref<HTMLElement[]>([])
const activeSectionIndex = ref(0)

function collectSections() {
  sectionEls.value = Array.from(
    document.querySelectorAll<HTMLElement>('.trl .trl-section')
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
    .querySelectorAll<HTMLElement>('.trl .fade-in')
    .forEach((el) => fadeObserver!.observe(el))
}

// Scale table toggle (single row expand)
const highlightedLevel = ref<number | null>(null)
function toggleLevel(level: number) {
  highlightedLevel.value = highlightedLevel.value === level ? null : level
}

onMounted(async () => {
  document.documentElement.classList.add('trl-page')
  await nextTick()
  collectSections()
  setupObservers()
})

onUnmounted(() => {
  document.documentElement.classList.remove('trl-page')
  sectionObserver?.disconnect()
  fadeObserver?.disconnect()
})
</script>

<template>
  <div class="trl">
    <!-- Nav dots -->
    <nav class="trl-nav-dots" aria-label="Section navigation">
      <button
        v-for="(section, i) in sectionList"
        :key="section.id"
        type="button"
        class="trl-nav-dot"
        :class="{ active: activeSectionIndex === i }"
        :aria-label="`Jump to ${section.label}`"
        @click="scrollToSection(i)"
      ></button>
    </nav>

    <!-- HERO -->
    <section id="hero" class="trl-section trl-hero">
      <div class="trl-container">
        <div class="trl-hero-logo">
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
        <h1 class="trl-hero-title">T<span class="trl-accent">RL</span> Audit</h1>
        <p class="trl-hero-sub">Technology Readiness Levels</p>
        <p class="trl-hero-tagline">
          An evidence-based evaluation of your technology maturity against the industry-standard
          TRL scale. Where you actually sit, what evidence supports the claim, and how to advance
          with credibility that holds up to investors, grant reviewers, and acquirers.
        </p>
      </div>
      <div class="trl-scroll-hint">
        <svg viewBox="0 0 24 24">
          <path d="M12 5v14M19 12l-7 7-7-7" />
        </svg>
        <span class="trl-small">scroll</span>
      </div>
    </section>

    <!-- PROBLEM -->
    <section id="problem" class="trl-section trl-problem">
      <div class="trl-container">
        <span class="trl-tag trl-tag-red">THE PROBLEM</span>
        <h2>Overclaims Get Caught</h2>
        <p>
          Founders claim TRL 6. Engineers know it is TRL 3. Investors, grant reviewers, and
          acquirers find out the same way: by asking for evidence. The gap between claim and
          reality is where deals collapse, credits get clawed back, and runway evaporates six
          months early.
        </p>
        <div class="trl-problem-grid fade-in">
          <div class="trl-problem-card">
            <div class="trl-icon-box">
              <svg viewBox="0 0 24 24">
                <path d="M12 2v4M12 18v4M4.93 4.93l2.83 2.83M16.24 16.24l2.83 2.83M2 12h4M18 12h4" />
                <circle cx="12" cy="12" r="5" stroke-dasharray="3 2" />
              </svg>
            </div>
            <h4>{{ problemCards[0]!.title }}</h4>
            <p>{{ problemCards[0]!.body }}</p>
          </div>
          <div class="trl-problem-card">
            <div class="trl-icon-box">
              <svg viewBox="0 0 24 24">
                <path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z" />
                <polyline points="14 2 14 8 20 8" />
                <line x1="9" y1="13" x2="15" y2="13" stroke-dasharray="2 2" />
                <line x1="9" y1="17" x2="13" y2="17" stroke-dasharray="2 2" />
              </svg>
            </div>
            <h4>{{ problemCards[1]!.title }}</h4>
            <p>{{ problemCards[1]!.body }}</p>
          </div>
          <div class="trl-problem-card">
            <div class="trl-icon-box">
              <svg viewBox="0 0 24 24">
                <polygon points="12 2 2 21 22 21" />
                <line x1="12" y1="9" x2="12" y2="14" />
                <line x1="12" y1="17" x2="12.01" y2="17" />
              </svg>
            </div>
            <h4>{{ problemCards[2]!.title }}</h4>
            <p>{{ problemCards[2]!.body }}</p>
          </div>
          <div class="trl-problem-card">
            <div class="trl-icon-box">
              <svg viewBox="0 0 24 24">
                <rect x="2" y="7" width="9" height="10" rx="1" />
                <rect x="15" y="3" width="7" height="18" rx="1" stroke-dasharray="3 2" />
                <line x1="11" y1="12" x2="15" y2="12" />
              </svg>
            </div>
            <h4>{{ problemCards[3]!.title }}</h4>
            <p>{{ problemCards[3]!.body }}</p>
          </div>
          <div class="trl-problem-card">
            <div class="trl-icon-box">
              <svg viewBox="0 0 24 24">
                <polyline points="3 17 9 11 13 15 21 7" />
                <polyline points="15 7 21 7 21 13" />
                <line x1="19" y1="17" x2="21" y2="17" />
                <line x1="22" y1="19" x2="17" y2="19" />
              </svg>
            </div>
            <h4>{{ problemCards[4]!.title }}</h4>
            <p>{{ problemCards[4]!.body }}</p>
          </div>
          <div class="trl-problem-card">
            <div class="trl-icon-box">
              <svg viewBox="0 0 24 24">
                <circle cx="12" cy="7" r="3" />
                <path d="M6 21v-2a4 4 0 0 1 4-4h4a4 4 0 0 1 4 4v2" />
                <line x1="3" y1="3" x2="9" y2="9" stroke-dasharray="2 2" />
              </svg>
            </div>
            <h4>{{ problemCards[5]!.title }}</h4>
            <p>{{ problemCards[5]!.body }}</p>
          </div>
        </div>
      </div>
    </section>

    <!-- PIPELINE -->
    <section id="pipeline" class="trl-section trl-pipeline">
      <div class="trl-container">
        <span class="trl-tag trl-tag-gold">THE MATURATION FLOW</span>
        <h2>Evidence Over Narrative</h2>
        <div class="trl-pipeline-layout fade-in">
          <div class="trl-pipeline-diagram">
            <div class="trl-pipe-node trl-pipe-rect-wide">TECHNOLOGY VISION</div>
            <div class="trl-pipe-arrow"></div>
            <div class="trl-pipe-node trl-pipe-rect">APPLIED CONCEPT</div>
            <div class="trl-pipe-arrow"></div>
            <div class="trl-pipe-node trl-pipe-rect">PROOF OF CONCEPT</div>
            <div class="trl-pipe-arrow"></div>
            <div class="trl-pipe-group-gold">
              <div class="trl-pipe-node trl-pipe-inner">LAB PROTOTYPE</div>
              <div class="trl-pipe-label">DEVELOPMENT FUNNEL</div>
              <div class="trl-pipe-arrow-inner"></div>
              <div class="trl-pipe-node trl-pipe-inner">FIELD VALIDATION</div>
            </div>
            <div class="trl-pipe-arrow"></div>
            <div class="trl-pipe-node trl-pipe-rect">OPERATIONAL DEMO</div>
            <div class="trl-pipe-arrow"></div>
            <div class="trl-pipe-branch">
              <div class="trl-pipe-branch-item">Safety</div>
              <div class="trl-pipe-branch-item">Regulatory</div>
              <div class="trl-pipe-branch-item">Industry</div>
            </div>
            <div class="trl-pipe-arrow"></div>
            <div class="trl-pipe-group-teal">
              <div class="trl-pipe-node trl-pipe-inner-teal">QUALIFIED</div>
              <div class="trl-pipe-arrow-inner"></div>
              <div class="trl-pipe-node trl-pipe-inner-teal">LAUNCH</div>
              <div class="trl-pipe-label">DEPLOYMENT FUNNEL</div>
              <div class="trl-pipe-arrow-inner"></div>
              <div class="trl-pipe-node trl-pipe-inner-teal">DEPLOYED</div>
            </div>
          </div>
          <div class="trl-pipeline-text">
            <div>
              <h4>Where Programs Break</h4>
              <p>
                TRL programs break between the lab and the field. Demonstrations that look perfect
                in controlled setups reveal their limits only when real users, real environments,
                and real regulatory scrutiny enter the picture. By then burn rate has accelerated
                and runway is half what it was.
              </p>
            </div>
            <div>
              <h4>What We Do About It</h4>
              <p>
                devEco™ audits every transition in the maturation flow: lab validity, field
                readiness, certification path, commercial fit. We identify which gaps are technical
                debt, which are paperwork, and which require deeper rework before the next funding
                milestone, grant deadline, or customer commitment.
              </p>
            </div>
            <div class="trl-pipeline-callout">
              TRL is not a marketing claim. It is a proof of progression. Every level is earned,
              every jump is evidence-based. Done right, the scale protects you. Done wrong, it
              exposes you.
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- SCALE -->
    <section id="scale" class="trl-section trl-scale">
      <div class="trl-container">
        <span class="trl-tag trl-tag-teal" style="color:var(--trl-teal);border-color:var(--trl-teal)">THE FRAMEWORK</span>
        <h2 style="color:#fff">TRL Readiness Levels</h2>
        <p>
          Borrowed from NASA, maintained by devEco™ with a commercial translation layer. Click any
          level to see the evaluation criteria.
        </p>
        <div class="trl-scale-wrapper fade-in">
          <table class="trl-scale-table">
            <thead>
              <tr>
                <th></th>
                <th>Phase</th>
                <th>Definition</th>
                <th>Commercial Translation</th>
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
                  <span class="trl-scale-level-num" :style="row.numStyle">{{ row.level }}</span>
                </td>
                <td class="trl-scale-phase">{{ row.phase }}</td>
                <td class="trl-scale-def">
                  {{ row.def }}
                  <div class="trl-scale-criteria">{{ row.criteria }}</div>
                </td>
                <td class="trl-scale-product">{{ row.product }}</td>
              </tr>
            </tbody>
          </table>
        </div>
        <div class="trl-scale-footer">
          Most deep-tech programs overclaim by two levels. Investors know. Grant reviewers know.
          Acquirers know.
          <strong>TRL only protects you when the level you claim matches the evidence you can produce.</strong>
          devEco™ audits the gap between claim and reality, then closes it with the engineering
          work the business requires.
        </div>
      </div>
    </section>

    <!-- KB / TECHNICAL FOUNDATION -->
    <section id="kb" class="trl-section trl-kb">
      <div class="trl-container">
        <span class="trl-tag trl-tag-gold">THE TECHNICAL FOUNDATION</span>
        <h2>Technical Knowledge Outlives the Engineers Who Built It</h2>
        <p>
          Hardware programs take years. Engineering teams turn over. IP lives for decades. When
          senior engineers leave, they take decades of decisions, tradeoffs, and rationale with
          them. Most companies reinvent the same lessons every eighteen months.
        </p>
        <div class="trl-kb-layout fade-in">
          <div class="trl-kb-text">
            <div>
              <h4>The Audit Surfaces What Is Undocumented</h4>
              <p>
                TRL surfaces every piece of technical evidence your organization needs but has not
                structured. Engineering decisions and their rationale. Test results and their
                context. Certification status and gaps. IP filings and their commercial logic. The
                material that makes or breaks a due diligence review, a grant application, or a
                tax credit audit.
              </p>
            </div>
            <div>
              <h4>devEco™ Preserves the Institutional Asset</h4>
              <p>
                From there, devEco™ builds and maintains a technical knowledge base that serves
                three audiences at once: the engineers who inherit the work, the executives who
                answer investor questions, and the reviewers who evaluate claims for grants,
                credits, and acquisition. White-glove capture, expert maintenance, zero
                hand-waving.
              </p>
            </div>
            <div class="trl-kb-callout">
              Your IP is not in patents alone. It is in the engineering decisions that led to
              them. TRL is how you preserve that decision history before it walks out the door.
            </div>
          </div>
          <div class="trl-kb-diagram">
            <div class="trl-kb-tier-label">Upstream Sources</div>
            <div class="trl-kb-pills">
              <span v-for="pill in upstreamPills" :key="pill" class="trl-kb-pill">{{ pill }}</span>
            </div>
            <div class="trl-kb-arrow-v"></div>
            <div class="trl-kb-core">
              <div class="trl-kb-core-title">TECHNICAL KNOWLEDGE</div>
              <div class="trl-kb-core-sub">Source of Truth</div>
              <div class="trl-kb-core-meta">Built and Maintained by devEco™</div>
            </div>
            <div class="trl-kb-arrow-v"></div>
            <div class="trl-kb-tier-label">Downstream Surfaces</div>
            <div class="trl-kb-pills">
              <span
                v-for="pill in downstreamPills"
                :key="pill"
                class="trl-kb-pill trl-kb-pill-out"
              >{{ pill }}</span>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- DELIVERABLES -->
    <section id="deliverables" class="trl-section trl-deliverables">
      <div class="trl-container">
        <span class="trl-tag trl-tag-teal">WHAT YOU GET</span>
        <h2>Audit Deliverables</h2>
        <p>
          Concrete outputs. Not a glossy maturity report, not generic TRL definitions, not a
          roadmap disconnected from your actual program.
        </p>
        <div class="trl-deliverables-list stagger fade-in">
          <div
            v-for="item in deliverables"
            :key="item.num"
            class="trl-deliverable-item"
          >
            <span class="trl-deliverable-num">{{ item.num }}</span>
            <span class="trl-deliverable-text">
              <strong>{{ item.title }}</strong> : {{ item.body }}
            </span>
          </div>
        </div>
      </div>
    </section>

    <!-- METHODOLOGY -->
    <section id="methodology" class="trl-section trl-methodology">
      <div class="trl-container">
        <span class="trl-tag trl-tag-gold">METHODOLOGY</span>
        <h2>How We Evaluate</h2>
        <p>
          Applied by engineers and ecosystem architects who have worked across semiconductor, edge
          AI, and embedded systems programs from research through commercialization.
        </p>
        <div class="trl-method-grid fade-in">
          <div
            v-for="card in methodologyCards"
            :key="card.letter"
            class="trl-method-card"
          >
            <span class="trl-method-num">{{ card.letter }}</span>
            <h4>{{ card.title }}</h4>
            <p>{{ card.body }}</p>
          </div>
        </div>
      </div>
    </section>

    <!-- PROCESS -->
    <section id="process" class="trl-section trl-process">
      <div class="trl-container">
        <span class="trl-tag trl-tag-teal">OUR PROCESS</span>
        <h2>From Discovery to Delivery</h2>
        <p>Embedded partner, not external auditor. We work alongside your team.</p>
        <div class="trl-process-steps fade-in">
          <div
            v-for="step in processSteps"
            :key="step.num"
            class="trl-process-step"
          >
            <div class="trl-step-hex">
              <svg viewBox="0 0 56 56">
                <polygon
                  points="28,2 52,15 52,41 28,54 4,41 4,15"
                  fill="var(--trl-teal)"
                  stroke="var(--trl-teal-deep)"
                  stroke-width="1.5"
                />
                <text
                  x="28"
                  y="33"
                  text-anchor="middle"
                  font-family="'JetBrains Mono',monospace"
                  font-size="18"
                  font-weight="700"
                  fill="var(--trl-teal-deep)"
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
    <section id="trl" class="trl-section trl-next">
      <div class="trl-container">
        <span class="trl-tag trl-tag-gold">WHAT COMES NEXT</span>
        <h2>Additional Readiness Assessments</h2>
        <p>
          TRL covers the technology. For a complete view of your organization's readiness to
          commercialize it, we offer three additional frameworks as separate engagements.
        </p>
        <div class="trl-next-layout fade-in">
          <div class="trl-next-text">
            <h4>Go-to-Market Readiness (G2MRL)</h4>
            <p>
              The natural commercial complement to TRL. Where TRL tells you whether the technology
              is ready, G2MRL tells you whether your company can sell it. Positioning, demand
              generation, pipeline motion, and ecosystem leverage, scored and rebuilt for the stage
              of commercialization your technology has earned.
            </p>
            <table class="trl-next-mini-table" style="margin-top:0.75rem">
              <thead>
                <tr>
                  <th>Lvl</th>
                  <th>Phase</th>
                  <th>Status</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="row in g2mrlData" :key="row.level">
                  <td>{{ row.level }}</td>
                  <td class="trl-next-phase">{{ row.phase }}</td>
                  <td>{{ row.status }}</td>
                </tr>
              </tbody>
            </table>
          </div>
          <div class="trl-next-text">
            <h4>Developer Experience Readiness (DevXRL)</h4>
            <p>
              Evaluates whether developers can actually adopt your technology. Documentation,
              onboarding, content pipelines, and community engagement. Applies when your
              commercial motion depends on builders choosing your stack. Closes the bottom-up loop
              that turns awareness into internal advocacy.
            </p>
            <h4 class="trl-sub">Social Media Readiness (SMRL)</h4>
            <p>
              Evaluates your brand's social presence, platform fit, and audience leverage. Where
              effort is wasted, where attention is leaking, and how to plant the source of truth
              your brand needs to thrive in the AI era.
            </p>
            <p style="margin-top:0.75rem">
              Technology can be ready while go-to-market stalls. GTM can be tight while developer
              adoption lags. Brand can be loud while technology is hollow. Each gap is where
              adoption breaks. By combining frameworks, we give you a complete picture of
              readiness across every surface that matters.
            </p>
            <div class="trl-next-badges">
              <div
                v-for="badge in readinessBadges"
                :key="badge.label"
                class="trl-next-badge"
              >
                <div class="trl-next-badge-label">{{ badge.label }}</div>
                <div class="trl-next-badge-desc">{{ badge.desc }}</div>
              </div>
            </div>
            <p style="margin-top:0.75rem;font-size:0.72rem;color:var(--trl-gray)">
              Each uses a 1-9 scale. These are separate engagements we recommend after TRL
              surfaces where your organization needs to focus next.
            </p>
          </div>
        </div>
      </div>
    </section>

    <!-- CTA -->
    <section id="cta" class="trl-section trl-cta">
      <div class="trl-container">
        <span class="trl-tag trl-tag-teal" style="color:var(--trl-teal);border-color:var(--trl-teal)">GET STARTED</span>
        <h2 style="color:#fff">Your technology claims should match your technology reality.</h2>
        <p>
          This is our general offering. Upon discussion we tailor the audit to your specific
          technology, stage of maturation, target use case, and commercial objectives.
        </p>
        <div class="trl-cta-row">
          <a href="mailto:hello@thedeveco.com" class="trl-btn trl-btn-primary">Contact Us</a>
          <a
            href="https://thedeveco.com"
            target="_blank"
            rel="noopener noreferrer"
            class="trl-btn trl-btn-outline"
          >thedeveco.com</a>
        </div>
        <div class="trl-cta-footer">
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
          <span class="trl-small" style="color:rgba(255,255,255,0.25)">Est. 2024</span>
        </div>
      </div>
    </section>
  </div>
</template>

<style>
/* Scroll-snap applies only when this page is mounted. */
html.trl-page {
  scroll-snap-type: y mandatory;
  scroll-behavior: smooth;
}

/* Noise overlay scoped to the TRL page only. */
html.trl-page body::after {
  content: '';
  position: fixed;
  inset: 0;
  pointer-events: none;
  z-index: 9999;
  opacity: 0.025;
  background: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
}

/* Everything below is namespaced under .trl to prevent leakage. */
.trl {
  /* Local palette (prefixed to avoid collision with global tokens in App.vue) */
  --trl-bg: #f0ede4;
  --trl-bg-alt: #e8e5dc;
  --trl-ink: #1e3a3a;
  --trl-teal: #4db3a8;
  --trl-teal-dark: #3d9991;
  --trl-teal-deep: #1e3a3a;
  --trl-gold: #d4a843;
  --trl-gold-dim: #c49a3a;
  --trl-paper: #f4f1e8;
  --trl-red: #c45b5b;
  --trl-gray: #8a9090;
  --trl-gray-light: #c8c5bc;

  font-family: 'JetBrains Mono', monospace;
  background: var(--trl-bg);
  color: var(--trl-ink);
  line-height: 1.6;
  overflow-x: hidden;
}

.trl *,
.trl *::before,
.trl *::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.trl .trl-section {
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

.trl .trl-container {
  max-width: 960px;
  width: 100%;
}

/* Nav dots */
.trl-nav-dots {
  position: fixed;
  right: 1.5rem;
  top: 50%;
  transform: translateY(-50%);
  z-index: 100;
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.trl-nav-dot {
  width: 8px;
  height: 8px;
  border: 1.5px solid var(--trl-teal-dark, #3d9991);
  border-radius: 50%;
  background: transparent;
  cursor: pointer;
  transition: all 0.3s ease;
  padding: 0;
}

.trl-nav-dot.active {
  background: var(--trl-teal, #4db3a8);
  border-color: var(--trl-teal, #4db3a8);
  transform: scale(1.4);
}

.trl-nav-dot:hover {
  border-color: var(--trl-teal, #4db3a8);
}

/* Typography */
.trl h1 {
  font-size: clamp(2rem, 5vw, 3.4rem);
  font-weight: 800;
  letter-spacing: -0.02em;
  line-height: 1.1;
}

.trl h2 {
  font-size: clamp(1.3rem, 2.8vw, 1.8rem);
  font-weight: 700;
  line-height: 1.25;
  margin-bottom: 0.6rem;
}

.trl h3 {
  font-size: 0.7rem;
  font-weight: 600;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: var(--trl-teal-dark);
  margin-bottom: 0.5rem;
}

.trl p {
  font-size: 0.82rem;
  font-weight: 400;
  max-width: 580px;
  color: #4a5555;
  line-height: 1.6;
}

.trl .trl-small {
  font-size: 0.7rem;
  color: var(--trl-gray);
}

/* Tags */
.trl .trl-tag {
  display: inline-block;
  font-size: 0.6rem;
  font-weight: 600;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  padding: 2px 8px;
  border: 1.5px dashed;
  margin-bottom: 0.75rem;
}

.trl .trl-tag-teal {
  color: var(--trl-teal-dark);
  border-color: var(--trl-teal);
}

.trl .trl-tag-gold {
  color: var(--trl-gold-dim);
  border-color: var(--trl-gold);
}

.trl .trl-tag-red {
  color: var(--trl-red);
  border-color: var(--trl-red);
}

/* HERO */
.trl .trl-hero {
  background: var(--trl-teal-deep);
  color: #fff;
}

.trl .trl-hero p {
  color: rgba(255, 255, 255, 0.6);
}

.trl .trl-hero h3 {
  color: var(--trl-teal);
}

.trl .trl-hero-logo {
  margin-bottom: 2rem;
}

.trl .trl-hero-title {
  color: #fff;
  margin-bottom: 0.4rem;
}

.trl .trl-accent {
  color: var(--trl-teal);
}

.trl .trl-hero-sub {
  font-size: clamp(0.95rem, 2vw, 1.2rem);
  font-weight: 400;
  color: rgba(255, 255, 255, 0.45);
  margin-bottom: 1.5rem;
}

.trl .trl-hero-tagline {
  font-size: 0.8rem;
  color: rgba(255, 255, 255, 0.5);
  max-width: 520px;
  line-height: 1.65;
}

.trl .trl-scroll-hint {
  position: absolute;
  bottom: 1.5rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 4px;
  color: rgba(255, 255, 255, 0.25);
  animation: trl-nudge 2s ease infinite;
}

.trl .trl-scroll-hint svg {
  width: 18px;
  height: 18px;
  stroke: currentColor;
  fill: none;
  stroke-width: 2;
  stroke-linecap: round;
  stroke-linejoin: round;
}

@keyframes trl-nudge {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(5px); }
}

/* PROBLEM */
.trl .trl-problem {
  background: var(--trl-bg);
}

.trl .trl-problem-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1.25rem;
  margin-top: 1.25rem;
}

.trl .trl-problem-card {
  background: var(--trl-paper);
  border: 1px solid var(--trl-gray-light);
  padding: 1rem 1.15rem;
}

.trl .trl-problem-card h4 {
  font-size: 0.78rem;
  font-weight: 700;
  margin-bottom: 0.4rem;
}

.trl .trl-problem-card p {
  font-size: 0.72rem;
  line-height: 1.55;
}

.trl .trl-icon-box {
  width: 28px;
  height: 28px;
  margin-bottom: 0.6rem;
  color: var(--trl-teal-dark);
}

.trl .trl-icon-box svg {
  width: 100%;
  height: 100%;
  stroke: currentColor;
  fill: none;
  stroke-width: 1.5;
  stroke-linecap: round;
  stroke-linejoin: round;
}

/* PIPELINE */
.trl .trl-pipeline {
  background: var(--trl-bg-alt);
}

.trl .trl-pipeline-layout {
  display: grid;
  grid-template-columns: 240px 1fr;
  gap: 2.5rem;
  margin-top: 1.25rem;
  align-items: start;
}

.trl .trl-pipeline-diagram {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.trl .trl-pipe-node {
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

.trl .trl-pipe-rect {
  background: var(--trl-paper);
  border: 1.5px solid var(--trl-gray-light);
  color: var(--trl-ink);
  border-radius: 3px;
}

.trl .trl-pipe-rect-wide {
  background: var(--trl-paper);
  border: 1.5px solid var(--trl-gray-light);
  color: var(--trl-ink);
  border-radius: 3px;
  min-width: 200px;
  padding: 14px 12px;
}

.trl .trl-pipe-inner {
  padding: 4px 10px;
  background: rgba(255, 255, 255, 0.45);
  border-radius: 3px;
  border: 1px solid rgba(255, 255, 255, 0.3);
}

.trl .trl-pipe-inner-teal {
  padding: 4px 10px;
  background: rgba(255, 255, 255, 0.2);
  border-radius: 3px;
  border: 1px solid rgba(255, 255, 255, 0.15);
}

.trl .trl-pipe-group-gold {
  background: var(--trl-gold);
  padding: 10px 14px 14px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0;
  clip-path: polygon(0% 0%, 100% 0%, 75% 100%, 25% 100%);
  min-width: 210px;
}

.trl .trl-pipe-group-gold .trl-pipe-node {
  background: none;
  border: none;
  color: var(--trl-teal-deep);
  min-width: auto;
  padding: 3px 8px;
}

.trl .trl-pipe-group-gold .trl-pipe-label {
  font-size: 0.42rem;
  font-weight: 600;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--trl-teal-deep);
  opacity: 0.7;
  margin-top: 2px;
}

.trl .trl-pipe-group-teal {
  background: var(--trl-teal);
  padding: 12px 14px 14px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0;
  clip-path: polygon(5% 0%, 95% 0%, 80% 100%, 20% 100%);
  min-width: 210px;
}

.trl .trl-pipe-group-teal .trl-pipe-node {
  background: none;
  border: none;
  color: var(--trl-teal-deep);
  min-width: auto;
  padding: 3px 8px;
}

.trl .trl-pipe-group-teal .trl-pipe-label {
  font-size: 0.42rem;
  font-weight: 600;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--trl-teal-deep);
  opacity: 0.7;
  margin-top: 2px;
}

.trl .trl-pipe-arrow {
  width: 1.5px;
  height: 10px;
  background: var(--trl-red);
  position: relative;
}

.trl .trl-pipe-arrow::after {
  content: '';
  position: absolute;
  bottom: -3px;
  left: 50%;
  transform: translateX(-50%);
  border-left: 3.5px solid transparent;
  border-right: 3.5px solid transparent;
  border-top: 5px solid var(--trl-red);
}

.trl .trl-pipe-arrow-inner {
  width: 1.5px;
  height: 6px;
  background: var(--trl-red);
  position: relative;
}

.trl .trl-pipe-arrow-inner::after {
  content: '';
  position: absolute;
  bottom: -2px;
  left: 50%;
  transform: translateX(-50%);
  border-left: 3px solid transparent;
  border-right: 3px solid transparent;
  border-top: 4px solid var(--trl-red);
}

.trl .trl-pipe-branch {
  display: flex;
  gap: 4px;
  position: relative;
}

.trl .trl-pipe-branch-item {
  font-size: 0.45rem;
  font-weight: 600;
  padding: 2px 6px;
  border: 1px solid var(--trl-red);
  color: var(--trl-red);
  text-transform: uppercase;
  border-radius: 2px;
}

.trl .trl-pipeline-text {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.trl .trl-pipeline-text h4 {
  font-size: 0.88rem;
  font-weight: 700;
  margin-bottom: 0.3rem;
}

.trl .trl-pipeline-text p {
  font-size: 0.75rem;
  line-height: 1.6;
  max-width: none;
}

.trl .trl-pipeline-callout {
  font-size: 0.72rem;
  line-height: 1.6;
  color: var(--trl-ink);
  padding: 0.75rem 1rem;
  border-left: 3px solid var(--trl-teal);
  background: rgba(77, 179, 168, 0.06);
}

/* SCALE */
.trl .trl-scale {
  background: var(--trl-teal-deep);
  color: #fff;
}

.trl .trl-scale h3 {
  color: var(--trl-teal);
}

.trl .trl-scale p {
  color: rgba(255, 255, 255, 0.55);
}

.trl .trl-scale-wrapper {
  margin-top: 1rem;
  width: 100%;
}

.trl .trl-scale-table {
  width: 100%;
  border-collapse: collapse;
  font-size: 0.68rem;
}

.trl .trl-scale-table thead th {
  font-size: 0.55rem;
  font-weight: 700;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--trl-teal);
  padding: 6px 8px;
  border-bottom: 1.5px solid rgba(255, 255, 255, 0.1);
  text-align: left;
}

.trl .trl-scale-table thead th:first-child {
  text-align: center;
  width: 44px;
}

.trl .trl-scale-table tbody tr {
  border-bottom: 1px solid rgba(255, 255, 255, 0.05);
  cursor: pointer;
  transition: all 0.2s ease;
}

.trl .trl-scale-table tbody tr:hover {
  background: rgba(77, 179, 168, 0.05);
}

.trl .trl-scale-table tbody tr.highlighted {
  background: rgba(77, 179, 168, 0.1);
}

.trl .trl-scale-table tbody td {
  padding: 5px 8px;
  vertical-align: top;
  line-height: 1.4;
}

.trl .trl-scale-table tbody td:first-child {
  text-align: center;
  font-weight: 800;
}

.trl .trl-scale-level-num {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 26px;
  height: 26px;
  font-size: 0.8rem;
  border-radius: 2px;
}

.trl .trl-scale-phase {
  font-weight: 600;
  color: #fff;
  white-space: nowrap;
}

.trl .trl-scale-def {
  color: rgba(255, 255, 255, 0.5);
}

.trl .trl-scale-product {
  color: rgba(255, 255, 255, 0.35);
  font-size: 0.62rem;
  font-style: italic;
}

.trl .trl-scale-criteria {
  display: none;
  color: rgba(255, 255, 255, 0.5);
  font-size: 0.62rem;
  padding-top: 4px;
  border-top: 1px solid rgba(255, 255, 255, 0.06);
  margin-top: 4px;
}

.trl .trl-scale-table tbody tr.highlighted .trl-scale-criteria {
  display: block;
}

.trl .trl-scale-footer {
  margin-top: 1rem;
  font-size: 0.72rem;
  color: rgba(255, 255, 255, 0.4);
  line-height: 1.6;
  max-width: 680px;
}

.trl .trl-scale-footer strong {
  color: rgba(255, 255, 255, 0.65);
  font-weight: 600;
}

/* KB FOUNDATION */
.trl .trl-kb {
  background: var(--trl-bg);
}

.trl .trl-kb-layout {
  display: grid;
  grid-template-columns: 1.15fr 1fr;
  gap: 2rem;
  margin-top: 1.25rem;
  align-items: center;
}

.trl .trl-kb-text {
  display: flex;
  flex-direction: column;
  gap: 0.9rem;
}

.trl .trl-kb-text p {
  font-size: 0.75rem;
  line-height: 1.6;
  max-width: none;
}

.trl .trl-kb-text h4 {
  font-size: 0.85rem;
  font-weight: 700;
  margin-bottom: 0.3rem;
}

.trl .trl-kb-callout {
  font-size: 0.72rem;
  line-height: 1.6;
  color: var(--trl-ink);
  padding: 0.75rem 1rem;
  border-left: 3px solid var(--trl-gold);
  background: rgba(212, 168, 67, 0.08);
  margin-top: 0.3rem;
}

.trl .trl-kb-diagram {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.3rem;
  padding: 0.5rem 0;
}

.trl .trl-kb-tier-label {
  font-size: 0.5rem;
  font-weight: 700;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  color: var(--trl-teal-dark);
  margin-bottom: 0.25rem;
}

.trl .trl-kb-pills {
  display: flex;
  flex-wrap: wrap;
  gap: 0.4rem;
  justify-content: center;
  max-width: 340px;
}

.trl .trl-kb-pill {
  font-size: 0.55rem;
  font-weight: 600;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  padding: 6px 10px;
  background: var(--trl-paper);
  border: 1.5px solid var(--trl-gray-light);
  color: var(--trl-ink);
  border-radius: 2px;
}

.trl .trl-kb-pill-out {
  border-color: var(--trl-teal);
  color: var(--trl-teal-dark);
  background: rgba(77, 179, 168, 0.08);
}

.trl .trl-kb-arrow-v {
  width: 1.5px;
  height: 18px;
  background: var(--trl-gold);
  position: relative;
  margin: 0.3rem 0;
}

.trl .trl-kb-arrow-v::after {
  content: '';
  position: absolute;
  bottom: -3px;
  left: 50%;
  transform: translateX(-50%);
  border-left: 4px solid transparent;
  border-right: 4px solid transparent;
  border-top: 6px solid var(--trl-gold);
}

.trl .trl-kb-core {
  background: var(--trl-teal);
  color: var(--trl-teal-deep);
  padding: 1.1rem 1.6rem;
  text-align: center;
  clip-path: polygon(4% 0%, 96% 0%, 100% 50%, 96% 100%, 4% 100%, 0% 50%);
  min-width: 280px;
}

.trl .trl-kb-core-title {
  font-size: 0.95rem;
  font-weight: 800;
  letter-spacing: 0.1em;
  margin-bottom: 0.2rem;
}

.trl .trl-kb-core-sub {
  font-size: 0.62rem;
  font-weight: 600;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  opacity: 0.8;
  margin-bottom: 0.35rem;
}

.trl .trl-kb-core-meta {
  font-size: 0.52rem;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  opacity: 0.55;
}

/* DELIVERABLES */
.trl .trl-deliverables {
  background: var(--trl-bg-alt);
}

.trl .trl-deliverables-list {
  margin-top: 1.25rem;
}

.trl .trl-deliverable-item {
  display: grid;
  grid-template-columns: 28px 1fr;
  gap: 0.75rem;
  align-items: start;
  padding: 0.8rem 0;
  border-bottom: 1px solid var(--trl-gray-light);
}

.trl .trl-deliverable-item:first-child {
  border-top: 1px solid var(--trl-gray-light);
}

.trl .trl-deliverable-num {
  font-size: 0.65rem;
  font-weight: 700;
  color: var(--trl-teal-dark);
  padding-top: 1px;
}

.trl .trl-deliverable-text {
  font-size: 0.78rem;
  color: var(--trl-ink);
  line-height: 1.5;
}

.trl .trl-deliverable-text strong {
  font-weight: 700;
}

/* METHODOLOGY */
.trl .trl-methodology {
  background: var(--trl-bg);
}

.trl .trl-method-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1.25rem;
  margin-top: 1.25rem;
}

.trl .trl-method-card {
  background: var(--trl-paper);
  border: 1px solid var(--trl-gray-light);
  padding: 1rem 1.15rem;
}

.trl .trl-method-card h4 {
  font-size: 0.78rem;
  font-weight: 700;
  margin-bottom: 0.35rem;
}

.trl .trl-method-card p {
  font-size: 0.72rem;
  line-height: 1.55;
  max-width: none;
}

.trl .trl-method-num {
  font-size: 0.6rem;
  font-weight: 700;
  color: var(--trl-teal);
  margin-bottom: 0.35rem;
  display: block;
}

/* PROCESS */
.trl .trl-process {
  background: var(--trl-bg-alt);
}

.trl .trl-process-steps {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 1.25rem;
  margin-top: 1.25rem;
}

.trl .trl-process-step {
  text-align: center;
  position: relative;
}

.trl .trl-process-step::after {
  content: '';
  position: absolute;
  top: 20px;
  right: -0.625rem;
  width: 1.25rem;
  height: 1.5px;
  background: var(--trl-gray-light);
}

.trl .trl-process-step:last-child::after {
  display: none;
}

.trl .trl-step-hex {
  width: 40px;
  height: 40px;
  margin: 0 auto 0.6rem;
}

.trl .trl-step-hex svg {
  width: 100%;
  height: 100%;
}

.trl .trl-process-step h4 {
  font-size: 0.78rem;
  font-weight: 700;
  margin-bottom: 0.3rem;
}

.trl .trl-process-step p {
  font-size: 0.68rem;
  color: #5a6868;
  max-width: none;
  line-height: 1.5;
}

/* TRL / WHAT COMES AFTER */
.trl .trl-next {
  background: var(--trl-bg-alt);
}

.trl .trl-next-layout {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 2rem;
  margin-top: 1.25rem;
  align-items: start;
}

.trl .trl-next-text p {
  font-size: 0.78rem;
  line-height: 1.6;
  max-width: none;
}

.trl .trl-next-text h4 {
  font-size: 0.88rem;
  font-weight: 700;
  margin-bottom: 0.4rem;
}

.trl .trl-next-text h4.trl-sub {
  margin-top: 1rem;
}

.trl .trl-next-mini-table {
  width: 100%;
  border-collapse: collapse;
  font-size: 0.65rem;
}

.trl .trl-next-mini-table th {
  font-size: 0.55rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--trl-teal-dark);
  padding: 5px 6px;
  border-bottom: 1.5px solid var(--trl-gray-light);
  text-align: left;
}

.trl .trl-next-mini-table th:first-child {
  width: 36px;
  text-align: center;
}

.trl .trl-next-mini-table td {
  padding: 4px 6px;
  border-bottom: 1px solid rgba(0, 0, 0, 0.04);
  vertical-align: top;
  line-height: 1.4;
}

.trl .trl-next-mini-table td:first-child {
  text-align: center;
  font-weight: 700;
  color: var(--trl-teal-dark);
}

.trl .trl-next-phase {
  font-weight: 600;
}

.trl .trl-next-badges {
  display: flex;
  gap: 0.75rem;
  margin-top: 1.25rem;
  flex-wrap: wrap;
}

.trl .trl-next-badge {
  padding: 0.5rem 0.75rem;
  border: 1.5px solid var(--trl-gray-light);
  background: var(--trl-paper);
  text-align: center;
}

.trl .trl-next-badge-label {
  font-size: 0.55rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--trl-teal-dark);
  margin-bottom: 2px;
}

.trl .trl-next-badge-desc {
  font-size: 0.62rem;
  color: var(--trl-gray);
}

/* CTA */
.trl .trl-cta {
  background: var(--trl-teal-deep);
  color: #fff;
}

.trl .trl-cta h3 {
  color: var(--trl-teal);
}

.trl .trl-cta p {
  color: rgba(255, 255, 255, 0.5);
}

.trl .trl-cta-row {
  display: flex;
  gap: 1rem;
  margin-top: 1.5rem;
  align-items: center;
}

.trl .trl-btn {
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

.trl .trl-btn-primary {
  background: var(--trl-teal);
  color: var(--trl-teal-deep);
}

.trl .trl-btn-primary:hover {
  background: var(--trl-teal-dark);
}

.trl .trl-btn-outline {
  background: transparent;
  color: rgba(255, 255, 255, 0.5);
  border: 1.5px solid rgba(255, 255, 255, 0.2);
}

.trl .trl-btn-outline:hover {
  border-color: var(--trl-teal);
  color: var(--trl-teal);
}

.trl .trl-cta-footer {
  margin-top: 2rem;
  padding-top: 1.5rem;
  border-top: 1px solid rgba(255, 255, 255, 0.07);
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
}

/* Animations */
.trl .fade-in {
  opacity: 0;
  transform: translateY(14px);
  transition: opacity 0.5s ease, transform 0.5s ease;
}

.trl .fade-in.visible {
  opacity: 1;
  transform: translateY(0);
}

.trl .stagger > * {
  opacity: 0;
  transform: translateY(10px);
}

.trl .stagger.visible > *:nth-child(1) {
  animation: trl-fadeUp 0.4s ease forwards 0.05s;
}

.trl .stagger.visible > *:nth-child(2) {
  animation: trl-fadeUp 0.4s ease forwards 0.1s;
}

.trl .stagger.visible > *:nth-child(3) {
  animation: trl-fadeUp 0.4s ease forwards 0.15s;
}

.trl .stagger.visible > *:nth-child(4) {
  animation: trl-fadeUp 0.4s ease forwards 0.2s;
}

.trl .stagger.visible > *:nth-child(5) {
  animation: trl-fadeUp 0.4s ease forwards 0.25s;
}

.trl .stagger.visible > *:nth-child(6) {
  animation: trl-fadeUp 0.4s ease forwards 0.3s;
}

.trl .stagger.visible > *:nth-child(7) {
  animation: trl-fadeUp 0.4s ease forwards 0.35s;
}

@keyframes trl-fadeUp {
  to { opacity: 1; transform: translateY(0); }
}

/* Responsive */
@media (max-width: 768px) {
  .trl .trl-section {
    padding: 1.5rem 1.25rem;
  }
  .trl .trl-problem-grid,
  .trl .trl-method-grid {
    grid-template-columns: 1fr;
  }
  .trl .trl-process-steps {
    grid-template-columns: 1fr 1fr;
    gap: 1.5rem;
  }
  .trl .trl-process-step::after {
    display: none;
  }
  .trl .trl-cta-footer {
    flex-direction: column;
    gap: 1rem;
    text-align: center;
  }
  .trl-nav-dots {
    right: 0.75rem;
  }
  .trl .trl-pipeline-layout,
  .trl .trl-next-layout,
  .trl .trl-kb-layout {
    grid-template-columns: 1fr;
  }
  .trl .trl-scale-table {
    font-size: 0.6rem;
  }
  .trl .trl-scale-table thead th {
    padding: 5px 4px;
    font-size: 0.5rem;
  }
  .trl .trl-scale-table tbody td {
    padding: 4px;
  }
  .trl .trl-scale-level-num {
    width: 22px;
    height: 22px;
    font-size: 0.7rem;
  }
}

@media (max-width: 480px) {
  .trl .trl-process-steps {
    grid-template-columns: 1fr;
  }
  .trl .trl-cta-row {
    flex-direction: column;
  }
  .trl .trl-pipe-node {
    min-width: 110px;
    font-size: 0.45rem;
  }
}
</style>
