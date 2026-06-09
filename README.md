# Website-cloning-agent
AI-powered Website Cloning Agent that converts live websites into fully structured React + Tailwind CSS projects. Uses Playwright for rendering, BeautifulSoup for DOM extraction, and LLMs for code generation. Supports both local models via Ollama and cloud-based AI providers.

# AI Website Cloning Agent

An AI-powered website cloning agent that automatically converts live websites into fully structured React + Tailwind CSS projects.

## Overview

This project streamlines frontend development by analyzing an existing website and generating reusable React components with Tailwind CSS styling. The agent crawls the target website, extracts and cleans the DOM structure, and leverages Large Language Models (LLMs) to recreate the interface as production-ready frontend code.

Designed for developers, designers, and agencies, the tool significantly reduces the time required to prototype, redesign, or migrate websites.

## Features

* Clone websites from a URL
* Dynamic page rendering with Playwright
* Intelligent DOM extraction and preprocessing
* Automated React component generation
* Tailwind CSS styling generation
* Local LLM support via Ollama
* Optional cloud LLM integration for enhanced accuracy
* Automated project structure creation
* Export generated projects as ready-to-use source code

## Tech Stack

* Python
* Playwright
* BeautifulSoup
* React
* Tailwind CSS
* Ollama
* Qwen Coder / LLMs
* Google Gemini (Optional)

## Workflow

1. Enter a target website URL.
2. Playwright renders and captures the complete page.
3. BeautifulSoup extracts and cleans the DOM structure.
4. The processed data is sent to an LLM.
5. The LLM generates React + Tailwind code.
6. The project is automatically packaged into a deployable structure.

## Use Cases

* Rapid UI prototyping
* Website modernization projects
* Design inspiration and analysis
* Frontend migration
* Learning and educational purposes
* Accelerated client development workflows

## Disclaimer

This project is intended for educational, research, and legitimate development purposes. Users are responsible for ensuring compliance with applicable copyright, intellectual property, and website terms of service.
