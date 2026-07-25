<div align="center">

# 🌤️ Weather Agent

**AI-powered weather assistant built with Mastra, Groq LLMs, and Open-Meteo — get real-time weather forecasts and activity recommendations in natural language.**

[![TypeScript](https://img.shields.io/badge/TypeScript-5.8-blue.svg?logo=typescript)](https://www.typescriptlang.org/)
[![Mastra](https://img.shields.io/badge/Mastra-Latest-purple.svg)](https://mastra.ai)
[![Groq](https://img.shields.io/badge/Groq-LLM-orange.svg)](https://groq.com)
[![License](https://img.shields.io/badge/License-ISC-green.svg)](LICENSE)
[![Node.js](https://img.shields.io/badge/Node.js-22+-green.svg?logo=node.js)](https://nodejs.org)

</div>

> **SEO Description:** Open-source TypeScript AI weather agent powered by Mastra framework and Groq LLMs (Llama 3.3). Real-time weather data via Open-Meteo API with intelligent activity planning workflows.

## Overview

Weather Agent is a production-ready AI assistant that delivers accurate weather information and context-aware activity recommendations through natural language conversations. Built on the Mastra agent framework with Groq's ultra-fast LLM inference, it combines geocoding, real-time weather data from Open-Meteo, and intelligent multi-step workflows to help users plan their day around the weather.

## Features

- 🌡️ **Real-Time Weather Data** — Current temperature, feels-like, humidity, wind speed, wind gusts, and detailed weather conditions powered by Open-Meteo API
- 🌍 **Intelligent Geocoding** — Automatically resolves city names to coordinates using Open-Meteo Geocoding API with multi-language support
- 🤖 **Groq LLM Integration** — Blazing-fast inference powered by Llama 3.3 70B on Groq hardware for sub-second responses
- 📋 **Activity Planning Workflows** — Multi-step workflow engine that analyzes forecasts and recommends morning/afternoon outdoor activities plus indoor alternatives
- 🔧 **Custom Tool System** — Zod-validated tool definitions with typed input/output schemas for reliable structured outputs
- 🪵 **Structured Logging** — Pino logger integration for production-grade observability
- 🏗️ **Mastra Framework** — Built on the modern Mastra agent framework with agents, tools, and workflows as first-class citizens
- ⌨️ **TypeScript Native** — Full type safety with Zod schemas throughout

## Tech Stack

| Layer | Technology |
|-------|-----------|
| **Language** | TypeScript 5.8+ |
| **Runtime** | Node.js 22+ |
| **Agent Framework** | [@mastra/core](https://mastra.ai) (latest) |
| **LLM Provider** | [Groq](https://groq.com) via [@ai-sdk/groq](https://sdk.vercel.ai/providers/ai-sdk-providers/groq) (Llama 3.3 70B Versatile) |
| **Weather API** | [Open-Meteo](https://open-meteo.com) (free, no API key required) |
| **Geocoding** | Open-Meteo Geocoding API |
| **Validation** | [Zod](https://zod.dev) 3.x |
| **Logging** | [Pino](https://github.com/pinojs/pino) via @mastra/loggers |
| **Dev Tooling** | Mastra CLI (`mastra dev`) |

## Architecture

```
weather-agent/
├── src/mastra/
│   ├── index.ts           # Mastra instance initialization (agents + workflows + logger)
│   ├── agents/
│   │   └── index.ts       # Weather Agent definition with Groq model + weather tool
│   ├── tools/
│   │   └── index.ts       # get-weather tool: geocoding → Open-Meteo API → structured output
│   └── workflows/
│       └── index.ts       # Weather workflow: fetch-weather step → plan-activities step
├── .env.example           # Environment variable template
├── package.json           # Dependencies and scripts
└── tsconfig.json          # TypeScript configuration
```

### How It Works

1. **User Query** → User asks about weather in natural language (e.g., "What's the weather in Tokyo?")
2. **Agent Processing** → The Weather Agent (powered by Groq Llama 3.3) determines if it needs to call the weather tool
3. **Geocoding Step** → City name is resolved to latitude/longitude via Open-Meteo Geocoding API
4. **Weather Fetch** → Current conditions are retrieved from Open-Meteo Forecast API (temperature, humidity, wind, weather code)
5. **Response Generation** → The LLM formats weather data into a natural language response with activity suggestions
6. **Workflow Mode** → For activity planning, a dedicated workflow runs: fetch forecast → LLM analyzes conditions → generates time-specific activity recommendations

## Quick Start

### Prerequisites

- **Node.js** 22+ ([install](https://nodejs.org))
- **Groq API Key** — Get one free at [console.groq.com](https://console.groq.com/keys)

### Installation

```bash
# Clone the repository
git clone https://github.com/yethikrishna/weather-agent.git
cd weather-agent

# Install dependencies
npm install
```

### Environment Setup

Copy the example environment file and add your Groq API key:

```bash
cp .env.example .env
```

Edit `.env`:

```env
GROQ_API_KEY=gsk_your_groq_api_key_here
MODEL=llama-3.3-70b-versatile
```

### Run the Agent

```bash
# Start the Mastra development server
npm run dev
# This runs: mastra dev
```

The Mastra dev server will start, and you can interact with the weather agent through the Mastra interface.

### Example Usage

The agent responds to queries like:

- "What's the weather in San Francisco?"
- "Should I bring an umbrella in London today?"
- "What activities do you recommend in Paris given the weather?"
- "Tell me the humidity and wind speed in Tokyo"

## Environment Variables

| Variable | Description | Required | Default |
|----------|-------------|----------|---------|
| `GROQ_API_KEY` | Groq API key for LLM inference ([get one](https://console.groq.com/keys)) | Yes | — |
| `MODEL` | Groq model identifier | No | `llama-3.3-70b-versatile` |

### Available Groq Models

- `llama-3.3-70b-versatile` — Best balance of speed and intelligence (default)
- `llama-3.1-8b-instant` — Fastest responses, good for simple queries
- `mixtral-8x7b-32768` — Large context window for complex workflows

## Project Structure

```
weather-agent/
├── src/
│   └── mastra/
│       ├── index.ts              # Mastra app entry point
│       ├── agents/
│       │   └── index.ts          # Weather agent with system prompt + tools
│       ├── tools/
│       │   └── index.ts          # get-weather tool (geocode + fetch + parse)
│       └── workflows/
│           └── index.ts          # Multi-step weather → activities workflow
├── .env.example                  # Environment template
├── .gitignore
├── description.txt               # Project description
├── package.json                  # Dependencies & scripts
├── README.md                     # This file
└── tsconfig.json                 # TypeScript config
```

## API Reference

### Weather Tool Output Schema

```typescript
{
  temperature: number;    // Temperature in Celsius
  feelsLike: number;      // Apparent temperature
  humidity: number;       // Relative humidity %
  windSpeed: number;      // Wind speed km/h
  windGust: number;       // Wind gusts km/h
  conditions: string;     // Human-readable condition (e.g., "Clear sky", "Moderate rain")
  location: string;       // Resolved location name
}
```

### Weather Conditions (WMO Codes)

The tool interprets WMO weather codes into readable conditions including: Clear sky, Mainly clear, Partly cloudy, Overcast, Foggy, Light/Moderate/Heavy drizzle, Light/Moderate/Heavy rain, Thunderstorm, Snow, and more.

## Deployment

### Local Development

```bash
npm run dev
```

### Production

For production deployment, you can:

1. **Build the TypeScript source:**
   ```bash
   npx tsc
   node dist/index.js
   ```

2. **Deploy to platforms** like Vercel, Railway, Render, or Fly.io that support Node.js.

3. **Docker deployment** — Create a Dockerfile with Node.js 22-alpine base.

## Contributing

Contributions are welcome! Feel free to:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

This project is licensed under the ISC License — see the [package.json](package.json) for details.

## Related Projects

- [Mastra](https://mastra.ai) — The TypeScript agent framework powering this project
- [Groq](https://groq.com) — Ultra-fast LLM inference platform
- [Open-Meteo](https://open-meteo.com) — Free, open-source weather API
- [Vercel AI SDK](https://sdk.vercel.ai) — The AI SDK used for Groq integration

---

<!--
SEO Keywords: weather agent, AI weather assistant, Mastra framework, Groq LLM, Llama 3.3, TypeScript AI agent, Open-Meteo API, weather chatbot, AI activity planner, weather forecasting AI, Node.js weather bot, geocoding weather, real-time weather API, AI workflow automation, Pino logging, Zod validation, Vercel AI SDK, open source weather AI, conversational weather, intelligent weather recommendations, weather tool AI, LLM weather agent, serverless weather, JavaScript AI agent, multi-step AI workflow
-->
