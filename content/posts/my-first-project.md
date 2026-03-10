---
title: "Project: Training & Fine Tuning a LLM"
date: 2026-03-08
draft: false
tags: ["LLM Training", "Artificial Intelligence", "AI Engineering"]
categories: ["Projects"]
cover:
    image: "/images/llmdiagram.jpg" 
    alt: "LM Studio Diagram"
    caption: "Visualization of feeding data to train LLM"
    hiddenInSingle: true
---

## Executive Summary
For this project, I built a fully offline, privacy-focused AI coding assistant tailored specifically for Swift development. My goal was to create a secure environment where I could troubleshoot my own code without sending any data to cloud-based APIs like ChatGPT. To run the AI locally, I navigated the strict 8GB VRAM limits of my RTX 3070 Ti by deploying a heavily optimized Qwen-3B model via LM Studio. Because base AI models often lack the newest programming rules, I used a Retrieval-Augmented Generation (RAG) pipeline to feed the AI static, up-to-date Swift developer guides. This allowed me to safely bridge the gap between hardware constraints and the need for accurate, current coding assistance.

## Hardware Architecture
Running a Retrieval-Augmented Generation (RAG) pipeline locally requires balancing compute power with strict memory constraints. The foundation of this localized AI environment relies on consumer-grade hardware optimized for parallel processing:

* **Compute / CPU:** Intel Core i7-12700K (Ensures rapid document embedding and preprocessing).
* **Inference Accelerator (GPU):** NVIDIA GeForce RTX 3070 Ti (8GB VRAM). This acts as the primary engine for token generation.
* **System Memory:** 32GB DDR5. 
* **Operating Environment:** Dual-boot configuration (Windows / Linux Mint) to allow for bare-metal benchmarking of CUDA performance and driver latency across different OS kernels.

## Software & Model Stack
To ensure strict Data Loss Prevention (DLP) and guarantee zero network exfiltration, the entire software stack is hosted offline:

* **Inference Engine:** **LM Studio**. This serves as both the local server and the graphical interface. It handles the CUDA offloading, pushing the model layers directly to the RTX 3070 Ti's VRAM for hardware acceleration. 
* **The Core Model:** **Qwen-3B-Code (`Q4_K_M.gguf`)**. A 3-billion parameter model selected specifically for its efficiency. The 4-bit quantization (Q4) compresses the model's weights, allowing it to operate comfortably within the 8GB VRAM limit without spilling over into slower system RAM.
* **RAG Implementation:** **Local Vector Ingestion**. Confidential documents (such as simulated network logs or proprietary schematics) are embedded and queried directly on the machine. No external vector databases or third-party APIs are contacted, maintaining a hermetically sealed data environment.

## LLM Diagram
!["LM Studio Diagram"](/images/llmdiagram.jpg)

## Challenges & Solutions
### Challenge: Getting Up-to-Date Swift Documentation
My primary goal for this local AI was to use it as a secure coding assistant for Swift development. Because offline AI models (like the 3B model I used) don't always have the absolute newest programming rules in their base training data, I wanted to use RAG to feed the AI the most recent Swift developer guides. 

However, I ran into a major roadblock: I couldn't automatically scrape the official developer websites. The sites use dynamic formatting that broke my basic web scraping tools, meaning I couldn't automatically pull the live data into my AI's database.

### Solution: A Practical, Offline Workaround
Since building a highly complex, custom web scraper was outside the scope of my goal, I pivoted to a more practical solution to get the system working:

1. **Sourcing Static Files:** Instead of trying to force a live connection to the website, I sourced static, offline versions of the documentation (such as clean text exports and Markdown files of the Swift guides).
2. **Local Ingestion:** I manually loaded these clean files directly into my local RAG database. 
3. **The Result:** The AI could now read the updated Swift rules directly from my hard drive. 

While it wasn't the fully automated web-scraping pipeline I originally envisioned, this workaround solved the core problem. It allowed me to successfully use the offline model as a helpful, up-to-date Swift coding assistant while maintaining the strict privacy boundary of my local machine.

## What I Learned
Building this local AI environment was a massive learning experience that bridged the gap between hardware capabilities and software architecture. Key takeaways include:

* **Hardware Dictates AI Reality:** I learned firsthand how VRAM limits (like the 8GB on my RTX 3070 Ti) strictly dictate what models you can run. Understanding how to use quantization to squeeze a capable 3-Billion parameter model into that limited space taught me a lot about resource management.
* **Data Quality is the Hardest Part:** The scraping roadblock taught me that setting up the AI engine is often easier than acquiring the clean data to feed it. RAG relies entirely on the quality of its input, and AI models heavily prefer clean, static text (like Markdown) over messy HTML.
* **RAG is a Secure Reference Book:** I realized that RAG isn't magic—it is essentially giving the AI a custom, offline reference book to read before it answers. By loading the Swift documentation locally, I effectively updated the AI's knowledge base without needing to retrain the model itself.
* **Privacy by Design:** Using this as a coding assistant reinforced the value of Data Loss Prevention (DLP). I can paste my own broken code or private project notes into the prompt without ever worrying about that data being sent to a public cloud server.