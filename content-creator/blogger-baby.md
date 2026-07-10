---
name: blogger-baby
description: Writes original, engaging long-or-short-form articles/blog posts ready to publish on Medium, personal blogs, LinkedIn, or newsletters, on ANY topic — technical (programming, AI/ML, software, engineering, science) and non-technical (lifestyle, travel, food, essays, how-to, culture, business) alike, auto-adjusting terminology, depth, and voice to fit. Produces a title, hook intro, labeled sections with subheadings, image/diagram-placement suggestions, a conclusion, and natural SEO/GEO phrasing for search and AI-answer-engine pickup. Delivers a clean, human-sounding voice — varied sentence rhythm, concrete details, zero filler or AI-cliche phrasing — and adapts length from a short 400-word post to a 2000+ word feature. Use whenever the user asks to write, draft, or turn notes/info into a blog post, article, Medium post, newsletter piece, or technical explainer, even without the word "article." Also use if the user names the "blogger baby" or "blogger-baby" skill.
---

# Blogger Baby

A skill for turning a topic, a pile of notes, or a rough idea into a polished, publish-ready
article that reads like it was written by a person who actually knows the subject —
not like a template got filled in.

## Core philosophy

Good writing doesn't need to be disguised to look human — it just needs to *be* good.
This skill does NOT try to trick AI-detection tools. Instead it focuses on the things that
actually make writing engaging and natural:

- Real specifics instead of vague generalities
- Sentences that vary in length and rhythm (not a metronome of similar-length sentences)
- A clear point of view / voice, not a neutral "encyclopedia" tone
- Zero filler transitions and hollow AI-cliche phrases (see the "Phrases and habits to avoid" list at the bottom of this document)
- Structure that serves the reader (skimmable headers, short paragraphs), not structure for
  structure's sake

If the user explicitly asks for tricks to evade AI-content detectors, explain that this skill
focuses on writing that's genuinely engaging and clear instead — which is both more useful and
avoids platform-deception issues — and proceed with that approach.

## Workflow

### 0. Detect the topic type and adjust accordingly

This skill handles both tech and non-tech topics equally well — don't default to a "casual
lifestyle blog" voice for everything. Quickly classify the topic and adjust:

**Tech / technical topics** (programming, AI/ML, software tools, engineering, science,
finance/investing mechanics, health/medical mechanics, how-a-thing-works pieces):
- Get terminology exactly right — verify unfamiliar terms/claims with `web_search` rather than
  guessing. Technical accuracy matters more than tone here.
- It's fine and often expected to include code snippets (in fenced code blocks), commands,
  formulas, or step-by-step technical instructions where relevant.
- Define jargon in-line the first time it's used, briefly, instead of avoiding it — a technical
  reader wants the real term, not a vague paraphrase, but a mixed audience still needs the
  one-line translation ("a race condition — when two processes try to change the same data at
  the same time").
- Still avoid AI-cliche filler and still vary sentence rhythm — technical writing that reads like a
  dry spec sheet is just as big a turnoff as fluffy lifestyle filler.
- Diagrams/screenshots are often more useful than generic stock-photo-style images — mark these
  as `[IMAGE: diagram of X]` or `[IMAGE: screenshot showing X]` so it's clear what's needed.

**Non-tech topics** (lifestyle, travel, food, personal essays, how-to/DIY, culture, general
advice, business/soft-skills):
- Lean more into story, scene-setting, and relatable examples — this is where anecdote and
  sensory detail earn their keep.
- Keep jargon out entirely unless the reader would already know it.
- Image spots are more often illustrative/emotional (a scene, a finished result, a comparison
  photo) than diagrams.

**Mixed / uncertain**: many topics (e.g. "how AI is changing home cooking," personal finance,
health) sit in between — blend both approaches: get facts and terminology right, but keep the
narrative, human framing around them. When genuinely unsure how technical the audience is,
default to explaining terms briefly on first use rather than assuming prior knowledge.

### 1. Gather the essentials

If not already provided, quickly confirm (don't over-interrogate — infer sensible defaults and
move on if the user has already given enough):

- **Topic or source material**: a topic name, a set of notes/facts, or a link/document to base
  the article on
- **Length**: short (~400-600 words), medium (~800-1200 words), or long (~1500-2500+ words)
- **Tone**: e.g. conversational, punchy/opinionated, warm and personal, professional-but-approachable
- **Platform** (if it matters): Medium, personal blog, LinkedIn, newsletter — mostly affects
  formatting conventions and how much self-promotion/CTA is appropriate at the end

Don't block on all of this — if the user just says "write me a blog post about X," pick sensible
defaults (medium length, conversational tone) and note the assumption briefly.

### 2. Research for accuracy and freshness

If the topic involves current facts, statistics, trends, or anything time-sensitive, use
`web_search` to verify details and pull in a few real, current data points or examples. Real,
specific, checkable facts are what make an article feel authoritative and are also what
search engines and AI answer engines (ChatGPT, Perplexity, Google AI Overviews, voice
assists) tend to surface and quote. Don't invent statistics or sources.

Never copy sentences from search results into the article. Read for understanding, then write
the point in original words. See the copyright rules Claude always follows — this applies with
extra force here since the whole point is a plagiarism-free, original piece.

### 3. Draft the article

Structure (adapt as needed — this isn't a rigid template, sections should earn their place):

1. **Title** — specific and curiosity-driving, not generic. Prefer concrete nouns/numbers over
   vague hype. Should double as something a reader would actually search for.
2. **Hook intro** (2-4 short paragraphs) — open with a concrete scene, surprising fact, sharp
   claim, or relatable problem. Never open with "In today's world..." or a dictionary-definition
   sentence. Establish why the reader should keep going in the first two sentences.
3. **Body sections** — break into 3-6 clearly labeled sections with descriptive subheadings
   (not "Section 1", but the actual point of that section, ideally phrased as something a
   reader might search for or ask a voice assistant — this is what helps with GEO/AI-answer-engine
   pickup). Each section:
   - Leads with the point, then supports it (inverted-pyramid within the section)
   - Uses short paragraphs (2-4 sentences) — long unbroken blocks lose readers
   - Mixes in a concrete example, mini-story, analogy, or specific number wherever possible
   - Marks a suggested image spot where visuals would help, formatted as:
     `[IMAGE: short description of what should go here]`
4. **Optional FAQ or "quick takeaways" section** near the end for longer/how-to pieces — short
   Q&A pairs or a bulleted summary are exactly the format AI answer engines and voice assistants
   like to lift and cite, so include one when the topic is informational/how-to in nature.
5. **Conclusion** — don't just summarize; land on a final thought, a call to action, or a
   forward-looking note. Keep it short (1-3 paragraphs).

### SEO / GEO woven in naturally

- Identify 1-3 natural target phrases (what someone would type into Google or ask a voice
  assistant) and make sure they appear organically in the title, a subheading, and the first
  ~100 words — never stuffed or repeated unnaturally.
- Write subheadings as real questions/phrases people search, where it fits the content.
- Keep paragraphs short and headers descriptive — this is as much about human skimmability as
  about search/AI-crawler friendliness, and both benefit from the same clean structure.

### Word choice and pacing

- Simple, everyday words over jargon or thesaurus-fancy words. If a 10-year-old wouldn't
  know the word and a simpler one exists, use the simpler one.
- Vary sentence length deliberately: a short punchy sentence after a longer explanatory one
  reads as natural. Avoid a wall of same-length sentences.
- Check the draft against the list in the "Phrases and habits to avoid" section below and cut/rewrite any matches.
- Contractions are fine and usually make the tone warmer ("it's", "you'll", "don't").

### 4. Output

Always save the finished article as a Markdown file (even short pieces) — see
`file_creation_advice`: this is a standalone publishable artifact. Use `/mnt/skills/public/md`
conventions if present, otherwise plain clean Markdown with `#`/`##` headers. Save to
`/mnt/user-data/outputs/` and present it with `present_files`. Briefly note in chat: word count,
the 1-3 target search phrases used, and where image spots were placed — don't repeat the whole
article in chat.

---

## Phrases and habits to avoid

These patterns are the fastest way to make writing feel generic and templated. Scan every
draft against this list before finalizing.

### Overused opening/transition phrases
- "In today's fast-paced world..."
- "In the ever-evolving landscape of..."
- "Let's dive in / delve into..."
- "It's important to note that..."
- "Without further ado..."
- "At the end of the day..."
- "When it comes to X..."
- "Needless to say..."

### Overused connectors (fine occasionally, deadly in every paragraph)
- "Moreover," "Furthermore," "Additionally," "In conclusion," stacked one after another
- Starting three+ sentences in a row the same way

### Hollow intensifiers / hype words
- "game-changer," "revolutionize," "unlock the power of," "seamless," "robust," "elevate,"
  "unleash," "supercharge," "cutting-edge," "in a nutshell"

### Empty hedging that adds no information
- "It's worth mentioning that..."
- "One could argue that..."
- "Arguably one of the most..."

### Structural tells
- Every single paragraph being exactly 3 sentences
- Every section ending with a mini-summary sentence
- A conclusion that just restates each section title in order
- Overuse of rule-of-three lists ("faster, easier, and more efficient") in nearly every
  paragraph

### Fix by
- Naming the actual thing instead of the vague category ("a 40% jump in signups" beats
  "significant growth")
- Starting sentences differently — with a subject, a question, a short fragment, a number
- Letting some paragraphs be one sentence and others be four
- Cutting the sentence entirely if it's just throat-clearing before the real point
