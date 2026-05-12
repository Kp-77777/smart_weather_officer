# Weather Assistant

An AI-powered weather assistant built with Streamlit, LangChain, Ollama, OpenWeatherMap, FAISS, and ElevenLabs.

The app can:

- Show the current weather for a city
- detect location and current date time
- Display a 5-day forecast in 3-hour intervals
- Plot forecast trends for temperature, humidity, and wind
- Answer weather questions through a ReAct-style agent
- Search weather knowledge from local PDF documents using RAG
- Read assistant responses aloud with ElevenLabs text-to-speech

## Project Overview

This project combines live weather data with a local LLM agent:

- `app.py`: Streamlit UI
- `reactagent.py`: LangChain ReAct agent setup
- `tools.py`: weather, forecast, time/date, and chart helpers
- `rag.py`: PDF ingestion and FAISS vector store loading/creation
- `prompt.py`: custom agent prompt
- `tts.py`: ElevenLabs speech output
- `data/climate_data/`: source PDF knowledge files
- `vector_store/pdf/faiss_index/`: saved FAISS index for PDF retrieval

## Requirements

Before running the app, make sure you have:

- Python 3.10+
- Ollama installed and running locally
- An OpenWeatherMap API key
- An ElevenLabs API key if you want voice output

### Ollama Models

This project uses these Ollama models:

- `mistral`
- `nomic-embed-text`

Pull them before running the app:

```powershell
ollama pull mistral
ollama pull nomic-embed-text
```

## Installation

1. Create and activate a virtual environment.
2. Install dependencies:

```powershell
pip install -r requirements.txt
```

## Environment Variables

Create a `.env` file in the project root with:

```env
openweather_api=your_openweathermap_api_key
ELEVENLABS_API_KEY=your_elevenlabs_api_key
```

Notes:

- `openweather_api` is required for current weather, forecast, and city-local time lookup.
- `ELEVENLABS_API_KEY` is only needed if you want the "Speak the Response" feature.

## Run the App

Start the Streamlit app with:

```powershell
streamlit run app.py
```

Then open the local Streamlit URL shown in your terminal, usually:

```text
http://localhost:8501
```

## How It Works

### 1. Live Weather Tools

The app calls OpenWeatherMap APIs to fetch:

- Current weather
- 5-day / 3-hour forecast
- City timezone data for local date and time

### 2. AI Weather Agent

The assistant in `reactagent.py` uses:

- LangChain ReAct agent
- `ChatOllama(model="mistral")`
- conversation memory for short chat history
- tool calls for weather, forecast, date/time, and weather knowledge

### 3. Weather Knowledge RAG

The app can load PDF files from `data/climate_data/`, split them into chunks, embed them with `nomic-embed-text`, and store them in FAISS.

If a saved vector store already exists in `vector_store/pdf/faiss_index/`, it is loaded automatically.

## Features

- Streamlit dashboard for weather lookup
- Forecast charting with Plotly
- Conversational weather assistant
- Local PDF-based knowledge retrieval
- Optional voice playback with ElevenLabs

## Example Questions

- "What is the weather in Delhi right now?"
- "what is normal temparature in eastern india?"
- "should I go outside for picnic today?"
- "why humidity affects rain?"

## Troubleshooting

### App cannot answer with the AI assistant

Check that:

- Ollama is installed and running
- `mistral` is available locally
- `nomic-embed-text` is available locally

### Weather data is not loading

Check that:

- your `openweather_api` value is valid
- your internet connection is working
- the city name is spelled correctly

### Voice output does not work

Check that:

- `ELEVENLABS_API_KEY` is set
- your ElevenLabs key is active

### PDF knowledge search returns nothing

Check that:

- PDF files exist under `data/climate_data/`
- the vector store can be created or loaded successfully

## Future Improvements

- Add unit tests
- Improve error handling and secrets management
- Support more weather providers
- Add multi-language support
- Allow document upload from the UI
