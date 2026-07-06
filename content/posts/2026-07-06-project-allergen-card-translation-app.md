---
title: 'Project: Allergen Card & Translation App'
date: 2026-07-06T09:49
description: How I built a high-accuracy, native iOS application to navigate complex dietary restrictions and local ingredient scanning completely offline.
---

Navigating dietary restrictions like Celiac disease internationally is a high-stakes challenge. Traditional translation utilities often fall short when dealing with niche technical terminology or hidden allergen variants (such as wheat-based shoyu or barley-derived cross-contaminants common in foreign food processing). 

To solve this, I designed and developed a native iOS application built entirely in Swift. The app functions as a 100% client-side safety net, combining structured digital allergen cards with real-time text parsing and fallback phrase generation—engineered to work flawlessly without cellular data or an internet connection.

## 🛠 Core Feature Implementation

### 1. Offline & Real-Time Camera Scanning

To parse foreign ingredient lists on physical product packaging, the app implements an on-device Optical Character Recognition (OCR) pipeline. 

\* \*\*The Architecture:\*\* Powered by Apple’s Vision Framework (\`RecognizeTextRequest\`), the app captures high-frequency camera frames via AVFoundation.

\* \*\*Technical Execution:\*\* It utilizes the engine's accurate recognition path, mapping localized multi-line bounding boxes into string sequences. Text normalization strips filler descriptions, allowing the app to scan text strings against localized arrays of blacklisted terms instantly.

### 2. On-Device AI Phrase Generation

When dynamic communication is necessary outside of pre-made templates, the app relies on a lightweight, privacy-focused client-side language engine.

\* \*\*The Architecture:\*\* Leveraging locally run translation utilities and contextual dictionary models, users can generate conversational phrases asking waitstaff about cooking environments, shared fryers, or ingredient substitutions.

\* \*\*Technical Execution:\*\* This removes reliance on network-based LLM endpoints, keeping processing fast, local, and completely free of third-party API costs or privacy-leak vectors.

### 3. Allergen Card Creation & Dynamic Localization

The foundational safety tool of the app allows users to create high-contrast, customizable medical/allergen declarations.

\* \*\*The Architecture:\*\* User profiles and custom card presets are persistent, managed via SwiftData. 

\* \*\*Technical Execution:\*\* Cards dynamically adapt to the target locale's language and cultural phrasing. Rather than using direct literal translation (which can dilute the severity of a medical condition), the system maps selected allergens to explicit, pre-vetted linguistic definitions indicating zero-tolerance for contamination.

---

## 🧠 Major Engineering Challenges Solved

### The Challenge: Bounding Box Mapping & Multi-line OCR Noise

During early testing, vertical ingredient layouts (such as traditional column layouts found on Japanese packaging) broke standard left-to-right text parsers, fracturing key compound kanji strings and missing allergen warnings entirely.

### The Solution

I built a geometric sorting layer directly on top of the Vision observations. By grouping text fragments into contextual rows and columns based on their layout bounding box origins before passing strings to the allergen matcher, parsing reliability jumped dramatically—eliminating dangerous false-negatives on dense, multi-column labels.

---

## 📈 Impact & Technical Takeaways

Building this application reinforced the power of leveraging Apple's native hardware-accelerated frameworks. By keeping the scanning, database querying (SwiftData), and phrase-mapping entirely on-device, the app achieves sub-100ms processing times, preserves battery health on the go, and guarantees absolute utility in remote regions where connectivity drops to zero.
