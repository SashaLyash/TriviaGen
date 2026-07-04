# TriviaGen 📺

**LLM-powered trivia quiz generator with a TV-style UI** — inspired by "featured experience" apps on streaming platforms.

Pick a topic, pick a difficulty, and Gemini writes a 5-question multiple-choice quiz on the fly. Navigate with arrow keys and Enter — just like a TV remote.

> Built as a weekend prototype exploring AI-driven content generation for TV/streaming experiences, drawing on my background in Roku app development.

## Demo

*(add a screenshot or GIF here — `demo.gif`)*

## Features

- **On-demand quiz generation** — any topic, three difficulty levels, powered by the Gemini API
- **Structured LLM output** — prompt engineered for strict JSON schema (`responseMimeType: application/json`), with defensive parsing and validation of every question
- **TV-friendly UX** — large focus targets, glow focus states, grid-aware arrow-key navigation modeled on remote-control interaction patterns (SceneGraph-style focus management, reimplemented in vanilla JS)
- **Production-like error handling** — API key rejection, rate limits, malformed model output, and partial results are all handled with user-facing messages
- **Zero dependencies** — a single `index.html`. No build step, no framework, no storage. The API key lives only in page memory.

## Run it

1. Get a free Gemini API key at [Google AI Studio](https://aistudio.google.com/apikey)
2. Open `index.html` in a browser (or serve it: `npx serve .`)
3. Enter a topic, paste your key, hit **Generate Quiz**

## How it works

```
topic + difficulty
      │
      ▼
prompt (strict JSON schema, constraints on length/ambiguity)
      │
      ▼
Gemini 2.0 Flash (responseMimeType: application/json)
      │
      ▼
parse → validate each question (4 options, valid correctIndex)
      │
      ▼
TV-style quiz UI (focus navigation, scoring, results)
```

The prompt constrains the model to exactly 5 questions × 4 options with one `correctIndex`, and the client validates every item before rendering — malformed questions are dropped rather than crashing the quiz.

## What I'd do next

- **Serverless proxy** (Vercel function) so the API key never touches the client
- **Question caching** by topic+difficulty to cut latency and API cost
- **Streaming generation** — render question 1 while 2–5 are still generating
- **Roku port** — the focus-navigation model maps 1:1 to SceneGraph, making this a natural BrightScript port
- Difficulty auto-tuning based on the player's running score

## Stack

Vanilla JavaScript · HTML/CSS · Gemini API (`gemini-2.0-flash`)
