<script setup lang="ts">
const codeUrl = 'https://github.com/NVIDIA/audio-flamingo'
const instructUrl = 'https://huggingface.co/nvidia/audio-flamingo-next-hf'
const captionerUrl = 'https://huggingface.co/nvidia/audio-flamingo-next-captioner-hf'

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
  { label: 'Instruct', href: instructUrl, primary: false, external: true },
  { label: 'Captioner', href: captionerUrl, primary: false, external: true },
  { label: 'Benchmarks', href: '#benchmarks', primary: false, external: false },
  { label: 'Architecture', href: '#training', primary: false, external: false },
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
      'Default checkpoint for QA, chat, ASR-style tasks, and direct assistant-like answers over long audio.',
  },
  {
    title: 'AF-Next-Think',
    description:
      'Reasoning-focused checkpoint tuned for timestamp-grounded analysis of harder, longer, and more evidence-heavy audio.',
  },
  {
    title: 'AF-Next-Captioner',
    description:
      'Use this one when you want denser long-form captions, richer scene descriptions, and timestamp-aware summaries.',
  },
]

const showcaseVideos = [
  {
    anchor: 'output-love-song-video',
    title: 'Love Song Caption Overlay',
    src: '/videos/love-song-captions-overlay.mp4',
    description: 'Time-grounded caption overlay on a music clip.',
  },
  {
    anchor: 'output-ufc-interview-video',
    title: 'UFC Interview Caption Overlay',
    src: '/videos/ufc-interview-captions-overlay.mp4',
    description: 'Time-grounded caption overlay on the UFC interview clip.',
  },
]

const longCaptionExamples = [
  {
    anchor: 'output-fight-caption',
    title: 'Fight-Night Interview',
    variant: 'AF-Next-Captioner',
    sourceUrl: 'https://youtu.be/i4jtyeZQCgY',
    prompt:
      'Construct a caption that captures every aspect of the audio — environmental sounds, musical score, and dialogue.',
    output: `The audio opens with a low‑fidelity male voice speaking over a faint crowd murmur, followed by a brief female voice and a short musical sting. The speaker says, “Well, Dana, I don't know that you could have written a better script for episode one, season two. Who's going to the big show?” The crowd noise continues as the male voice continues, “Well, we, uh, last season we had Mennefield here and we sent him away and said, come back when you get some more experience. He did that and obviously he looks spectacular tonight. So he is in. We are, we are definitely going to take him. And then, this is strange, but, uh, you know, when you look at a guy like Hardy, Hardy has, has no experience. Tonight was his first professional fight and, um, but man, he can punch and obviously, you know, there's a lot more to that guy. Um, so what we're going to do is we're, we're actually going to bring Hardy in and put him on a, um, you know, you know, this guy needs, needs to get some more experience and, uh, we'll figure out how to do that. We've done this before and, uh, and that's it. We'll take Mennefield and Hardy. Congratulations.” The background remains a low‑level crowd ambience.
Around the 30‑second mark the male voice returns, now over a slightly louder crowd and a brief musical sting, saying, “If Hardy is able to go away and get that experience, how quickly can you imagine yourself bringing him into the UFC? I don't know if he goes away. We've talked about some ideas to help this guy grow and, uh, he's already with a great team. He's, he's, he's, uh, he's with a great team right now. He's with Dan Lambert and his crew down in Florida. Um, you know, he's in the right place as far as, uh, training and learning. And, uh, now he just needs some experience and, uh, and we can do that too. Well, Alonzo Mennefield certainly proved himself to be a light heavyweight to contend with. What kind of impact do you think he can make on the division?” The crowd noise persists, with occasional faint cheers.
From 1:00 to 1:30 the male speaker continues, still over the crowd, “No, I mean, if he fights like he fought tonight, uh, who knows what can happen to the kid. He looked great, explosive, fast, athletic, strong, um, yeah. We've got a lot of week two fighters, I'm sure watching right now. What's your message to the guys watching right now that are fighting next week? Well, I can tell you this. If you don't have some killer instinct, get it, because you have one shot. You come in here, you fight in front of us and, uh, we watch and, and, and we, we decide right there. If you come in and you look like Mennefield looked, you're probably going to get signed. If you come in and look like some of the other guys look, you're probably not going to get signed. So come in here and give it everything you got and, uh, win the fight impressively. Dana, thank you so much. Alonzo, why did you step on in here? Congratulations, man. You look great, kid. Thank you, thank you. How good does it feel to have Dana White tell you you look good? Feel is awesome, you know, because I remember UFC when it was like little and he blew it up. And for him to tell me that, I, I, I feel honored. Last year, obviously you had the stoppage, you won the way you didn't want to win, you went away. How, how, what has this journey been like and what is this moment like now, given the fact of what happened last year? It's been a learning experience, you know, fighting in a, uh, organization smaller, um, and, you know, having this opportunity and now it feels great. I feel like on another level, like just super happy. Light heavyweight division is one that is wide open at the moment. What kind of impact do you think you can make when you make your way over there? I plan to be a top dog, you know, I say this in my interviews, um, and I plan to be a top dog at 205, uh, and, you know, make my name known in that division. So here we come. Alonzo, what's your message to that light heavyweight division? Uh, you know, I'm giving out hugs, smiles, kisses and then, when we get in there, let's, let's battle. You know, I'm coming to fight y'all. There it is. Thank you so much. Congratulations. And we have Greg Hardy making his way back now. Congratulations, Greg. It sounds like, uh, from what Dana said, you will be put on some sort of developmental contract, not quite in the UFC yet, but just give us your thoughts on that news. It's awesome news. Uh, you know, I get the opportunity to show what I got, go back and, uh, work hard and come back even better than before, you know, it's, it's, it's exciting. It's everything I could ask for. How quickly would you like to get back to the big show? I just want to get back in the ring. You know, whatever show it is, uh, I just want another opportunity to fight. Man, I'm having a time of my life and I'm really enjoying it. Coming from another professional sport, what has this transition been like and what is this moment like compared to some of the big moments that you've had on the gridiron? This is definitely the top feeling I've probably ever had. You know, there's nothing like being in the octagon, facing another man, you know, worthy opponents, but, um, just transitioning has been very hard. Uh, it's been some of the hardest work I've ever done and it's made it as ten times sweeter, you know. You've had a lot of chatter coming into this fight. How good does it feel to shut up some of those naysayers? It me, it feels good. You know, I got a message and it's just keep watching. I'm gonna keep getting better and I'm gonna keep having fun and, you know, thank y'all for having me. Well, Greg, congratulations. Can't wait to see you in the UFC. Thank you so much.” The background remains a constant low‑level crowd murmur with occasional cheers and a faint musical sting at the end of the segment.
Overall, the audio consists of a male interviewer speaking over a persistent low‑level crowd ambience, interspersed with brief musical stings and occasional female interjections, discussing the selection of fighters for the UFC, the impact of the fighters’ performances, and the transition of a former football player into MMA.`,
  },
  {
    anchor: 'output-recipe-caption',
    title: 'Recipe Walkthrough',
    variant: 'AF-Next-Captioner',
    sourceUrl: 'https://youtu.be/1trMZ_5xQlU',
    prompt:
      'Generate a comprehensive caption that describes the background sounds, music, and spoken dialogue of the given audio.',
    output: `The audio begins with a brief, high‑pitched electronic beep followed by a short, low‑frequency hum that fades quickly. A flat‑toned female voice then speaks over a faint, continuous hiss, saying, “Hi, welcome to Sanjeev Kapoor’s Khazana. I’m Jyotsna and today I’m here to teach you a recipe called stuffed karela. I know karela sounds a bit, you know, everybody doesn’t enjoy karela. But this is the recipe by which I started eating karela. It’s my friend’s mother’s recipe and I’m sure once you make it, you’ll really be able to eat karela. For those who enjoy karela, of course, it’s a wonderful recipe. So here’s stuffed bitter gourd, that’s karela for you. For this recipe, the ingredients are as follows: bitter gourd or karela, this can be either the small ones or a medium size ones, don’t take the large ones, finely chopped onions, yogurt, that’s the he, turmeric powder, coriander powder, amchur powder, that’s dry mango powder, red chili powder, cumin powder, garam masala powder, salt and oil. To make this recipe, we have to prepare the bitter gourd a bit. What we have to do is take these bitter gourd and scrape it. Mind you, don’t throw the scrapings because we are going to use this for the stuffing. Once the karela has been scraped, we’ll give it a slit in the middle and open it up, take out the seeds that is inside. Once we take these seeds out, we get a nice gap in the middle in which we’ll be later on doing the stuffing. You can keep these seeds also if they are not too hard, that will go into the stuffing. Now we have a nice gap in the middle. Now what we are going to do to this karela which has been scraped and cut, we’ll add some salt and keep it aside for about half an hour. You can nicely add some salt because later on once the water is out, we are going to drain the salt water. You can even add some sea salt that is mota namak that’ll give a nice flavor to it. So we keep these aside. Now we’ve soaked this karela for half an hour. It’s let out some water so we’ll be draining out this water, squeezing it out. Some people who have diabetes can drink this juice in fact, but we don’t need it for the recipe. We’ll take it out on another plate. Now we have this karela which has been squeezed out. We heat some oil for deep frying. And we’ll add the karelas to this. We fry these karelas till they are golden brown. It doesn’t take much time to fry these. These stuffed karelas can be used while travelling because they stay for a long time. Now the karelas have got a nice golden brown color and they are ready to be taken out. Let’s drain them and take them out on a kitchen napkin so that the excess oil gets drained. Now that our karelas are ready to be stuffed, we need to make our stuffing, of course. Let’s heat a pan, take some oil. Once the oil is hot, we’ll add the chopped onions. The onions need to be sautéed till they are golden brown. Now the onions are half done, so we’ll add the karela scrapings. Not too much, in proportion with the onions. In case you have too many karelas and the scrapings are a lot, you don’t need to add all of it, probably equal to the onions. Now we’ll mix both these and sauté them. Let’s cover this and cook it for just two minutes so that the rawness of the bitter gourd scrapings go away. Now our onion and bitter gourd mixture is almost done. Now we’ll add all the masalas, a little bit of salt just enough for the stuffing, turmeric powder, coriander powder, cumin powder, red chili powder, garam masala powder, and amchur powder to make it tangy. Give it a nice mix. Now we’ll add the yogurt. And we’ll mix this stuffing. The yogurt will give out a little bit of water so we’ll sauté it until it becomes dry. Our stuffing is almost done. We’re just going to dry off the excess water. Here you can adjust the seasoning that is salt. You can taste the stuffing and see if the salt is enough. The yogurt gives a very nice flavor and it brings out all the flavors of the other ingredients. Our stuffing is now ready. Let’s switch off the burner. Cool it a little bit. We have our karelas here which have been fried. And now we’ll be adding the stuffing into these gaps of the karela and then we’ll be tying it with a thread. It’s not funny, it is true and it’s done with karelas all the time. Now let’s stuff our karelas. Let’s take a bit of the stuffing and let’s fill it inside. If the stuffing is extra, doesn’t matter, we’ll use it in our vegetable later on. Don’t overstuff the karelas, otherwise it will all come out. So I’m stuffing them first and then I’ll tie them up. Now our karelas have been stuffed. The excess masala can remain in this pan. We’ll use this for a final dish. We’ll take a thread, we’ll tie up the karelas so that the stuffing doesn’t come out. But remember, before serving, either you tell the people to open it up or you open it and then serve it. Just tie the thread across the cut that you’ve done for the stuffing. Our stuffed karelas are now ready with all the thread. They just need a little bit of finishing touch. We’ll use this pan which has the excess masala. You can add a little bit of oil, but I think it doesn’t require. So we’ll heat this masala. We’ll add these stuffed and tied karelas and let it cook in its own juices for about two to three minutes on slow flame. We can also cover it and just wait for two minutes before your karelas are totally ready. Just toss the karelas a bit. Our yummy stuffed karelas are now ready. Let’s take them out. And serve them immediately. I’m going to serve it with the thread and inform the people who are going to eat it that each one can take the thread out on their own. It’s good fun and exciting too, but I think the dish is even more exciting. Ready karelas, stuffed and very, very tasty. Enjoy.”`,
  },
]

const timeGroundedExamples = [
  {
    anchor: 'output-love-song-caption',
    title: 'Love Song Time-Grounded Caption',
    variant: 'AF-Next-Captioner',
    sourceUrl: '/videos/love-song-captions-overlay.mp4',
    sourceLabel: 'Open video',
    showResponse: false,
    prompt: 'Describe the audio over time, including instrumentation, vocal delivery, and timestamped lyrics.',
    responseLabel: 'AF-Next-Captioner response',
    output: `The audio begins with an instrumental intro featuring a gentle, fingerpicked acoustic guitar playing a melancholic, arpeggiated melody. The tempo is slow, creating a reflective and intimate mood. The texture is sparse, focusing on the clean, warm tone of the guitar. There are no lyrics or other instruments present.

A male vocalist begins singing in a soft, slightly breathy, and intimate tone. The acoustic guitar continues its arpeggiated pattern, providing a consistent, gentle harmonic foundation. The dynamics remain soft, emphasizing the personal and heartfelt delivery of the lyrics. The production is clean, highlighting the natural sound of the guitar and vocals. Lyrics: [11.0 - 14.0] "There's no need to complicate it," [14.0 - 17.0] "dress it up or overstate it." [17.0 - 22.0] "Without too much hesitation, here's the way I feel." [22.0 - 24.0] "I like you."

The male vocalist continues with the same intimate and heartfelt delivery. The acoustic guitar maintains its gentle arpeggiated pattern, providing a consistent, soft dynamic. Lyrics: [24.0 - 26.0] "I like you," [26.0 - 30.0] "even when I don't try to."`,
  },
  {
    anchor: 'output-reporter-asr',
    title: 'Reporter Interview ASR',
    variant: 'AF-Next-Instruct',
    sourceUrl: 'https://youtu.be/i4jtyeZQCgY',
    sourceLabel: 'Watch on YouTube',
    showResponse: true,
    prompt: 'Transcribe the spoken content to written English text. Provide segment-level timestamps for the transcription.',
    responseLabel: 'AF-Next-Instruct transcription',
    output: `<t>0.0-0.066</t> Speaker 1 (Female, Interviewer): Well, Dana, I don't know that you could have written a better script for episode one, season two. Who's going to the big show?
<t>0.066-0.134</t> Speaker 2 (Male, Interviewee): Well, we we uh last season we had Menfield here and we sent him away and said come back when you get some more experience.
<t>0.134-0.174</t> Speaker 2 (Male, Interviewee): He did that and obviously he looks spectacular tonight.
<t>0.174-0.204</t> Speaker 2 (Male, Interviewee): So he is in. We are we are definitely going to take him.
<t>0.204-0.214</t> Speaker 2 (Male, Interviewee): Um.
<t>0.214-0.284</t> Speaker 2 (Male, Interviewee): And then this is strange, but uh, you know, when you look at a guy like Hardy, Hardy has has no experience.
<t>0.284-0.314</t> Speaker 2 (Male, Interviewee): Um, tonight was his first professional fight.
<t>0.314-0.374</t> Speaker 2 (Male, Interviewee): And um, but man, he can punch and obviously, you know, there's a lot more to that guy.
<t>0.374-0.484</t> Speaker 2 (Male, Interviewee): Um, so what we're going to do is we're we're actually going to bring Hardy in and put him on a um, you know, you know, this this guy needs needs to get some more experience.
<t>0.484-0.514</t> Speaker 2 (Male, Interviewee): And uh, we'll figure out how to do that.
<t>0.514-0.534</t> Speaker 2 (Male, Interviewee): We've done this before and uh.
<t>0.534-0.564</t> Speaker 2 (Male, Interviewee): And that's it. We'll take Menfield and Hardy.
<t>0.564-0.574</t> Speaker 1 (Female, Interviewer): Congratulations.
<t>0.574-1.034</t> Speaker 1 (Female, Interviewer): If Hardy is able to go away and get that experience, how quickly can you imagine yourself bringing him into the UFC?
<t>1.034-1.044</t> Speaker 2 (Male, Interviewee): I don't know if he goes away.
<t>1.044-1.094</t> Speaker 2 (Male, Interviewee): We we've talked about some ideas to help this guy grow and uh he's already with a great team.
<t>1.094-1.144</t> Speaker 2 (Male, Interviewee): He he's he's he's uh he's with a great team right now, he's with Dan Lambert and his crew down in Florida.
<t>1.144-1.224</t> Speaker 2 (Male, Interviewee): Um, you know, he's in the right place as far as uh training and learning and uh now he just needs some experience and uh and we can do that too.
<t>1.224-1.274</t> Speaker 1 (Female, Interviewer): Well, Alonzo Menfield certainly proved himself to be a light heavyweight to contend with.
<t>1.274-1.294</t> Speaker 1 (Female, Interviewer): What kind of impact do you think he can make on that division?
<t>1.294-1.334</t> Speaker 2 (Male, Interviewee): No, I mean if he fights like he fought tonight, who knows what can happen to the kid?
<t>1.334-1.364</t> Speaker 2 (Male, Interviewee): He look great, explosive, fast, athletic, strong.
<t>1.364-1.384</t> Speaker 2 (Male, Interviewee): Um, yeah.
<t>1.384-1.424</t> Speaker 1 (Female, Interviewer): We've got a lot of week two fighters, I'm sure watching right now.
<t>1.424-1.454</t> Speaker 1 (Female, Interviewer): What's what's your message to the guys watching right now that are fighting next week?
<t>1.454-1.464</t> Speaker 2 (Male, Interviewee): Well, I can tell you this.
<t>1.464-1.484</t> Speaker 2 (Male, Interviewee): If you don't have some killer instinct, get it.
<t>1.484-1.564</t> Speaker 2 (Male, Interviewee): Because you have one shot. You come in here, you fight in front of us and uh we watch and and and we we we decide right there.
<t>1.564-2.024</t> Speaker 2 (Male, Interviewee): If you come in and you look like Menfield looked, you're probably going to get signed. If you come in and look like some of the other guys look, you're probably not going to get signed.
<t>2.024-2.074</t> Speaker 2 (Male, Interviewee): So, come in here and give it everything you got and uh win the fight impressively.
<t>2.074-2.084</t> Speaker 1 (Female, Interviewer): Dana, thank you so much.
<t>2.084-2.104</t> Speaker 1 (Female, Interviewer): Alonzo, why did you step on in here?
<t>2.104-2.124</t> Speaker 2 (Male, Interviewee): Oh, look at you.
<t>2.124-2.134</t> Speaker 2 (Male, Interviewee): Congratulations, man.
<t>2.134-2.144</t> Speaker 2 (Male, Interviewee): You look great, kid.
<t>2.144-2.154</t> Speaker 1 (Female, Interviewer): Thank you, thank you.
<t>2.154-2.164</t> Speaker 2 (Male, Interviewee): All right, movement.
<t>2.164-2.194</t> Speaker 1 (Female, Interviewer): How good does it feel to have Dana White tell you you look good?
<t>2.194-2.214</t> Speaker 2 (Male, Interviewee): Feel is awesome, you know, could have been a UFC when it was like little and he blew it up.
<t>2.214-2.244</t> Speaker 2 (Male, Interviewee): And for him to tell me that I I I feel honored.`,
  },
]

const timeGroundedPairs = [
  { video: showcaseVideos[0]!, example: timeGroundedExamples[0]! },
  { video: showcaseVideos[1]!, example: timeGroundedExamples[1]! },
]

const mmarComparisons = [
  {
    anchor: 'output-mmar-soup',
    title: 'Environmental Sound Reasoning',
    kind: 'Soup Or Not',
    audioSrc: '/audio/mmar-soup-or-not.wav',
    question: 'Is someone making soup in the video?',
    groundTruth: '(A) No',
    instructPrediction: '(B) Yes',
    thinkTrace:
      'The first pass loosely resembles stirring and chopping for soup, but the revision matters: there is no bubbling, simmering, or other liquid-based cooking cue. Once the model re-anchors on the actual audio evidence, “No” becomes the better answer. The audio begins with a sharp metallic scrape and rapid chopping sounds. Those cues could initially suggest kitchen activity, but the lack of bubbling, boiling, or blending makes ordinary prep noise a better fit than soup-making.',
    thinkFinal: '(A) No',
    captionerTrace:
      'A sharp hiss erupts as a spray can is activated, followed by a second burst and then a loud continuous sizzle on a hot surface. The captioner output reframes the clip away from cooking broth entirely: the dominant events are spray activation and high-frequency sizzling, which rules out soup preparation.',
    captionerFinal: '(A) No',
  },
  {
    anchor: 'output-mmar-flashback',
    title: 'Temporal Cue Recovery',
    kind: 'Flashback Timing',
    audioSrc: '/audio/mmar-flashback-timing.wav',
    question: 'At what second does the flashback start in the video?',
    groundTruth: '(D) 00:15:00',
    instructPrediction: '(C) 00:25:00',
    thinkTrace:
      'The clip has three speech segments, and the second one spans roughly 10.7s to 16.9s. That places the flashback cue closest to 15 seconds rather than 10, 20, or 25. The model explicitly reasons over segment timing instead of guessing from the answer choices. By localizing the middle spoken event and using its midpoint, it recovers the right temporal answer.',
    thinkFinal: '(D) 00:15:00',
    captionerTrace:
      'A strained father-daughter exchange escalates until the father says, “Hey, it’s been 15 minutes. Turn it off,” which anchors the clip near the correct answer. The captioner does not solve the multiple-choice task directly, but it surfaces the phrase containing the relevant temporal cue. That extra descriptive grounding still points to the right option.',
    captionerFinal: '(D) 00:15:00',
  },
  {
    anchor: 'output-mmar-mispronunciation',
    title: 'Speech Intention',
    kind: 'Mispronunciation',
    audioSrc: '/audio/mmar-mispronunciation.wav',
    question: 'Did this person mispronounce the word accidentally or intentionally?',
    groundTruth: '(C) Intentionally',
    instructPrediction: '(A) Accidentally',
    thinkTrace:
      'The speaker says “jalapenos,” gets corrected to “jalapeño peppers,” and then repeats “jalapenos.” Repeating the same form after correction suggests deliberate joking rather than an unaware slip. The correction-response pattern is the key evidence here. Once the speaker has been corrected and still repeats the same pronunciation, the mistake is no longer best explained as accidental.',
    thinkFinal: '(C) Intentionally',
    captionerTrace:
      'A male voice asks about jalapenos and then requests a croissant over faint background chatter. The captioner provides a compact surface description of the exchange, which is enough context for the final answer once paired with the question.',
    captionerFinal: '(C) Intentionally',
  },
]

const outputNavigator = [
  {
    section: 'Long Captions',
    href: '#outputs-long',
    items: longCaptionExamples.map((example) => ({ label: example.title, href: `#${example.anchor}` })),
  },
  {
    section: 'Time-Grounded Outputs',
    href: '#outputs-time',
    items: [
      ...showcaseVideos.map((video) => ({ label: video.title, href: `#${video.anchor}` })),
      ...timeGroundedExamples.map((example) => ({ label: example.title, href: `#${example.anchor}` })),
    ],
  },
  {
    section: 'MMAR Comparison',
    href: '#outputs-mmar',
    items: mmarComparisons.map((sample) => ({ label: sample.kind, href: `#${sample.anchor}` })),
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
        <p class="text-sm md:text-base text-gray-600 max-w-4xl mx-auto leading-relaxed">
          <a href="https://huggingface.co/nvidia/audio-flamingo-next-hf" target="_blank" rel="noopener"
            class="font-semibold text-teal-700 underline decoration-teal-300 underline-offset-3">AF-Next-Instruct</a>
          is the default checkpoint for QA, chat, and ASR-style use.
          <a href="https://huggingface.co/nvidia/audio-flamingo-next-captioner-hf" target="_blank" rel="noopener"
            class="font-semibold text-teal-700 underline decoration-teal-300 underline-offset-3">AF-Next-Captioner</a>
          is better when you want denser long-form captions and more descriptive outputs.
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

    <div id="model-outputs" class="section-card">
      <h2 class="text-3xl md:text-4xl font-bold mb-4 tracking-tight">Example Model Outputs</h2>
      <p class="text-base text-gray-700 leading-relaxed max-w-4xl">
        Jump directly to long captions, time-grounded captioning, timestamped ASR, and MMAR recovery cases. Each card
        keeps the task, source media, and model output together so the page is easier to scan.
      </p>

      <div class="output-browser">
        <div v-for="group in outputNavigator" :key="group.section" class="output-browser__group">
          <a :href="group.href" class="output-browser__heading">{{ group.section }}</a>
          <div class="output-browser__links">
            <a v-for="item in group.items" :key="item.href" :href="item.href" class="output-browser__link">
              {{ item.label }}
            </a>
          </div>
        </div>
      </div>

      <div id="outputs-long" class="output-section">
        <div class="output-section__heading">
          <h3 class="text-2xl md:text-3xl font-semibold tracking-tight">Long Captions</h3>
          <p class="text-base text-gray-600 leading-relaxed">
            Full long-form captions with the prompt and source video linked directly inside each card.
          </p>
        </div>
        <div class="output-grid output-grid--two">
          <article v-for="example in longCaptionExamples" :id="example.anchor" :key="example.title" class="output-card">
            <div class="output-card__meta">
              <span class="output-chip">{{ example.title }}</span>
              <span class="output-muted">{{ example.variant }}</span>
              <a :href="example.sourceUrl" target="_blank" rel="noopener" class="source-link">Watch on YouTube</a>
            </div>
            <p class="output-label">Prompt</p>
            <p class="output-prompt">{{ example.prompt }}</p>
            <p class="output-label">AF-Next-Captioner response</p>
            <div class="output-scroll output-scroll--long">{{ example.output }}</div>
          </article>
        </div>
      </div>

      <div id="outputs-time" class="output-section">
        <div class="output-section__heading">
          <h3 class="text-2xl md:text-3xl font-semibold tracking-tight">Time-Grounded Outputs</h3>
          <p class="text-base text-gray-600 leading-relaxed">
            Two video overlays with prompts, plus one timestamped ASR transcript example.
          </p>
        </div>
        <div class="time-grounded-pairs">
          <article
            v-for="pair in timeGroundedPairs"
            :id="pair.example.anchor"
            :key="pair.video.anchor"
            class="output-card time-grounded-card"
          >
            <div class="output-card__meta">
              <span class="output-chip">{{ pair.example.title }}</span>
              <span class="output-muted">{{ pair.example.variant }}</span>
              <a :href="pair.example.sourceUrl" target="_blank" rel="noopener" class="source-link">
                {{ pair.example.sourceLabel || 'Watch on YouTube' }}
              </a>
            </div>
            <p class="output-label">Overlay Preview</p>
            <div :id="pair.video.anchor" class="time-grounded-card__video-anchor">
              <video class="showcase-video" :src="pair.video.src" controls preload="metadata" playsinline />
            </div>
            <p class="time-grounded-pair__caption">{{ pair.video.description }}</p>
            <div class="time-grounded-card__details">
              <div class="time-grounded-card__detail-block">
                <p class="output-label">Prompt</p>
                <p class="output-prompt">{{ pair.example.prompt }}</p>
              </div>
              <div v-if="pair.example.showResponse" class="time-grounded-card__detail-block">
                <p class="output-label time-grounded-pair__response-label">{{ pair.example.responseLabel }}</p>
                <div class="output-scroll output-scroll--time">{{ pair.example.output }}</div>
              </div>
            </div>
          </article>
        </div>
      </div>

      <div id="outputs-mmar" class="output-section">
        <div class="output-section__heading">
          <h3 class="text-2xl md:text-3xl font-semibold tracking-tight">MMAR Comparison and Think Traces</h3>
          <p class="text-base text-gray-600 leading-relaxed">
            Three cases where AF-Next-Instruct misses the answer and AF-Next-Think or AF-Next-Captioner recover it.
          </p>
        </div>

        <div class="comparison-stack">
          <article v-for="sample in mmarComparisons" :id="sample.anchor" :key="sample.title" class="comparison-card">
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
                <p class="model-panel__label">Think trace excerpt</p>
                <p class="output-excerpt">{{ sample.thinkTrace }}</p>
                <p class="model-panel__final">Final answer: {{ sample.thinkFinal }}</p>
              </section>

              <section class="model-panel">
                <span class="model-badge model-badge--good">AF-Next-Captioner</span>
                <p class="model-panel__label">Grounding caption</p>
                <p class="output-excerpt">{{ sample.captionerTrace }}</p>
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

.output-section + .output-section {
  margin-top: 3.5rem;
}

.output-section__heading {
  margin-bottom: 1.5rem;
}

.output-browser {
  margin-top: 1.5rem;
  margin-bottom: 2.25rem;
  display: grid;
  gap: 1rem;
  border-radius: 1rem;
  border: 1px solid #d1d5db;
  background: rgba(249, 250, 251, 0.92);
  padding: 1.25rem;
}

.output-browser__group {
  display: grid;
  gap: 0.75rem;
}

.output-browser__heading {
  color: #0f766e;
  font-size: 0.95rem;
  font-weight: 800;
  letter-spacing: 0.08em;
  text-transform: uppercase;
}

.output-browser__links {
  display: flex;
  flex-wrap: wrap;
  gap: 0.75rem;
}

.output-browser__link {
  display: inline-flex;
  align-items: center;
  border-radius: 9999px;
  border: 1px solid #d1fae5;
  background: rgba(240, 253, 250, 0.78);
  color: #115e59;
  font-size: 0.92rem;
  font-weight: 600;
  padding: 0.5rem 0.85rem;
  transition: background-color 0.15s ease, border-color 0.15s ease;
}

.output-browser__link:hover {
  background: rgba(204, 251, 241, 0.95);
  border-color: #99f6e4;
}

.output-grid {
  display: grid;
  gap: 1.5rem;
}

.output-grid--two {
  grid-template-columns: repeat(2, minmax(0, 1fr));
}

.output-card {
  display: flex;
  flex-direction: column;
  border-radius: 1rem;
  border: 1px solid #d1d5db;
  box-shadow: 0 24px 45px -28px rgba(15, 23, 42, 0.24);
  background: linear-gradient(180deg, rgba(255, 255, 255, 0.98) 0%, rgba(248, 250, 252, 0.96) 100%);
  padding: 1.5rem;
}

.output-card--video {
  padding-bottom: 1.25rem;
}

.output-stack {
  display: grid;
  gap: 1.5rem;
}

.time-grounded-pairs {
  display: grid;
  gap: 1.5rem;
}

.time-grounded-card {
  min-width: 0;
}

.time-grounded-card__video-anchor {
  margin-top: 1rem;
}

.time-grounded-card__details {
  display: grid;
  gap: 1.25rem;
  margin-top: 0.5rem;
}

.time-grounded-card__detail-block {
  min-width: 0;
}

.time-grounded-pair__response-label {
  margin-top: 0;
}

.time-grounded-pair__caption {
  margin-top: 1rem;
  color: #374151;
  line-height: 1.7;
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

.output-label {
  margin-top: 1rem;
  color: #4b5563;
  font-size: 0.8rem;
  font-weight: 800;
  letter-spacing: 0.08em;
  text-transform: uppercase;
}

.output-excerpt {
  margin-top: 1rem;
  color: #374151;
  line-height: 1.75;
  white-space: pre-line;
}

.output-scroll {
  margin-top: 1rem;
  border-radius: 0.95rem;
  border: 1px solid #e5e7eb;
  background: rgba(248, 250, 252, 0.9);
  color: #374151;
  line-height: 1.75;
  white-space: pre-line;
  padding: 1rem;
  overflow: auto;
}

.output-scroll--long {
  max-height: 30rem;
}

.output-scroll--time {
  font-size: 0.97rem;
  max-height: none;
  overflow: visible;
}

.source-link {
  display: inline-flex;
  align-items: center;
  border-radius: 9999px;
  background: rgba(15, 118, 110, 0.08);
  color: #0f766e;
  font-size: 0.8rem;
  font-weight: 700;
  padding: 0.45rem 0.75rem;
  transition: background-color 0.15s ease;
}

.source-link:hover {
  background: rgba(15, 118, 110, 0.16);
}

.showcase-video {
  width: 100%;
  aspect-ratio: 16 / 9;
  border-radius: 0.9rem;
  border: 1px solid #d1d5db;
  background: #0f172a;
  box-shadow: 0 22px 40px -30px rgba(15, 23, 42, 0.45);
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
  .comparison-grid {
    grid-template-columns: 1fr;
  }

  .comparison-card__header {
    flex-direction: column;
  }
}
</style>
