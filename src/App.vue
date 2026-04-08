<script setup lang="ts">
const codeUrl = 'https://github.com/NVIDIA/audio-flamingo'
const modelUrl = 'https://huggingface.co/nvidia/audio-flamingo-next-hf'

const authors = [
  { name: 'Sreyan Ghosh', affiliations: '1,2' },
  { name: 'Arushi Goel', affiliations: '1' },
  { name: 'Kaousheik Jayakumar', affiliations: '2' },
  { name: 'Lasha Koroshinadze', affiliations: '2' },
  { name: 'Nishit Anand', affiliations: '2' },
  { name: 'Zhifeng Kong', affiliations: '1' },
  { name: 'Siddharth Gururani', affiliations: '1' },
  { name: 'Sang-gil Lee', affiliations: '1' },
  { name: 'Jaehyeon Kim', affiliations: '1' },
  { name: 'Aya Aljafari', affiliations: '1' },
  { name: 'Chao-Han Huck Yang', affiliations: '1' },
  { name: 'Sungwon Kim', affiliations: '1' },
  { name: 'Ramani Duraiswami', affiliations: '2' },
  { name: 'Dinesh Manocha', affiliations: '2' },
  { name: 'Mohammad Shoeybi', affiliations: '1' },
  { name: 'Bryan Catanzaro', affiliations: '1' },
  { name: 'Ming-Yu Liu', affiliations: '1' },
  { name: 'Wei Ping', affiliations: '1' },
]

const topLinks = [
  { label: 'Code', href: codeUrl, primary: true, external: true },
  { label: 'Models', href: modelUrl, primary: false, external: true },
  { label: 'Benchmarks', href: '#benchmarks', primary: false, external: false },
  { label: 'Architecture', href: '#training', primary: false, external: false },
  { label: 'Demos', href: '#demo-prompts', primary: false, external: false },
  { label: 'Examples', href: '#examples', primary: false, external: false },
]

const keyFeatures = [
  'AF-Next is a generalist audio-language model for speech, environmental sound, and music rather than a music-only system.',
  'It supports long and complex audio up to 30 minutes with long-context training up to 128K tokens.',
  'Temporal Audio Chain-of-Thought grounds intermediate reasoning to timestamps for more interpretable long-audio answers.',
  'The training recipe scales beyond academic benchmarks with real-world long audio, multi-speaker speech, safety data, and multi-audio instruction data.',
  'The release is organized into AF-Next-Instruct, AF-Next-Think, and AF-Next-Captioner for QA, advanced reasoning, and detailed captioning.',
]

const benchmarks = [
  {
    area: 'General audio QA',
    benchmark: 'MMAU (avg)',
    priorModel: 'Audio Flamingo 3',
    metric: 'ACC ↑',
    prior: '72.42',
    result: '75.76',
    afNextVariant: 'AF-Next-Captioner',
  },
  {
    area: 'General audio QA',
    benchmark: 'MMAU-Pro',
    priorModel: 'Gemini 2.0 Flash',
    metric: 'ACC ↑',
    prior: '55.7',
    result: '58.7',
    afNextVariant: 'AF-Next-Think',
  },
  {
    area: 'Long-audio reasoning',
    benchmark: 'LongAudioBench',
    priorModel: 'Audio Flamingo 3',
    metric: 'GPT4o ↑',
    prior: '68.6',
    result: '73.9',
    afNextVariant: 'AF-Next-Instruct',
  },
  {
    area: 'Long-audio reasoning',
    benchmark: 'LongAudioBench + Speech',
    priorModel: 'Audio Flamingo 3',
    metric: 'GPT4o ↑',
    prior: '72.9',
    result: '81.2',
    afNextVariant: 'AF-Next-Instruct',
  },
  {
    area: 'Music transfer',
    benchmark: 'MuchoMusic',
    priorModel: 'Music Flamingo',
    metric: 'ACC ↑',
    prior: '74.5',
    result: '75.6',
    afNextVariant: 'AF-Next-Instruct',
  },
  {
    area: 'Music transfer',
    benchmark: 'NSynth',
    priorModel: 'Pengi / Qwen-A',
    metric: 'ACC ↑',
    prior: '62.0 | 78.8',
    result: '66.7 | 81.7',
    afNextVariant: 'AF-Next-Instruct',
  },
  {
    area: 'Music transfer',
    benchmark: 'Medley-Solos-DB',
    priorModel: 'Audio Flamingo 2',
    metric: 'ACC ↑',
    prior: '85.80',
    result: '92.13',
    afNextVariant: 'AF-Next-Instruct',
  },
  {
    area: 'Captioning',
    benchmark: 'SongCaps',
    priorModel: 'Audio Flamingo 3',
    metric: 'GPT5 ↑',
    prior: '6.7 | 6.2',
    result: '8.8 | 8.9',
    afNextVariant: 'AF-Next-Instruct',
  },
]

const trainingStages = [
  {
    title: 'Pre-training',
    detail:
      'Align the audio adaptor first, then fine-tune the encoder while keeping the language model frozen. This stage focuses on classification, captioning, and ASR.',
  },
  {
    title: 'Mid-training',
    detail:
      'Move into full fine-tuning, introduce AudioSkills-style reasoning data, and then expand heavily into long-audio captioning and long-audio QA up to 30 minutes.',
  },
  {
    title: 'Post-training',
    detail:
      'Use GRPO-based reinforcement learning to improve instruction following, safety behavior, multi-turn chat, and higher-value audio understanding skills.',
  },
  {
    title: 'CoT training',
    detail:
      'Train AF-Next-Think with AF-Think-Time so the model reasons with timestamp-grounded evidence rather than generic chain-of-thought text.',
  },
]

const variants = [
  {
    title: 'AF-Next-Instruct',
    description:
      'General-purpose checkpoint for question answering, instruction following, multi-turn interaction, and broad long-audio understanding.',
  },
  {
    title: 'AF-Next-Think',
    description:
      'Reasoning-focused checkpoint tuned for timestamp-grounded analysis of harder, longer, and more evidence-heavy audio.',
  },
  {
    title: 'AF-Next-Captioner',
    description:
      'Captioning-oriented checkpoint for dense descriptions across speech, sound, and music, including long-form summaries.',
  },
]

const demoExamples = [
  {
    title: 'Long Captions',
    category: 'Dense Captioning',
    prompt: 'Write a detailed caption of the input audio capturing sound, music and speech details',
  },
  {
    title: 'Timestamped Captions',
    category: 'Event Timeline',
    prompt: 'Describe all events in the audio with start and end times.',
  },
  {
    title: 'Timestamped ASR',
    category: 'Speech Transcription',
    prompt:
      'Transcribe the spoken context to written English text. Provide segment-level timestamps for the transcription',
  },
]

const limitations = [
  'Internet-scale audio is still noisy and unevenly distributed across languages, rare events, and specialized domains.',
  'Long-context reasoning improves substantially, but sparse evidence spread across distant segments remains difficult.',
  'Several supported capabilities, including multi-talker ASR and timestamped captioning, still need broader standardized evaluation.',
]
</script>

<template>
  <div id="app" class="min-h-screen bg-gradient-to-br from-slate-50 via-cyan-50 to-emerald-50 pb-16">
    <div class="max-w-6xl mx-auto px-4 pt-12">
      <header class="text-center space-y-6">
        <img src="/logo.webp" alt="Audio Flamingo Next logo" width="120" height="120"
          class="hero-logo mx-auto">
        <h1
          class="inline-block pb-1 md:pb-2 text-4xl md:text-6xl font-extrabold tracking-tight bg-clip-text text-transparent bg-gradient-to-r from-cyan-600 via-teal-600 to-emerald-600 leading-[1.08]">
          Audio Flamingo Next
        </h1>
        <p class="text-xl md:text-2xl text-gray-700 max-w-4xl mx-auto leading-relaxed">
          Next-generation open audio-language models for speech, sound, and music
        </p>
        <div class="text-sm md:text-base text-gray-600 max-w-5xl mx-auto leading-relaxed">
          <span class="font-semibold text-gray-800">Authors:</span>&nbsp;
          <template v-for="(author, index) in authors" :key="author.name">
            <span class="inline-block whitespace-nowrap">
              {{ author.name }}<sup>{{ author.affiliations }}</sup><span v-if="index < authors.length - 1">,&nbsp;</span>
            </span>
          </template>
        </div>
        <p class="text-sm text-gray-500">
          <span class="font-semibold text-gray-700">Affiliations:</span>
          <sup>1</sup> NVIDIA, CA, USA |
          <sup>2</sup> University of Maryland, College Park, USA
        </p>
        <p class="text-base md:text-lg text-gray-600 max-w-4xl mx-auto leading-relaxed">
          AF-Next is the most advanced LALM in the Audio Flamingo series yet, with stronger general audio
          understanding, longer context, richer real-world training data, and timestamp-grounded reasoning for complex
          long-form recordings.
        </p>
        <div class="flex flex-wrap items-center justify-center gap-3 pt-2">
          <a v-for="link in topLinks" :key="link.href" :href="link.href"
            :target="link.external ? '_blank' : undefined"
            :rel="link.external ? 'noopener' : undefined"
            :class="link.primary ? 'btn-primary' : 'btn-secondary'">
            {{ link.label }}
          </a>
        </div>
      </header>
    </div>

    <div class="section-card">
      <h2 class="text-3xl md:text-4xl font-bold mb-6 tracking-tight">Why It Matters</h2>
      <p class="text-base md:text-lg text-gray-700 leading-relaxed mb-8">
        AF-Next pushes open audio-language modeling beyond narrow benchmark-centric training by scaling data curation
        across long-form speech, environmental audio, and music, then pairing that with a curriculum that extends
        context length and strengthens reasoning. Compared with earlier Audio Flamingo models, AF-Next is built to
        handle longer, noisier, and more realistic recordings while remaining competitive across a broad set of
        established evaluations.
      </p>

      <h2 class="text-3xl md:text-4xl font-bold mb-6 tracking-tight">Key Features</h2>
      <ul class="list-disc list-inside space-y-3 text-base text-gray-700 leading-relaxed">
        <li v-for="feature in keyFeatures" :key="feature">{{ feature }}</li>
      </ul>
    </div>

    <div class="text-center mt-8 px-4">
      <h2 class="text-3xl md:text-4xl font-bold tracking-tight">Broad Audio Understanding</h2>
      <p class="mt-2 text-gray-600 max-w-3xl mx-auto text-base leading-relaxed">
        AF-Next is a generalist system rather than a domain-specific one, and some of its strongest gains show up on
        long-audio reasoning while still transferring well to music-heavy tasks.
      </p>
    </div>

    <div id="benchmarks" class="section-card">
      <div class="flex flex-col lg:flex-row lg:items-start gap-8">
        <div class="lg:w-[34%] space-y-4">
          <img src="/figures/radar.webp" alt="AF-Next benchmark radar"
            class="w-full h-auto rounded-xl border border-gray-200 bg-white shadow-sm" loading="lazy">
          <p class="text-sm text-gray-600 leading-relaxed">
            Normalized benchmark view comparing AF-Next with prior state-of-the-art audio-language models.
          </p>
        </div>

        <div class="space-y-6 lg:flex-1">
          <h2 class="text-3xl md:text-4xl font-bold mb-2 tracking-tight">Performance Snapshot</h2>
          <p class="text-base text-gray-600 leading-relaxed">
            AF-Next improves general audio QA, long-audio understanding, and several music transfer benchmarks.
            The numbers below use the strongest AF-Next variant reported in the current paper for each task.
          </p>
          <div class="overflow-x-auto">
            <table class="performance-table min-w-full text-base text-left text-gray-700 border border-gray-200 rounded-lg">
              <thead class="bg-gray-50">
                <tr>
                  <th class="px-4 py-3 text-left text-xs font-semibold uppercase tracking-wide text-gray-600 border-b border-gray-200">
                    Area
                  </th>
                  <th class="px-4 py-3 text-left text-xs font-semibold uppercase tracking-wide text-gray-600 border-b border-gray-200">
                    Benchmark
                  </th>
                  <th class="w-[22%] min-w-[180px] px-4 py-3 text-left text-xs font-semibold uppercase tracking-wide text-gray-600 border-b border-gray-200">
                    Prior SOTA
                  </th>
                  <th class="px-4 py-3 text-center text-xs font-semibold uppercase tracking-wide text-gray-600 border-b border-gray-200 whitespace-nowrap">
                    Metric
                  </th>
                  <th class="px-4 py-3 text-center text-xs font-semibold uppercase tracking-wide text-gray-600 border-b border-gray-200">
                    Best AF-Next
                  </th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="row in benchmarks" :key="row.area + row.benchmark" class="border-b border-gray-200">
                  <td class="px-4 py-3 align-top font-medium text-gray-900">{{ row.area }}</td>
                  <td class="px-4 py-3 align-top">
                    <div class="font-medium text-gray-900">{{ row.benchmark }}</div>
                  </td>
                  <td class="w-[22%] min-w-[180px] px-4 py-3 align-top">
                    <div class="text-gray-700">{{ row.priorModel }}</div>
                    <div class="text-sm text-gray-500">{{ row.prior }}</div>
                  </td>
                  <td class="px-4 py-3 align-top text-center whitespace-nowrap">{{ row.metric }}</td>
                  <td class="px-4 py-3 align-top text-center">
                    <div class="font-semibold text-teal-700">{{ row.result }}</div>
                    <div class="text-xs text-gray-500 mt-1">{{ row.afNextVariant }}</div>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>

    <div id="training" class="section-card">
      <h2 class="text-3xl md:text-4xl font-bold mb-6 tracking-tight">Training Pipeline</h2>
      <div class="grid grid-cols-1 lg:grid-cols-[1.05fr,0.95fr] gap-8 items-start">
        <div class="space-y-4">
          <img src="/figures/pipeline.webp" alt="AF-Next training pipeline and architecture"
            class="w-full h-auto rounded-xl border border-gray-200 bg-white shadow-sm" loading="lazy">
        </div>
        <div class="space-y-4">
          <p class="text-base text-gray-700 leading-relaxed">
            AF-Next is trained with a staged curriculum: alignment and encoder tuning first, full fine-tuning next,
            then GRPO-based post-training, and finally time-grounded reasoning training with AF-Think-Time. It also
            uses sequence-parallel long-context training to make 128K-token audio-language training practical.
          </p>
          <div class="grid grid-cols-1 gap-4">
            <div v-for="stage in trainingStages" :key="stage.title" class="mini-card">
              <h3 class="text-lg font-semibold text-gray-900">{{ stage.title }}</h3>
              <p class="text-sm md:text-base text-gray-600 leading-relaxed mt-2">{{ stage.detail }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div id="examples" class="section-card">
      <h2 class="text-3xl md:text-4xl font-bold mb-6 tracking-tight">Capability and Data Examples</h2>
      <div class="space-y-10">
        <div class="space-y-4">
          <img src="/figures/examples.webp" alt="Examples of AF-Next data types"
            class="w-full h-auto rounded-xl border border-gray-200 bg-white shadow-sm" loading="lazy">
          <p class="text-base text-gray-700 leading-relaxed">
            AF-Next expands training data with long audio QA, timestamped diarization, safety supervision,
            timestamped captions, and multi-turn audio chat.
          </p>
        </div>
      </div>
    </div>

    <div id="demo-prompts" class="section-card">
      <h2 class="text-3xl md:text-4xl font-bold mb-4 tracking-tight">Demo Prompts</h2>
      <p class="text-base text-gray-700 leading-relaxed mb-8 max-w-4xl">
        These three prompt patterns showcase the captioning, time-grounded event description, and timestamped speech
        transcription workflows highlighted by AF-Next.
      </p>
      <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
        <div v-for="example in demoExamples" :key="example.title" class="demo-card h-full">
          <div class="inline-flex items-center rounded-full bg-teal-50 px-3 py-1 text-xs font-semibold uppercase tracking-[0.18em] text-teal-700">
            {{ example.category }}
          </div>
          <h3 class="mt-4 text-2xl font-semibold text-gray-900">{{ example.title }}</h3>
          <p class="mt-4 prompt-card text-sm md:text-base text-gray-700 leading-relaxed">
            {{ example.prompt }}
          </p>
        </div>
      </div>
    </div>

    <div class="section-card">
      <h2 class="text-3xl md:text-4xl font-bold mb-6 tracking-tight">Release Variants</h2>
      <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
        <div v-for="variant in variants" :key="variant.title" class="mini-card h-full">
          <h3 class="text-xl font-semibold text-gray-900">{{ variant.title }}</h3>
          <p class="text-base text-gray-600 leading-relaxed mt-3">{{ variant.description }}</p>
        </div>
      </div>
    </div>

    <div class="section-card">
      <h2 class="text-3xl md:text-4xl font-bold mb-6 tracking-tight">Limitations</h2>
      <ul class="list-disc list-inside space-y-3 text-base text-gray-700 leading-relaxed">
        <li v-for="item in limitations" :key="item">{{ item }}</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.hero-logo {
  border-radius: 50%;
  box-shadow: 0 12px 30px rgba(13, 148, 136, 0.18);
}

.section-card {
  max-width: 1280px;
  margin: 0 auto;
  padding: 48px;
  margin-top: 64px;
  margin-bottom: 48px;
  background: white;
  border-radius: 12px;
  box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.16), 0 0 0 1px rgba(209, 213, 219, 0.5);
}

.performance-table tbody tr:hover {
  background-color: rgba(13, 148, 136, 0.04);
}

.btn-primary {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 0.65rem 0.95rem;
  font-size: 0.875rem;
  font-weight: 500;
  border-radius: 0.375rem;
  background-color: #0f766e;
  color: white;
  transition: background-color 0.15s ease;
  border: none;
  cursor: pointer;
}

.btn-primary:hover {
  background-color: #115e59;
}

.btn-secondary {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 0.65rem 0.95rem;
  font-size: 0.875rem;
  font-weight: 500;
  border-radius: 0.375rem;
  background-color: white;
  color: #374151;
  border: 1px solid #d1d5db;
  transition: background-color 0.15s ease, border-color 0.15s ease;
  cursor: pointer;
}

.btn-secondary:hover {
  background-color: #f0fdfa;
  border-color: #99f6e4;
}

.mini-card {
  border-radius: 1rem;
  border: 1px solid #e5e7eb;
  box-shadow: 0 20px 35px -20px rgba(15, 23, 42, 0.2);
  background-color: rgba(249, 250, 251, 0.9);
  padding: 1.25rem;
}

.demo-card {
  border-radius: 1rem;
  border: 1px solid #ccfbf1;
  box-shadow: 0 24px 45px -28px rgba(13, 148, 136, 0.24);
  background: linear-gradient(180deg, rgba(240, 253, 250, 0.95) 0%, rgba(249, 250, 251, 0.92) 100%);
  padding: 1.5rem;
}

.prompt-card {
  border-radius: 0.875rem;
  border: 1px solid #d1fae5;
  background-color: rgba(255, 255, 255, 0.88);
  padding: 1rem;
}

.resource-card {
  display: block;
  border-radius: 1rem;
  border: 1px solid #e5e7eb;
  box-shadow: 0 20px 35px -20px rgba(15, 23, 42, 0.2);
  background-color: rgba(249, 250, 251, 0.9);
  padding: 1.25rem;
  transition: transform 0.15s ease, box-shadow 0.15s ease, border-color 0.15s ease;
}

.resource-card:hover {
  transform: translateY(-2px);
  border-color: #5eead4;
  box-shadow: 0 24px 40px -20px rgba(13, 148, 136, 0.25);
}

@media (max-width: 768px) {
  .section-card {
    padding: 28px 20px;
    margin-top: 32px;
    margin-bottom: 24px;
  }
}
</style>
