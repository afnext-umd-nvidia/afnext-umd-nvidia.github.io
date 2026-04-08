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
  { label: 'Outputs', href: '#model-outputs', primary: false, external: false },
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

const showcaseVideo = {
  title: 'Overlayed Time-Caption Showcase',
  src: '/videos/ilikeyou-captions-overlayed.mp4',
  description:
    'A short overlaid demo that shows time-grounded captions directly on top of the source video. This is the inline showcase direction requested during review, rather than a static screenshot of outputs.',
}

const longCaptionExamples = [
  {
    title: 'Fight-Night Interview',
    prompt: 'Construct a caption that captures every aspect of the audio — environmental sounds, musical score, and dialogue.',
    excerpt:
      'The audio opens with a low‑fidelity male voice speaking over a faint crowd murmur, followed by a brief female voice and a short musical sting. The speaker says, “Well, Dana, I don’t know that you could have written a better script for episode one, season two. Who’s going to the big show?”',
    more:
      'The caption keeps following the room tone, interview back-and-forth, and the discussion around Menifield and Hardy instead of collapsing the clip into a generic “sports interview” label. It preserves both the atmosphere and the running dialogue across a long segment.',
  },
  {
    title: 'Recipe Walkthrough',
    prompt: 'Generate a comprehensive caption that describes the background sounds, music, and spoken dialogue of the given audio.',
    excerpt:
      'The audio begins with a brief, high‑pitched electronic beep followed by a short, low‑frequency hum that fades quickly. A flat‑toned female voice then speaks over a faint, continuous hiss: “Hi, welcome to Sanjeev Kapoor’s Khazana. I’m Jyotsna and today I’m here to teach you a recipe called stuffed karela.”',
    more:
      'The write-up continues through the ingredient list, frying sounds, and kitchen narration, showing that the model can stay detailed over long instructional audio instead of switching to a short abstract summary.',
  },
]

const timeGroundedExamples = [
  {
    title: 'Fight Commentary Timeline',
    prompt: 'Describe all events in the audio with start and end times.',
    excerpt: `<t>0.0-0.589</t> Crowd cheering and applause.
<t>0.589-4.199</t> “Tonight’s tale of the tape, brought to you by Toyo Tires, all or nothing.”
<t>4.199-10.999</t> Sean O’Malley is introduced with age, height, and reach advantages.
<t>20.089-22.849</t> “All right, Sean ready? First round, fights on.”`,
    more:
      'This example is event-aligned rather than dumping timestamps at fixed gaps. Commentary, crowd reactions, and the fight-start cue all land on distinct spans that are easy to read inline.',
  },
]

const mmarComparisons = [
  {
    title: 'Environmental Sound Reasoning',
    kind: 'Soup Or Not',
    audioSrc: '/audio/mmar-soup-or-not.wav',
    question: 'Is someone making soup in the video?',
    groundTruth: '(A) No',
    instructPrediction: '(B) Yes',
    thinkExcerpt:
      'The first pass loosely resembles stirring and chopping for soup, but the revision matters: there is no bubbling, simmering, or other liquid-based cooking cue. Once the model re-anchors on the actual audio evidence, “No” becomes the better answer.',
    thinkDetails:
      'The audio begins with a sharp metallic scrape and rapid chopping sounds. Those cues could initially suggest kitchen activity, but the lack of bubbling, boiling, or blending makes ordinary prep noise a better fit than soup-making.',
    thinkFinal: '(A) No',
    captionerExcerpt:
      'A sharp hiss erupts as a spray can is activated, followed by a second burst and then a loud continuous sizzle on a hot surface.',
    captionerDetails:
      'The captioner output reframes the clip away from cooking broth entirely: the dominant events are spray activation and high-frequency sizzling, which rules out soup preparation.',
    captionerFinal: '(A) No',
  },
  {
    title: 'Temporal Cue Recovery',
    kind: 'Flashback Timing',
    audioSrc: '/audio/mmar-flashback-timing.wav',
    question: 'At what second does the flashback start in the video?',
    groundTruth: '(D) 00:15:00',
    instructPrediction: '(C) 00:25:00',
    thinkExcerpt:
      'The clip has three speech segments, and the second one spans roughly 10.7s to 16.9s. That places the flashback cue closest to 15 seconds rather than 10, 20, or 25.',
    thinkDetails:
      'The model explicitly reasons over segment timing instead of guessing from the answer choices. By localizing the middle spoken event and using its midpoint, it recovers the right temporal answer.',
    thinkFinal: '(D) 00:15:00',
    captionerExcerpt:
      'A strained father-daughter exchange escalates until the father says, “Hey, it’s been 15 minutes. Turn it off,” which anchors the clip near the correct answer.',
    captionerDetails:
      'The captioner does not solve the multiple-choice task directly, but it surfaces the phrase containing the relevant temporal cue. That extra descriptive grounding still points to the right option.',
    captionerFinal: '(D) 00:15:00',
  },
  {
    title: 'Speech Intention',
    kind: 'Mispronunciation',
    audioSrc: '/audio/mmar-mispronunciation.wav',
    question: 'Did this person mispronounce the word accidentally or intentionally?',
    groundTruth: '(C) Intentionally',
    instructPrediction: '(A) Accidentally',
    thinkExcerpt:
      'The speaker says “jalapenos,” gets corrected to “jalapeño peppers,” and then repeats “jalapenos.” Repeating the same form after correction suggests deliberate joking rather than an unaware slip.',
    thinkDetails:
      'The correction-response pattern is the key evidence here. Once the speaker has been corrected and still repeats the same pronunciation, the mistake is no longer best explained as accidental.',
    thinkFinal: '(C) Intentionally',
    captionerExcerpt:
      'A male voice asks about jalapenos and then requests a croissant over faint background chatter.',
    captionerDetails:
      'The captioner provides a compact surface description of the exchange, which is enough context for the final answer once paired with the question.',
    captionerFinal: '(C) Intentionally',
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

    <div id="model-outputs" class="section-card">
      <h2 class="text-3xl md:text-4xl font-bold mb-4 tracking-tight">Model Outputs</h2>
      <p class="text-base text-gray-700 leading-relaxed max-w-4xl">
        The website review asked for inline outputs rather than screenshots. The examples below are trimmed from real
        model generations and include both strong qualitative wins and a few explicit comparison cases where
        AF-Next-Instruct fails but AF-Next-Think and AF-Next-Captioner recover the correct answer.
      </p>

      <div class="output-section">
        <div class="output-section__heading">
          <h3 class="text-2xl md:text-3xl font-semibold tracking-tight">Long Captions</h3>
          <p class="text-base text-gray-600 leading-relaxed">
            Dense, readable captions for long-form audio without collapsing everything into a one-line summary.
          </p>
        </div>
        <div class="output-grid output-grid--two">
          <article v-for="example in longCaptionExamples" :key="example.title" class="output-card">
            <div class="output-card__meta">
              <span class="output-chip">{{ example.title }}</span>
            </div>
            <p class="output-prompt">{{ example.prompt }}</p>
            <p class="output-excerpt">{{ example.excerpt }}</p>
            <details class="trace-details">
              <summary>Read why this example was selected</summary>
              <p class="output-excerpt">{{ example.more }}</p>
            </details>
          </article>
        </div>
      </div>

      <div class="output-section">
        <div class="output-section__heading">
          <h3 class="text-2xl md:text-3xl font-semibold tracking-tight">Time-Grounded Outputs</h3>
          <p class="text-base text-gray-600 leading-relaxed">
            A small showcase of timestamped outputs, including the overlaid video requested for the public-facing page.
          </p>
        </div>
        <div class="output-grid output-grid--showcase">
          <article class="output-card output-card--video">
            <div class="output-card__meta">
              <span class="output-chip">{{ showcaseVideo.title }}</span>
              <span class="output-muted">Inline video showcase</span>
            </div>
            <video class="showcase-video" :src="showcaseVideo.src" controls preload="metadata" playsinline />
            <p class="text-base text-gray-700 leading-relaxed mt-4">{{ showcaseVideo.description }}</p>
          </article>

          <article v-for="example in timeGroundedExamples" :key="example.title" class="output-card">
            <div class="output-card__meta">
              <span class="output-chip">{{ example.title }}</span>
            </div>
            <p class="output-prompt">{{ example.prompt }}</p>
            <p class="output-excerpt">{{ example.excerpt }}</p>
            <details class="trace-details">
              <summary>Why this timeline made the cut</summary>
              <p class="output-excerpt">{{ example.more }}</p>
            </details>
          </article>
        </div>
      </div>

      <div class="output-section">
        <div class="output-section__heading">
          <h3 class="text-2xl md:text-3xl font-semibold tracking-tight">MMAR Comparison</h3>
          <p class="text-base text-gray-600 leading-relaxed">
            Three compact cases where the same clip is shown through the lens of AF-Next-Instruct, AF-Next-Think, and
            AF-Next-Captioner. These are intentionally comparison-heavy rather than purely cherry-picked wins.
          </p>
        </div>

        <div class="comparison-stack">
          <article v-for="sample in mmarComparisons" :key="sample.title" class="comparison-card">
            <div class="comparison-card__header">
              <div>
                <div class="output-card__meta">
                  <span class="output-chip">{{ sample.kind }}</span>
                </div>
                <h4 class="text-xl md:text-2xl font-semibold text-gray-900 mt-3">{{ sample.title }}</h4>
              </div>
              <div class="ground-truth">
                <span class="ground-truth__label">Ground Truth</span>
                <span class="ground-truth__value">{{ sample.groundTruth }}</span>
              </div>
            </div>

            <p class="comparison-question">{{ sample.question }}</p>
            <div class="audio-strip">
              <span class="audio-strip__label">Listen to the clip</span>
              <audio class="audio-player" :src="sample.audioSrc" controls preload="none" />
            </div>

            <div class="comparison-grid">
              <section class="model-panel model-panel--wrong">
                <span class="model-badge model-badge--wrong">AF-Next-Instruct</span>
                <p class="model-panel__label">Prediction</p>
                <p class="model-panel__answer">{{ sample.instructPrediction }}</p>
              </section>

              <section class="model-panel">
                <span class="model-badge model-badge--good">AF-Next-Think</span>
                <p class="output-excerpt">{{ sample.thinkExcerpt }}</p>
                <details class="trace-details">
                  <summary>Read longer reasoning excerpt</summary>
                  <p class="output-excerpt">{{ sample.thinkDetails }}</p>
                </details>
                <p class="model-panel__final">Final answer: {{ sample.thinkFinal }}</p>
              </section>

              <section class="model-panel">
                <span class="model-badge model-badge--good">AF-Next-Captioner</span>
                <p class="output-excerpt">{{ sample.captionerExcerpt }}</p>
                <details class="trace-details">
                  <summary>Read longer caption excerpt</summary>
                  <p class="output-excerpt">{{ sample.captionerDetails }}</p>
                </details>
                <p class="model-panel__final">Final answer: {{ sample.captionerFinal }}</p>
              </section>
            </div>
          </article>
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

.output-section + .output-section {
  margin-top: 3.5rem;
}

.output-section__heading {
  margin-bottom: 1.5rem;
}

.output-grid {
  display: grid;
  gap: 1.5rem;
}

.output-grid--two {
  grid-template-columns: repeat(2, minmax(0, 1fr));
}

.output-grid--showcase {
  grid-template-columns: minmax(0, 1.2fr) minmax(0, 0.8fr);
  align-items: start;
}

.output-card {
  border-radius: 1rem;
  border: 1px solid #d1d5db;
  box-shadow: 0 24px 45px -28px rgba(15, 23, 42, 0.24);
  background: linear-gradient(180deg, rgba(255, 255, 255, 0.98) 0%, rgba(248, 250, 252, 0.96) 100%);
  padding: 1.5rem;
}

.output-card--video {
  padding-bottom: 1.25rem;
}

.output-card__meta {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap: 0.65rem;
}

.output-chip {
  display: inline-flex;
  align-items: center;
  border-radius: 9999px;
  background: rgba(15, 118, 110, 0.1);
  color: #0f766e;
  font-size: 0.75rem;
  font-weight: 700;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  padding: 0.45rem 0.75rem;
}

.output-muted {
  color: #6b7280;
  font-size: 0.9rem;
}

.output-prompt {
  margin-top: 1rem;
  border-radius: 0.875rem;
  border: 1px solid #d1fae5;
  background: rgba(240, 253, 250, 0.75);
  color: #115e59;
  padding: 0.9rem 1rem;
  font-size: 0.98rem;
  line-height: 1.6;
}

.output-excerpt {
  margin-top: 1rem;
  color: #374151;
  line-height: 1.75;
  white-space: pre-line;
}

.showcase-video {
  width: 100%;
  margin-top: 1rem;
  border-radius: 0.9rem;
  border: 1px solid #d1d5db;
  background: #0f172a;
  box-shadow: 0 22px 40px -30px rgba(15, 23, 42, 0.45);
}

.trace-details {
  margin-top: 1rem;
  border-top: 1px solid #e5e7eb;
  padding-top: 0.9rem;
}

.trace-details summary {
  cursor: pointer;
  color: #0f766e;
  font-weight: 600;
  list-style: none;
}

.trace-details summary::-webkit-details-marker {
  display: none;
}

.comparison-stack {
  display: grid;
  gap: 1.5rem;
}

.comparison-card {
  border-radius: 1rem;
  border: 1px solid #d1d5db;
  background: linear-gradient(180deg, rgba(255, 255, 255, 0.98) 0%, rgba(248, 250, 252, 0.96) 100%);
  box-shadow: 0 24px 45px -28px rgba(15, 23, 42, 0.24);
  padding: 1.5rem;
}

.comparison-card__header {
  display: flex;
  justify-content: space-between;
  gap: 1rem;
  align-items: flex-start;
}

.ground-truth {
  min-width: 140px;
  border-radius: 0.9rem;
  border: 1px solid #a7f3d0;
  background: rgba(236, 253, 245, 0.9);
  padding: 0.9rem 1rem;
}

.ground-truth__label {
  display: block;
  color: #047857;
  font-size: 0.78rem;
  font-weight: 700;
  letter-spacing: 0.08em;
  text-transform: uppercase;
}

.ground-truth__value {
  display: block;
  margin-top: 0.35rem;
  color: #065f46;
  font-size: 1rem;
  font-weight: 700;
}

.comparison-question {
  margin-top: 1rem;
  color: #111827;
  font-size: 1.02rem;
  line-height: 1.7;
}

.audio-strip {
  margin-top: 1rem;
}

.audio-strip__label {
  display: block;
  margin-bottom: 0.55rem;
  color: #4b5563;
  font-size: 0.8rem;
  font-weight: 700;
  letter-spacing: 0.08em;
  text-transform: uppercase;
}

.audio-player {
  width: 100%;
}

.comparison-grid {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 1rem;
  margin-top: 1.35rem;
}

.model-panel {
  border-radius: 0.95rem;
  border: 1px solid #e5e7eb;
  background: rgba(249, 250, 251, 0.92);
  padding: 1rem;
}

.model-panel--wrong {
  border-color: #fecaca;
  background: rgba(254, 242, 242, 0.88);
}

.model-badge {
  display: inline-flex;
  align-items: center;
  border-radius: 9999px;
  font-size: 0.74rem;
  font-weight: 800;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  padding: 0.42rem 0.7rem;
}

.model-badge--good {
  background: rgba(20, 184, 166, 0.14);
  color: #0f766e;
}

.model-badge--wrong {
  background: rgba(239, 68, 68, 0.12);
  color: #b91c1c;
}

.model-panel__label {
  margin-top: 1rem;
  color: #6b7280;
  font-size: 0.82rem;
  text-transform: uppercase;
  letter-spacing: 0.08em;
}

.model-panel__answer {
  margin-top: 0.4rem;
  color: #991b1b;
  font-size: 1.15rem;
  font-weight: 700;
}

.model-panel__final {
  margin-top: 1rem;
  color: #0f766e;
  font-weight: 700;
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

  .output-grid--two,
  .output-grid--showcase,
  .comparison-grid {
    grid-template-columns: 1fr;
  }

  .comparison-card__header {
    flex-direction: column;
  }
}
</style>
