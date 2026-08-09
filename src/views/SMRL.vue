<script setup lang="ts">
import { ref, onMounted, onUnmounted, nextTick } from 'vue'

// Section list drives nav dot rendering and IntersectionObserver targeting.
const sectionList = [
  { id: 'hero', label: 'Hero' },
  { id: 'problem', label: 'The Problem' },
  { id: 'pipeline', label: 'Social Pipeline' },
  { id: 'scale', label: 'SMRL Scale' },
  { id: 'kb', label: 'KB Foundation' },
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
    title: 'Platform Sprawl',
    body: 'Presence on every platform means excellence on none. Your team burns out maintaining accounts nobody owns, measures, or believes in.',
  },
  {
    title: 'Voice Dissonance',
    body: 'Your brand sounds different on every channel. Audiences lose trust when the voice does not match the promise, and they scroll past quietly.',
  },
  {
    title: 'Invisible ROI',
    body: 'Budget flows to social but attribution is broken. You cannot tell which dollars are working and which are decoration. Reporting fills the gap with vanity metrics.',
  },
  {
    title: 'Unsustainable Pace',
    body: 'Content calendars built for teams you do not have. Burnout is baked into the plan. Results disappear when one person takes a week off.',
  },
  {
    title: 'No Source of Truth',
    body: 'Brand voice, positioning, and facts live scattered across decks, docs, and Slack threads. When your own team cannot find them, AI tools cannot either.',
  },
  {
    title: 'AI-Era Drift',
    body: 'LLMs, chatbots, and assistants are already representing your brand to the world. Without a source of truth, they hallucinate your voice, your products, and your claims.',
  },
]

const deliverables: Deliverable[] = [
  {
    num: '01',
    title: 'Platform & Channel Audit',
    body: 'Every active platform assessed for audience fit, content performance, and resource sustainability. Clear recommendations for what to cut, what to double down on, and what to leave alone.',
  },
  {
    num: '02',
    title: 'Audience Map',
    body: 'Who you are actually reaching by platform versus who you think you are reaching. Segment analysis and ICP alignment with platform-native behavior patterns.',
  },
  {
    num: '03',
    title: 'Voice & Brand Consistency Report',
    body: 'How your voice lands across channels, with dissonance flagged and rewrite guidance for each platform you keep.',
  },
  {
    num: '04',
    title: 'Content & Engagement Analysis',
    body: 'What is working, what is wasting time. Performance by format, topic, and platform. Engagement patterns tied to conversion signals where attribution allows.',
  },
  {
    num: '05',
    title: 'Tiered 12-Month Roadmap',
    body: 'Plan sorted by effort and impact. What you can fix by restructuring versus what requires investment. Platform-by-platform priorities sized to your actual team.',
  },
  {
    num: '06',
    title: 'Team Coaching',
    body: 'One-on-one sessions with your social and marketing staff, paired with coaching from a practitioner who has built and scaled communities across Arm, Linaro, AWS, EDGE AI Foundation, and Hackster.io.',
  },
  {
    num: '07',
    title: 'Source of Truth Blueprint',
    body: 'Structured plan for your knowledge base foundation. What exists, what needs capturing, how to organize it, and how it connects to downstream AI tools. The first seed of an ongoing KB that devEco™ builds and maintains.',
  },
]

const methodologyCards: MethodCard[] = [
  {
    letter: 'A',
    title: 'Platform-Fit Analysis',
    body: 'Every platform evaluated against audience behavior, content requirements, and your team\'s realistic output. Most brands run three platforms they should retire and miss one they should own.',
  },
  {
    letter: 'B',
    title: 'Audience & Voice Audit',
    body: 'Who you are reaching versus who you target. Whether your voice survives the platforms you post to, or flattens into noise. Dissonance between channels is measured, not guessed.',
  },
  {
    letter: 'C',
    title: 'SMRL Readiness Scoring',
    body: 'Proprietary metric evaluating presence, content, community, distribution, measurement, resource fit, and source-of-truth maturity. Scored 1-9 across dimensions to identify where focus will compound.',
  },
  {
    letter: 'D',
    title: 'Resource & Bandwidth Mapping',
    body: 'What your team can sustain without burnout. Social programs break when they outgrow the people running them. We size your ambition to your capacity, not the other way around.',
  },
]

const readinessBadges: ReadinessBadge[] = [
  { label: 'DevXRL', desc: 'Developer Experience' },
  { label: 'TRL', desc: 'Technology Maturity' },
  { label: 'G2MRL', desc: 'Go-to-Market' },
]

const scaleData: ScaleRow[] = [
  { level: 9, phase: 'Iconic', def: 'Your social presence shapes the conversation for the entire category.', criteria: 'Competitors cite or copy your framing. Content shapes industry discourse. Social becomes a durable moat, not a tactic.', product: 'Cultural Force', numStyle: 'background:rgba(77,179,168,0.25);color:var(--smrl-teal)' },
  { level: 8, phase: 'Leveraged', def: 'Small team, disproportionate impact. Social drives pipeline directly.', criteria: 'Measurable pipeline from social channels. Content engine produces multi-format output from single inputs. ROI per hour is top-decile.', product: 'Influence Engine', numStyle: 'background:rgba(77,179,168,0.2);color:var(--smrl-teal)' },
  { level: 7, phase: 'Magnetic', def: 'Audience finds you. Community forms around the content.', criteria: 'Inbound opportunities arriving through social. Engaged community replying, sharing, and defending the brand. Recognized voice in the category.', product: 'Gravity', numStyle: 'background:rgba(77,179,168,0.16);color:var(--smrl-teal)' },
  { level: 6, phase: 'Compounding', def: 'Each piece of content earns more than the last. Momentum is real.', criteria: 'Content repurposed effectively across formats. Engagement growing organically. Clear attribution for key conversions. Audience segments understood.', product: 'Flywheel', numStyle: 'background:rgba(77,179,168,0.12);color:var(--smrl-teal)' },
  { level: 5, phase: 'Focused', def: 'Right platforms, right voice, effort sized to bandwidth.', criteria: 'Platforms chosen for fit, not FOMO. Voice consistent across channels. Effort sized to team bandwidth. Metrics tied to business objectives.', product: 'Aligned', numStyle: 'background:rgba(212,168,67,0.2);color:var(--smrl-gold)' },
  { level: 4, phase: 'Functional', def: 'Basics are covered. Cadence exists. Quality is passable.', criteria: 'Regular posting on chosen platforms. Content produced on calendar. Team roles defined. Baseline engagement tracked.', product: 'Active', numStyle: 'background:rgba(212,168,67,0.15);color:var(--smrl-gold)' },
  { level: 3, phase: 'Draining', def: 'Investment happens but nothing returns. Resources spent, outcomes unclear.', criteria: 'Budget and time going in, no measurable impact out. Tool stack expensive but underused. Metrics tracked but disconnected from outcomes. Burnout forming.', product: 'Expensive', numStyle: 'background:rgba(196,91,91,0.2);color:var(--smrl-red)' },
  { level: 2, phase: 'Scattered', def: 'Presence exists but nothing compounds. Spray and pray.', criteria: 'Accounts on too many platforms. Inconsistent posting. No voice or brand discipline. Random content types. No attribution, no learning loop.', product: 'Noise', numStyle: 'background:rgba(196,91,91,0.15);color:var(--smrl-red)' },
  { level: 1, phase: 'Absent', def: 'No presence, or accounts left dormant. Brand is invisible on social.', criteria: 'No active accounts, or zombie accounts with no recent posts. No content calendar, no owner, no budget, no strategy.', product: 'Dormant', numStyle: 'background:rgba(196,91,91,0.1);color:var(--smrl-red)' },
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
  { num: 1, title: 'Discovery', desc: 'We analyze your brand, audience, and current social footprint to understand what you actually need.' },
  { num: 2, title: 'Audit', desc: 'Deep evaluation using SMRL scoring, platform-fit analysis, and audience mapping across every active channel.' },
  { num: 3, title: 'Deliver', desc: 'Complete report with scored findings, platform recommendations, and a tiered 12-month roadmap sized to your team.' },
  { num: 4, title: 'Support', desc: 'Optional follow-on for implementation. Systems that grow, not dependencies that require us forever.' },
]

const upstreamPills = ['Social Content', 'Brand Voice', 'Positioning', 'Customer Stories']
const downstreamPills = ['AI Assistants', 'Content Gen', 'Sales & Support', 'White-Label Tools']

// Nav dots + section tracking
const sectionEls = ref<HTMLElement[]>([])
const activeSectionIndex = ref(0)

function collectSections() {
  sectionEls.value = Array.from(
    document.querySelectorAll<HTMLElement>('.smrl .smrl-section')
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
    .querySelectorAll<HTMLElement>('.smrl .fade-in')
    .forEach((el) => fadeObserver!.observe(el))
}

// Scale table toggle (single row expand)
const highlightedLevel = ref<number | null>(null)
function toggleLevel(level: number) {
  highlightedLevel.value = highlightedLevel.value === level ? null : level
}

onMounted(async () => {
  document.documentElement.classList.add('smrl-page')
  await nextTick()
  collectSections()
  setupObservers()
})

onUnmounted(() => {
  document.documentElement.classList.remove('smrl-page')
  sectionObserver?.disconnect()
  fadeObserver?.disconnect()
})
</script>

<template>
  <div class="smrl">
    <!-- Nav dots -->
    <nav class="smrl-nav-dots" aria-label="Section navigation">
      <button
        v-for="(section, i) in sectionList"
        :key="section.id"
        type="button"
        class="smrl-nav-dot"
        :class="{ active: activeSectionIndex === i }"
        :aria-label="`Jump to ${section.label}`"
        @click="scrollToSection(i)"
      ></button>
    </nav>

    <!-- HERO -->
    <section id="hero" class="smrl-section smrl-hero">
      <div class="smrl-container">
        <div class="smrl-hero-logo">
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
        <h1 class="smrl-hero-title">SM<span class="smrl-accent">RL</span> Audit</h1>
        <p class="smrl-hero-sub">Social Media Readiness Levels</p>
        <p class="smrl-hero-tagline">
          A structured evaluation of your social presence, platform fit, and audience leverage. What
          to cut, what to compound, and how to plant the source of truth your brand needs to thrive
          in the AI era.
        </p>
      </div>
      <div class="smrl-scroll-hint">
        <svg viewBox="0 0 24 24">
          <path d="M12 5v14M19 12l-7 7-7-7" />
        </svg>
        <span class="smrl-small">scroll</span>
      </div>
    </section>

    <!-- PROBLEM -->
    <section id="problem" class="smrl-section smrl-problem">
      <div class="smrl-container">
        <span class="smrl-tag smrl-tag-red">THE PROBLEM</span>
        <h2>More Social, Less Leverage</h2>
        <p>
          Most brands are on too many platforms, producing too much content, for audiences they have
          not clearly defined. The result is expensive noise instead of compounding presence.
        </p>
        <div class="smrl-problem-grid fade-in">
          <div class="smrl-problem-card">
            <div class="smrl-icon-box">
              <svg viewBox="0 0 24 24">
                <circle cx="12" cy="12" r="4" />
                <path d="M12 2v2M12 20v2M4.93 4.93l1.41 1.41M17.66 17.66l1.41 1.41M2 12h2M20 12h2M4.93 19.07l1.41-1.41M17.66 6.34l1.41-1.41" />
              </svg>
            </div>
            <h4>{{ problemCards[0]!.title }}</h4>
            <p>{{ problemCards[0]!.body }}</p>
          </div>
          <div class="smrl-problem-card">
            <div class="smrl-icon-box">
              <svg viewBox="0 0 24 24">
                <path d="M3 18v-6a9 9 0 0 1 18 0v6" />
                <path d="M21 19a2 2 0 0 1-2 2h-1v-6h3zM3 19a2 2 0 0 0 2 2h1v-6H3z" />
              </svg>
            </div>
            <h4>{{ problemCards[1]!.title }}</h4>
            <p>{{ problemCards[1]!.body }}</p>
          </div>
          <div class="smrl-problem-card">
            <div class="smrl-icon-box">
              <svg viewBox="0 0 24 24">
                <line x1="12" y1="1" x2="12" y2="23" />
                <path d="M17 5H9.5a3.5 3.5 0 0 0 0 7h5a3.5 3.5 0 0 1 0 7H6" />
              </svg>
            </div>
            <h4>{{ problemCards[2]!.title }}</h4>
            <p>{{ problemCards[2]!.body }}</p>
          </div>
          <div class="smrl-problem-card">
            <div class="smrl-icon-box">
              <svg viewBox="0 0 24 24">
                <circle cx="12" cy="12" r="10" />
                <polyline points="12 6 12 12 16 14" />
              </svg>
            </div>
            <h4>{{ problemCards[3]!.title }}</h4>
            <p>{{ problemCards[3]!.body }}</p>
          </div>
          <div class="smrl-problem-card">
            <div class="smrl-icon-box">
              <svg viewBox="0 0 24 24">
                <rect x="3" y="3" width="7" height="7" rx="1" />
                <rect x="14" y="3" width="7" height="7" rx="1" />
                <rect x="3" y="14" width="7" height="7" rx="1" />
                <line x1="14" y1="14" x2="21" y2="21" />
                <line x1="21" y1="14" x2="14" y2="21" />
              </svg>
            </div>
            <h4>{{ problemCards[4]!.title }}</h4>
            <p>{{ problemCards[4]!.body }}</p>
          </div>
          <div class="smrl-problem-card">
            <div class="smrl-icon-box">
              <svg viewBox="0 0 24 24">
                <path d="M18 10h-1.26A8 8 0 1 0 9 20h9a5 5 0 0 0 0-10z" />
                <path d="M8 14l4-4 4 4" />
                <line x1="12" y1="10" x2="12" y2="17" />
              </svg>
            </div>
            <h4>{{ problemCards[5]!.title }}</h4>
            <p>{{ problemCards[5]!.body }}</p>
          </div>
        </div>
      </div>
    </section>

    <!-- PIPELINE -->
    <section id="pipeline" class="smrl-section smrl-pipeline">
      <div class="smrl-container">
        <span class="smrl-tag smrl-tag-gold">THE SOCIAL PIPELINE</span>
        <h2>Focus Over Volume</h2>
        <div class="smrl-pipeline-layout fade-in">
          <div class="smrl-pipeline-diagram">
            <div class="smrl-pipe-node smrl-pipe-rect-wide">TARGET AUDIENCE</div>
            <div class="smrl-pipe-arrow"></div>
            <div class="smrl-pipe-node smrl-pipe-rect">POSITIONING &amp; VOICE</div>
            <div class="smrl-pipe-arrow"></div>
            <div class="smrl-pipe-node smrl-pipe-rect">CONTENT HOOKS</div>
            <div class="smrl-pipe-arrow"></div>
            <div class="smrl-pipe-group-gold">
              <div class="smrl-pipe-node smrl-pipe-inner">CONTENT ENGINE</div>
              <div class="smrl-pipe-label">PRODUCTION FUNNEL</div>
              <div class="smrl-pipe-arrow-inner"></div>
              <div class="smrl-pipe-node smrl-pipe-inner">DISTRIBUTION</div>
            </div>
            <div class="smrl-pipe-arrow"></div>
            <div class="smrl-pipe-node smrl-pipe-rect">CHANNEL PORTFOLIO</div>
            <div class="smrl-pipe-arrow"></div>
            <div class="smrl-pipe-branch">
              <div class="smrl-pipe-branch-item">LinkedIn</div>
              <div class="smrl-pipe-branch-item">YouTube</div>
              <div class="smrl-pipe-branch-item">etc.</div>
            </div>
            <div class="smrl-pipe-arrow"></div>
            <div class="smrl-pipe-group-teal">
              <div class="smrl-pipe-node smrl-pipe-inner-teal">LISTEN / RESPOND / BUILD</div>
              <div class="smrl-pipe-arrow-inner"></div>
              <div class="smrl-pipe-node smrl-pipe-inner-teal">RELATIONSHIP</div>
              <div class="smrl-pipe-label">RELATIONSHIP FUNNEL</div>
              <div class="smrl-pipe-arrow-inner"></div>
              <div class="smrl-pipe-node smrl-pipe-inner-teal">PIPELINE</div>
            </div>
          </div>
          <div class="smrl-pipeline-text">
            <div>
              <h4>Where Effort Gets Wasted</h4>
              <p>
                Most brands produce too much content, for too many platforms, with too little focus.
                Strategy defines the target audience but execution never adjusts for platform fit,
                team bandwidth, or actual audience behavior. Every channel demands attention. No
                channel gets mastery.
              </p>
            </div>
            <div>
              <h4>What We Do About It</h4>
              <p>
                devEco™ identifies which platforms your brand can actually own given current
                resources, then rebuilds the pipeline so every piece of content compounds. Fewer
                platforms. Sharper voice. Higher leverage per dollar spent and per hour worked.
              </p>
            </div>
            <div class="smrl-pipeline-callout">
              The industry rewards posting volume. We measure what compounds. Presence over noise,
              leverage over sprawl, sustainability over sprints.
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- SCALE -->
    <section id="scale" class="smrl-section smrl-scale">
      <div class="smrl-container">
        <span class="smrl-tag smrl-tag-teal" style="color:var(--smrl-teal);border-color:var(--smrl-teal)">THE FRAMEWORK</span>
        <h2 style="color:#fff">SMRL Readiness Levels</h2>
        <p>Where does your social presence sit? Click any level to see the evaluation criteria.</p>
        <div class="smrl-scale-wrapper fade-in">
          <table class="smrl-scale-table">
            <thead>
              <tr>
                <th></th>
                <th>Phase</th>
                <th>Definition</th>
                <th>Presence Status</th>
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
                  <span class="smrl-scale-level-num" :style="row.numStyle">{{ row.level }}</span>
                </td>
                <td class="smrl-scale-phase">{{ row.phase }}</td>
                <td class="smrl-scale-def">
                  {{ row.def }}
                  <div class="smrl-scale-criteria">{{ row.criteria }}</div>
                </td>
                <td class="smrl-scale-product">{{ row.product }}</td>
              </tr>
            </tbody>
          </table>
        </div>
        <div class="smrl-scale-footer">
          Most brands never clear level 5. The friction is rarely creative. It is resourcing, focus,
          and the willingness to cut what is not working.
          <strong>SMRL shows organizations where they sit and what it takes to advance without burning out the team.</strong>
          We help you do less, better, and measure what matters.
        </div>
      </div>
    </section>

    <!-- KB FOUNDATION -->
    <section id="kb" class="smrl-section smrl-kb">
      <div class="smrl-container">
        <span class="smrl-tag smrl-tag-gold">THE KB FOUNDATION</span>
        <h2>Your Brand Needs a Source of Truth</h2>
        <p>
          AI tools are already consuming your brand. LLMs draft your emails, chatbots field your
          support, assistants recommend your products. Without a source of truth, they drift. With
          one, they compound.
        </p>
        <div class="smrl-kb-layout fade-in">
          <div class="smrl-kb-text">
            <div>
              <h4>The Audit Plants the Seeds</h4>
              <p>
                SMRL is where the source of truth begins. We identify what brand assets exist, what
                needs capturing, and what structure the knowledge base will require. Your social
                content is not just content. It is the richest existing surface of your brand's
                voice, and the audit turns it into a foundation the rest of the business can grow
                on.
              </p>
            </div>
            <div>
              <h4>devEco™ Grows What the Audit Plants</h4>
              <p>
                From there, we build and maintain the full KB through a comprehensive process.
                White-glove from first capture to live deployment. The KB becomes the upstream
                source for AI assistants, content generation, sales enablement, and white-label
                tools that let your customers feel the difference without ever knowing who built
                them.
              </p>
            </div>
            <div class="smrl-kb-callout">
              The next decade of brand operations runs on source-of-truth knowledge bases. The SMRL
              audit is how you plant one without turning your team into a documentation team.
            </div>
          </div>
          <div class="smrl-kb-diagram">
            <div class="smrl-kb-tier-label">Upstream Sources</div>
            <div class="smrl-kb-pills">
              <span v-for="pill in upstreamPills" :key="pill" class="smrl-kb-pill">{{ pill }}</span>
            </div>
            <div class="smrl-kb-arrow-v"></div>
            <div class="smrl-kb-core">
              <div class="smrl-kb-core-title">KNOWLEDGE BASE</div>
              <div class="smrl-kb-core-sub">Source of Truth</div>
              <div class="smrl-kb-core-meta">Built and Maintained by devEco™</div>
            </div>
            <div class="smrl-kb-arrow-v"></div>
            <div class="smrl-kb-tier-label">Downstream Surfaces</div>
            <div class="smrl-kb-pills">
              <span
                v-for="pill in downstreamPills"
                :key="pill"
                class="smrl-kb-pill smrl-kb-pill-out"
              >{{ pill }}</span>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- DELIVERABLES -->
    <section id="deliverables" class="smrl-section smrl-deliverables">
      <div class="smrl-container">
        <span class="smrl-tag smrl-tag-teal">WHAT YOU GET</span>
        <h2>Audit Deliverables</h2>
        <p>
          Concrete outputs. Not a 40-slide deck, not a hypothetical content calendar, not vanity
          metrics dressed up as strategy.
        </p>
        <div class="smrl-deliverables-list stagger fade-in">
          <div
            v-for="item in deliverables"
            :key="item.num"
            class="smrl-deliverable-item"
          >
            <span class="smrl-deliverable-num">{{ item.num }}</span>
            <span class="smrl-deliverable-text">
              <strong>{{ item.title }}</strong> : {{ item.body }}
            </span>
          </div>
        </div>
      </div>
    </section>

    <!-- METHODOLOGY -->
    <section id="methodology" class="smrl-section smrl-methodology">
      <div class="smrl-container">
        <span class="smrl-tag smrl-tag-gold">METHODOLOGY</span>
        <h2>How We Evaluate</h2>
        <p>
          Frameworks applied by people who have built and scaled communities across Arm, Linaro,
          AWS, EDGE AI Foundation, and Hackster.io.
        </p>
        <div class="smrl-method-grid fade-in">
          <div
            v-for="card in methodologyCards"
            :key="card.letter"
            class="smrl-method-card"
          >
            <span class="smrl-method-num">{{ card.letter }}</span>
            <h4>{{ card.title }}</h4>
            <p>{{ card.body }}</p>
          </div>
        </div>
      </div>
    </section>

    <!-- PROCESS -->
    <section id="process" class="smrl-section smrl-process">
      <div class="smrl-container">
        <span class="smrl-tag smrl-tag-teal">OUR PROCESS</span>
        <h2>From Discovery to Delivery</h2>
        <p>Embedded partner, not external auditor. We work alongside your team.</p>
        <div class="smrl-process-steps fade-in">
          <div
            v-for="step in processSteps"
            :key="step.num"
            class="smrl-process-step"
          >
            <div class="smrl-step-hex">
              <svg viewBox="0 0 56 56">
                <polygon
                  points="28,2 52,15 52,41 28,54 4,41 4,15"
                  fill="var(--smrl-teal)"
                  stroke="var(--smrl-teal-deep)"
                  stroke-width="1.5"
                />
                <text
                  x="28"
                  y="33"
                  text-anchor="middle"
                  font-family="'JetBrains Mono',monospace"
                  font-size="18"
                  font-weight="700"
                  fill="var(--smrl-teal-deep)"
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
    <section id="trl" class="smrl-section smrl-trl">
      <div class="smrl-container">
        <span class="smrl-tag smrl-tag-gold">WHAT COMES NEXT</span>
        <h2>Additional Readiness Assessments</h2>
        <p>
          SMRL covers your social presence. For a complete view of your organization's readiness,
          we offer three additional frameworks as separate engagements.
        </p>
        <div class="smrl-trl-layout fade-in">
          <div class="smrl-trl-text">
            <h4>Developer Experience Readiness (DevXRL)</h4>
            <p>
              A structured evaluation of your developer journey, documentation, content pipelines,
              and community engagement. DevXRL identifies where developers drop off, where your
              pipeline leaks, and how to turn developer interest into measurable business outcomes.
              Applies when your audience is technical.
            </p>
            <table class="smrl-trl-mini-table" style="margin-top:0.75rem">
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
                  <td class="smrl-trl-phase">{{ row.phase }}</td>
                  <td>{{ row.status }}</td>
                </tr>
              </tbody>
            </table>
          </div>
          <div class="smrl-trl-text">
            <h4>Technology Readiness Levels (TRL)</h4>
            <p>
              Adapted from NASA and widely used in hardware and deep-tech, TRL provides a clear path
              from fundamental research to market-ready product. We map your technology's maturity
              and build a stage-by-stage roadmap with focused time-to-market objectives.
            </p>
            <h4 class="smrl-sub">Go-to-Market Readiness (G2MRL)</h4>
            <p>
              A custom framework we build per engagement that assesses your sales, marketing, and
              partnership readiness alongside technical and audience maturity. G2MRL maps how
              prepared your organization is to convert interest into pipeline activity and revenue.
            </p>
            <p style="margin-top:0.75rem">
              Technology can be mature while developer experience lags behind. Social can be strong
              while go-to-market is broken. These gaps are where adoption stalls. By combining
              frameworks, we give you a complete picture.
            </p>
            <div class="smrl-trl-badges">
              <div
                v-for="badge in readinessBadges"
                :key="badge.label"
                class="smrl-trl-badge"
              >
                <div class="smrl-trl-badge-label">{{ badge.label }}</div>
                <div class="smrl-trl-badge-desc">{{ badge.desc }}</div>
              </div>
            </div>
            <p style="margin-top:0.75rem;font-size:0.72rem;color:var(--smrl-gray)">
              Each uses a 1-9 scale. These are separate engagements we recommend after SMRL surfaces
              where your organization needs to focus next.
            </p>
          </div>
        </div>
      </div>
    </section>

    <!-- CTA -->
    <section id="cta" class="smrl-section smrl-cta">
      <div class="smrl-container">
        <span class="smrl-tag smrl-tag-teal" style="color:var(--smrl-teal);border-color:var(--smrl-teal)">GET STARTED</span>
        <h2 style="color:#fff">Your brand deserves social leverage, not social sprawl.</h2>
        <p>
          This is our general offering. Upon discussion we tailor the audit to your specific
          platforms, team, audience, and available resources.
        </p>
        <div class="smrl-cta-row">
          <a href="mailto:hello@thedeveco.com" class="smrl-btn smrl-btn-primary">Contact Us</a>
          <a
            href="https://thedeveco.com"
            target="_blank"
            rel="noopener noreferrer"
            class="smrl-btn smrl-btn-outline"
          >thedeveco.com</a>
        </div>
        <div class="smrl-cta-footer">
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
          <span class="smrl-small" style="color:rgba(255,255,255,0.25)">Est. 2024</span>
        </div>
      </div>
    </section>
  </div>
</template>

<style>
/* Scroll-snap applies only when this page is mounted. */
html.smrl-page {
  scroll-snap-type: y mandatory;
  scroll-behavior: smooth;
}

/* Noise overlay scoped to the SMRL page only. */
html.smrl-page body::after {
  content: '';
  position: fixed;
  inset: 0;
  pointer-events: none;
  z-index: 9999;
  opacity: 0.025;
  background: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
}

/* Everything below is namespaced under .smrl to prevent leakage. */
.smrl {
  /* Local palette (prefixed to avoid collision with global tokens in App.vue) */
  --smrl-bg: #f0ede4;
  --smrl-bg-alt: #e8e5dc;
  --smrl-ink: #1e3a3a;
  --smrl-teal: #4db3a8;
  --smrl-teal-dark: #3d9991;
  --smrl-teal-deep: #1e3a3a;
  --smrl-gold: #d4a843;
  --smrl-gold-dim: #c49a3a;
  --smrl-paper: #f4f1e8;
  --smrl-red: #c45b5b;
  --smrl-gray: #8a9090;
  --smrl-gray-light: #c8c5bc;

  font-family: 'JetBrains Mono', monospace;
  background: var(--smrl-bg);
  color: var(--smrl-ink);
  line-height: 1.6;
  overflow-x: hidden;
}

.smrl *,
.smrl *::before,
.smrl *::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.smrl .smrl-section {
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

.smrl .smrl-container {
  max-width: 960px;
  width: 100%;
}

/* Nav dots */
.smrl-nav-dots {
  position: fixed;
  right: 1.5rem;
  top: 50%;
  transform: translateY(-50%);
  z-index: 100;
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.smrl-nav-dot {
  width: 8px;
  height: 8px;
  border: 1.5px solid var(--smrl-teal-dark, #3d9991);
  border-radius: 50%;
  background: transparent;
  cursor: pointer;
  transition: all 0.3s ease;
  padding: 0;
}

.smrl-nav-dot.active {
  background: var(--smrl-teal, #4db3a8);
  border-color: var(--smrl-teal, #4db3a8);
  transform: scale(1.4);
}

.smrl-nav-dot:hover {
  border-color: var(--smrl-teal, #4db3a8);
}

/* Typography */
.smrl h1 {
  font-size: clamp(2rem, 5vw, 3.4rem);
  font-weight: 800;
  letter-spacing: -0.02em;
  line-height: 1.1;
}

.smrl h2 {
  font-size: clamp(1.3rem, 2.8vw, 1.8rem);
  font-weight: 700;
  line-height: 1.25;
  margin-bottom: 0.6rem;
}

.smrl h3 {
  font-size: 0.7rem;
  font-weight: 600;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: var(--smrl-teal-dark);
  margin-bottom: 0.5rem;
}

.smrl p {
  font-size: 0.82rem;
  font-weight: 400;
  max-width: 580px;
  color: #4a5555;
  line-height: 1.6;
}

.smrl .smrl-small {
  font-size: 0.7rem;
  color: var(--smrl-gray);
}

/* Tags */
.smrl .smrl-tag {
  display: inline-block;
  font-size: 0.6rem;
  font-weight: 600;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  padding: 2px 8px;
  border: 1.5px dashed;
  margin-bottom: 0.75rem;
}

.smrl .smrl-tag-teal {
  color: var(--smrl-teal-dark);
  border-color: var(--smrl-teal);
}

.smrl .smrl-tag-gold {
  color: var(--smrl-gold-dim);
  border-color: var(--smrl-gold);
}

.smrl .smrl-tag-red {
  color: var(--smrl-red);
  border-color: var(--smrl-red);
}

/* HERO */
.smrl .smrl-hero {
  background: var(--smrl-teal-deep);
  color: #fff;
}

.smrl .smrl-hero p {
  color: rgba(255, 255, 255, 0.6);
}

.smrl .smrl-hero h3 {
  color: var(--smrl-teal);
}

.smrl .smrl-hero-logo {
  margin-bottom: 2rem;
}

.smrl .smrl-hero-title {
  color: #fff;
  margin-bottom: 0.4rem;
}

.smrl .smrl-accent {
  color: var(--smrl-teal);
}

.smrl .smrl-hero-sub {
  font-size: clamp(0.95rem, 2vw, 1.2rem);
  font-weight: 400;
  color: rgba(255, 255, 255, 0.45);
  margin-bottom: 1.5rem;
}

.smrl .smrl-hero-tagline {
  font-size: 0.8rem;
  color: rgba(255, 255, 255, 0.5);
  max-width: 520px;
  line-height: 1.65;
}

.smrl .smrl-scroll-hint {
  position: absolute;
  bottom: 1.5rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 4px;
  color: rgba(255, 255, 255, 0.25);
  animation: smrl-nudge 2s ease infinite;
}

.smrl .smrl-scroll-hint svg {
  width: 18px;
  height: 18px;
  stroke: currentColor;
  fill: none;
  stroke-width: 2;
  stroke-linecap: round;
  stroke-linejoin: round;
}

@keyframes smrl-nudge {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(5px); }
}

/* PROBLEM */
.smrl .smrl-problem {
  background: var(--smrl-bg);
}

.smrl .smrl-problem-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1.25rem;
  margin-top: 1.25rem;
}

.smrl .smrl-problem-card {
  background: var(--smrl-paper);
  border: 1px solid var(--smrl-gray-light);
  padding: 1rem 1.15rem;
}

.smrl .smrl-problem-card h4 {
  font-size: 0.78rem;
  font-weight: 700;
  margin-bottom: 0.4rem;
}

.smrl .smrl-problem-card p {
  font-size: 0.72rem;
  line-height: 1.55;
}

.smrl .smrl-icon-box {
  width: 28px;
  height: 28px;
  margin-bottom: 0.6rem;
  color: var(--smrl-teal-dark);
}

.smrl .smrl-icon-box svg {
  width: 100%;
  height: 100%;
  stroke: currentColor;
  fill: none;
  stroke-width: 1.5;
  stroke-linecap: round;
  stroke-linejoin: round;
}

/* PIPELINE */
.smrl .smrl-pipeline {
  background: var(--smrl-bg-alt);
}

.smrl .smrl-pipeline-layout {
  display: grid;
  grid-template-columns: 240px 1fr;
  gap: 2.5rem;
  margin-top: 1.25rem;
  align-items: start;
}

.smrl .smrl-pipeline-diagram {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.smrl .smrl-pipe-node {
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

.smrl .smrl-pipe-rect {
  background: var(--smrl-paper);
  border: 1.5px solid var(--smrl-gray-light);
  color: var(--smrl-ink);
  border-radius: 3px;
}

.smrl .smrl-pipe-rect-wide {
  background: var(--smrl-paper);
  border: 1.5px solid var(--smrl-gray-light);
  color: var(--smrl-ink);
  border-radius: 3px;
  min-width: 200px;
  padding: 14px 12px;
}

.smrl .smrl-pipe-inner {
  padding: 4px 10px;
  background: rgba(255, 255, 255, 0.45);
  border-radius: 3px;
  border: 1px solid rgba(255, 255, 255, 0.3);
}

.smrl .smrl-pipe-inner-teal {
  padding: 4px 10px;
  background: rgba(255, 255, 255, 0.2);
  border-radius: 3px;
  border: 1px solid rgba(255, 255, 255, 0.15);
}

.smrl .smrl-pipe-group-gold {
  background: var(--smrl-gold);
  padding: 10px 14px 14px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0;
  clip-path: polygon(0% 0%, 100% 0%, 75% 100%, 25% 100%);
  min-width: 210px;
}

.smrl .smrl-pipe-group-gold .smrl-pipe-node {
  background: none;
  border: none;
  color: var(--smrl-teal-deep);
  min-width: auto;
  padding: 3px 8px;
}

.smrl .smrl-pipe-group-gold .smrl-pipe-label {
  font-size: 0.42rem;
  font-weight: 600;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--smrl-teal-deep);
  opacity: 0.7;
  margin-top: 2px;
}

.smrl .smrl-pipe-group-teal {
  background: var(--smrl-teal);
  padding: 12px 14px 14px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0;
  clip-path: polygon(5% 0%, 95% 0%, 80% 100%, 20% 100%);
  min-width: 210px;
}

.smrl .smrl-pipe-group-teal .smrl-pipe-node {
  background: none;
  border: none;
  color: var(--smrl-teal-deep);
  min-width: auto;
  padding: 3px 8px;
}

.smrl .smrl-pipe-group-teal .smrl-pipe-label {
  font-size: 0.42rem;
  font-weight: 600;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--smrl-teal-deep);
  opacity: 0.7;
  margin-top: 2px;
}

.smrl .smrl-pipe-arrow {
  width: 1.5px;
  height: 10px;
  background: var(--smrl-red);
  position: relative;
}

.smrl .smrl-pipe-arrow::after {
  content: '';
  position: absolute;
  bottom: -3px;
  left: 50%;
  transform: translateX(-50%);
  border-left: 3.5px solid transparent;
  border-right: 3.5px solid transparent;
  border-top: 5px solid var(--smrl-red);
}

.smrl .smrl-pipe-arrow-inner {
  width: 1.5px;
  height: 6px;
  background: var(--smrl-red);
  position: relative;
}

.smrl .smrl-pipe-arrow-inner::after {
  content: '';
  position: absolute;
  bottom: -2px;
  left: 50%;
  transform: translateX(-50%);
  border-left: 3px solid transparent;
  border-right: 3px solid transparent;
  border-top: 4px solid var(--smrl-red);
}

.smrl .smrl-pipe-branch {
  display: flex;
  gap: 4px;
  position: relative;
}

.smrl .smrl-pipe-branch-item {
  font-size: 0.45rem;
  font-weight: 600;
  padding: 2px 6px;
  border: 1px solid var(--smrl-red);
  color: var(--smrl-red);
  text-transform: uppercase;
  border-radius: 2px;
}

.smrl .smrl-pipeline-text {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.smrl .smrl-pipeline-text h4 {
  font-size: 0.88rem;
  font-weight: 700;
  margin-bottom: 0.3rem;
}

.smrl .smrl-pipeline-text p {
  font-size: 0.75rem;
  line-height: 1.6;
  max-width: none;
}

.smrl .smrl-pipeline-callout {
  font-size: 0.72rem;
  line-height: 1.6;
  color: var(--smrl-ink);
  padding: 0.75rem 1rem;
  border-left: 3px solid var(--smrl-teal);
  background: rgba(77, 179, 168, 0.06);
}

/* SCALE */
.smrl .smrl-scale {
  background: var(--smrl-teal-deep);
  color: #fff;
}

.smrl .smrl-scale h3 {
  color: var(--smrl-teal);
}

.smrl .smrl-scale p {
  color: rgba(255, 255, 255, 0.55);
}

.smrl .smrl-scale-wrapper {
  margin-top: 1rem;
  width: 100%;
}

.smrl .smrl-scale-table {
  width: 100%;
  border-collapse: collapse;
  font-size: 0.68rem;
}

.smrl .smrl-scale-table thead th {
  font-size: 0.55rem;
  font-weight: 700;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--smrl-teal);
  padding: 6px 8px;
  border-bottom: 1.5px solid rgba(255, 255, 255, 0.1);
  text-align: left;
}

.smrl .smrl-scale-table thead th:first-child {
  text-align: center;
  width: 44px;
}

.smrl .smrl-scale-table tbody tr {
  border-bottom: 1px solid rgba(255, 255, 255, 0.05);
  cursor: pointer;
  transition: all 0.2s ease;
}

.smrl .smrl-scale-table tbody tr:hover {
  background: rgba(77, 179, 168, 0.05);
}

.smrl .smrl-scale-table tbody tr.highlighted {
  background: rgba(77, 179, 168, 0.1);
}

.smrl .smrl-scale-table tbody td {
  padding: 5px 8px;
  vertical-align: top;
  line-height: 1.4;
}

.smrl .smrl-scale-table tbody td:first-child {
  text-align: center;
  font-weight: 800;
}

.smrl .smrl-scale-level-num {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 26px;
  height: 26px;
  font-size: 0.8rem;
  border-radius: 2px;
}

.smrl .smrl-scale-phase {
  font-weight: 600;
  color: #fff;
  white-space: nowrap;
}

.smrl .smrl-scale-def {
  color: rgba(255, 255, 255, 0.5);
}

.smrl .smrl-scale-product {
  color: rgba(255, 255, 255, 0.35);
  font-size: 0.62rem;
  font-style: italic;
}

.smrl .smrl-scale-criteria {
  display: none;
  color: rgba(255, 255, 255, 0.5);
  font-size: 0.62rem;
  padding-top: 4px;
  border-top: 1px solid rgba(255, 255, 255, 0.06);
  margin-top: 4px;
}

.smrl .smrl-scale-table tbody tr.highlighted .smrl-scale-criteria {
  display: block;
}

.smrl .smrl-scale-footer {
  margin-top: 1rem;
  font-size: 0.72rem;
  color: rgba(255, 255, 255, 0.4);
  line-height: 1.6;
  max-width: 680px;
}

.smrl .smrl-scale-footer strong {
  color: rgba(255, 255, 255, 0.65);
  font-weight: 600;
}

/* KB FOUNDATION */
.smrl .smrl-kb {
  background: var(--smrl-bg);
}

.smrl .smrl-kb-layout {
  display: grid;
  grid-template-columns: 1.15fr 1fr;
  gap: 2rem;
  margin-top: 1.25rem;
  align-items: center;
}

.smrl .smrl-kb-text {
  display: flex;
  flex-direction: column;
  gap: 0.9rem;
}

.smrl .smrl-kb-text p {
  font-size: 0.75rem;
  line-height: 1.6;
  max-width: none;
}

.smrl .smrl-kb-text h4 {
  font-size: 0.85rem;
  font-weight: 700;
  margin-bottom: 0.3rem;
}

.smrl .smrl-kb-callout {
  font-size: 0.72rem;
  line-height: 1.6;
  color: var(--smrl-ink);
  padding: 0.75rem 1rem;
  border-left: 3px solid var(--smrl-gold);
  background: rgba(212, 168, 67, 0.08);
  margin-top: 0.3rem;
}

.smrl .smrl-kb-diagram {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.3rem;
  padding: 0.5rem 0;
}

.smrl .smrl-kb-tier-label {
  font-size: 0.5rem;
  font-weight: 700;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  color: var(--smrl-teal-dark);
  margin-bottom: 0.25rem;
}

.smrl .smrl-kb-pills {
  display: flex;
  flex-wrap: wrap;
  gap: 0.4rem;
  justify-content: center;
  max-width: 340px;
}

.smrl .smrl-kb-pill {
  font-size: 0.55rem;
  font-weight: 600;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  padding: 6px 10px;
  background: var(--smrl-paper);
  border: 1.5px solid var(--smrl-gray-light);
  color: var(--smrl-ink);
  border-radius: 2px;
}

.smrl .smrl-kb-pill-out {
  border-color: var(--smrl-teal);
  color: var(--smrl-teal-dark);
  background: rgba(77, 179, 168, 0.08);
}

.smrl .smrl-kb-arrow-v {
  width: 1.5px;
  height: 18px;
  background: var(--smrl-gold);
  position: relative;
  margin: 0.3rem 0;
}

.smrl .smrl-kb-arrow-v::after {
  content: '';
  position: absolute;
  bottom: -3px;
  left: 50%;
  transform: translateX(-50%);
  border-left: 4px solid transparent;
  border-right: 4px solid transparent;
  border-top: 6px solid var(--smrl-gold);
}

.smrl .smrl-kb-core {
  background: var(--smrl-teal);
  color: var(--smrl-teal-deep);
  padding: 1.1rem 1.6rem;
  text-align: center;
  clip-path: polygon(4% 0%, 96% 0%, 100% 50%, 96% 100%, 4% 100%, 0% 50%);
  min-width: 280px;
}

.smrl .smrl-kb-core-title {
  font-size: 0.95rem;
  font-weight: 800;
  letter-spacing: 0.1em;
  margin-bottom: 0.2rem;
}

.smrl .smrl-kb-core-sub {
  font-size: 0.62rem;
  font-weight: 600;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  opacity: 0.8;
  margin-bottom: 0.35rem;
}

.smrl .smrl-kb-core-meta {
  font-size: 0.52rem;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  opacity: 0.55;
}

/* DELIVERABLES */
.smrl .smrl-deliverables {
  background: var(--smrl-bg-alt);
}

.smrl .smrl-deliverables-list {
  margin-top: 1.25rem;
}

.smrl .smrl-deliverable-item {
  display: grid;
  grid-template-columns: 28px 1fr;
  gap: 0.75rem;
  align-items: start;
  padding: 0.8rem 0;
  border-bottom: 1px solid var(--smrl-gray-light);
}

.smrl .smrl-deliverable-item:first-child {
  border-top: 1px solid var(--smrl-gray-light);
}

.smrl .smrl-deliverable-num {
  font-size: 0.65rem;
  font-weight: 700;
  color: var(--smrl-teal-dark);
  padding-top: 1px;
}

.smrl .smrl-deliverable-text {
  font-size: 0.78rem;
  color: var(--smrl-ink);
  line-height: 1.5;
}

.smrl .smrl-deliverable-text strong {
  font-weight: 700;
}

/* METHODOLOGY */
.smrl .smrl-methodology {
  background: var(--smrl-bg);
}

.smrl .smrl-method-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1.25rem;
  margin-top: 1.25rem;
}

.smrl .smrl-method-card {
  background: var(--smrl-paper);
  border: 1px solid var(--smrl-gray-light);
  padding: 1rem 1.15rem;
}

.smrl .smrl-method-card h4 {
  font-size: 0.78rem;
  font-weight: 700;
  margin-bottom: 0.35rem;
}

.smrl .smrl-method-card p {
  font-size: 0.72rem;
  line-height: 1.55;
  max-width: none;
}

.smrl .smrl-method-num {
  font-size: 0.6rem;
  font-weight: 700;
  color: var(--smrl-teal);
  margin-bottom: 0.35rem;
  display: block;
}

/* PROCESS */
.smrl .smrl-process {
  background: var(--smrl-bg-alt);
}

.smrl .smrl-process-steps {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 1.25rem;
  margin-top: 1.25rem;
}

.smrl .smrl-process-step {
  text-align: center;
  position: relative;
}

.smrl .smrl-process-step::after {
  content: '';
  position: absolute;
  top: 20px;
  right: -0.625rem;
  width: 1.25rem;
  height: 1.5px;
  background: var(--smrl-gray-light);
}

.smrl .smrl-process-step:last-child::after {
  display: none;
}

.smrl .smrl-step-hex {
  width: 40px;
  height: 40px;
  margin: 0 auto 0.6rem;
}

.smrl .smrl-step-hex svg {
  width: 100%;
  height: 100%;
}

.smrl .smrl-process-step h4 {
  font-size: 0.78rem;
  font-weight: 700;
  margin-bottom: 0.3rem;
}

.smrl .smrl-process-step p {
  font-size: 0.68rem;
  color: #5a6868;
  max-width: none;
  line-height: 1.5;
}

/* TRL / WHAT COMES AFTER */
.smrl .smrl-trl {
  background: var(--smrl-bg-alt);
}

.smrl .smrl-trl-layout {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 2rem;
  margin-top: 1.25rem;
  align-items: start;
}

.smrl .smrl-trl-text p {
  font-size: 0.78rem;
  line-height: 1.6;
  max-width: none;
}

.smrl .smrl-trl-text h4 {
  font-size: 0.88rem;
  font-weight: 700;
  margin-bottom: 0.4rem;
}

.smrl .smrl-trl-text h4.smrl-sub {
  margin-top: 1rem;
}

.smrl .smrl-trl-mini-table {
  width: 100%;
  border-collapse: collapse;
  font-size: 0.65rem;
}

.smrl .smrl-trl-mini-table th {
  font-size: 0.55rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--smrl-teal-dark);
  padding: 5px 6px;
  border-bottom: 1.5px solid var(--smrl-gray-light);
  text-align: left;
}

.smrl .smrl-trl-mini-table th:first-child {
  width: 36px;
  text-align: center;
}

.smrl .smrl-trl-mini-table td {
  padding: 4px 6px;
  border-bottom: 1px solid rgba(0, 0, 0, 0.04);
  vertical-align: top;
  line-height: 1.4;
}

.smrl .smrl-trl-mini-table td:first-child {
  text-align: center;
  font-weight: 700;
  color: var(--smrl-teal-dark);
}

.smrl .smrl-trl-phase {
  font-weight: 600;
}

.smrl .smrl-trl-badges {
  display: flex;
  gap: 0.75rem;
  margin-top: 1.25rem;
  flex-wrap: wrap;
}

.smrl .smrl-trl-badge {
  padding: 0.5rem 0.75rem;
  border: 1.5px solid var(--smrl-gray-light);
  background: var(--smrl-paper);
  text-align: center;
}

.smrl .smrl-trl-badge-label {
  font-size: 0.55rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--smrl-teal-dark);
  margin-bottom: 2px;
}

.smrl .smrl-trl-badge-desc {
  font-size: 0.62rem;
  color: var(--smrl-gray);
}

/* CTA */
.smrl .smrl-cta {
  background: var(--smrl-teal-deep);
  color: #fff;
}

.smrl .smrl-cta h3 {
  color: var(--smrl-teal);
}

.smrl .smrl-cta p {
  color: rgba(255, 255, 255, 0.5);
}

.smrl .smrl-cta-row {
  display: flex;
  gap: 1rem;
  margin-top: 1.5rem;
  align-items: center;
}

.smrl .smrl-btn {
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

.smrl .smrl-btn-primary {
  background: var(--smrl-teal);
  color: var(--smrl-teal-deep);
}

.smrl .smrl-btn-primary:hover {
  background: var(--smrl-teal-dark);
}

.smrl .smrl-btn-outline {
  background: transparent;
  color: rgba(255, 255, 255, 0.5);
  border: 1.5px solid rgba(255, 255, 255, 0.2);
}

.smrl .smrl-btn-outline:hover {
  border-color: var(--smrl-teal);
  color: var(--smrl-teal);
}

.smrl .smrl-cta-footer {
  margin-top: 2rem;
  padding-top: 1.5rem;
  border-top: 1px solid rgba(255, 255, 255, 0.07);
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
}

/* Animations */
.smrl .fade-in {
  opacity: 0;
  transform: translateY(14px);
  transition: opacity 0.5s ease, transform 0.5s ease;
}

.smrl .fade-in.visible {
  opacity: 1;
  transform: translateY(0);
}

.smrl .stagger > * {
  opacity: 0;
  transform: translateY(10px);
}

.smrl .stagger.visible > *:nth-child(1) {
  animation: smrl-fadeUp 0.4s ease forwards 0.05s;
}

.smrl .stagger.visible > *:nth-child(2) {
  animation: smrl-fadeUp 0.4s ease forwards 0.1s;
}

.smrl .stagger.visible > *:nth-child(3) {
  animation: smrl-fadeUp 0.4s ease forwards 0.15s;
}

.smrl .stagger.visible > *:nth-child(4) {
  animation: smrl-fadeUp 0.4s ease forwards 0.2s;
}

.smrl .stagger.visible > *:nth-child(5) {
  animation: smrl-fadeUp 0.4s ease forwards 0.25s;
}

.smrl .stagger.visible > *:nth-child(6) {
  animation: smrl-fadeUp 0.4s ease forwards 0.3s;
}

.smrl .stagger.visible > *:nth-child(7) {
  animation: smrl-fadeUp 0.4s ease forwards 0.35s;
}

@keyframes smrl-fadeUp {
  to { opacity: 1; transform: translateY(0); }
}

/* Responsive */
@media (max-width: 768px) {
  .smrl .smrl-section {
    padding: 1.5rem 1.25rem;
  }
  .smrl .smrl-problem-grid,
  .smrl .smrl-method-grid {
    grid-template-columns: 1fr;
  }
  .smrl .smrl-process-steps {
    grid-template-columns: 1fr 1fr;
    gap: 1.5rem;
  }
  .smrl .smrl-process-step::after {
    display: none;
  }
  .smrl .smrl-cta-footer {
    flex-direction: column;
    gap: 1rem;
    text-align: center;
  }
  .smrl-nav-dots {
    right: 0.75rem;
  }
  .smrl .smrl-pipeline-layout,
  .smrl .smrl-trl-layout,
  .smrl .smrl-kb-layout {
    grid-template-columns: 1fr;
  }
  .smrl .smrl-scale-table {
    font-size: 0.6rem;
  }
  .smrl .smrl-scale-table thead th {
    padding: 5px 4px;
    font-size: 0.5rem;
  }
  .smrl .smrl-scale-table tbody td {
    padding: 4px;
  }
  .smrl .smrl-scale-level-num {
    width: 22px;
    height: 22px;
    font-size: 0.7rem;
  }
}

@media (max-width: 480px) {
  .smrl .smrl-process-steps {
    grid-template-columns: 1fr;
  }
  .smrl .smrl-cta-row {
    flex-direction: column;
  }
  .smrl .smrl-pipe-node {
    min-width: 110px;
    font-size: 0.45rem;
  }
}
</style>
