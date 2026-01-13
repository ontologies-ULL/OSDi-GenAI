# OSDi Generative AI Experiments

This repository contains the full transcripts of the conversations conducted with generative artificial intelligence (GenAI) models during the experimental phase of the research project:
“Ontology-Guided Generative AI: A Prompt Engineering Study in Health Economic Evaluation Models.”

This project aims at exploring the potential of an ontology (https://w3id.org/ontologies-ULL/OSDi) to serve as a framework to streamline the early stages of Health Technology Assessment (HTA) modeling and support expert-driven processes.
 
Currently, the repository contains a first round of conversations (in Spanish), corresponding to a series of structured prompts sent to ChatGPT o-4.1 and Perplexity Deep Research as part of a factorial experimental design. During these conversations, the GenAI model is requested to generate structured instances relevant to HTA, using varying levels of contextual information and reproducing typical case uses for the HTA community.

### Factorial Matrix of Factors and Levels (HTA-Oriented)

| Factor | Levels (Id) | Supporting Hypotheses / HTA Relevance |
| :--- | :--- | :--- |
| **Use Case** | • Only PICO (UC1)<br>• PICO+Model (UC2)<br>• PICO+Report (UC3) | **UC1** evaluates the feasibility of early-stage conceptualization with scarce evidence.<br>**UC2** simulates the systematic population of established disease structures.<br>**UC3** assesses the automation of data extraction from extensive technical reports. |
| **Ontology Format** | • OWL (F1)<br>• RDF (F2) | Different formats influence technical reliability, syntactic validity, and the interoperability of the generated evidence model. |
| **Sample Individuals** | • Without individuals (I1)<br>• With individuals (I2) | Providing samples assesses if "few-shot" learning improves adherence to HTA domain standards and ontology syntax. |
| **Prompting Style** | • Complete upfront (P1)<br>• Iterative (P2) | Iterative refinement is expected to enhance methodological rigor, transparency, and completeness of the expert-guided generation process. |
| **Information Source** | • With sources (S1)<br>• Without sources (S2) | Structured sources are critical to ensure evidence-based accuracy and to mitigate the risk of clinical hallucinations. |
| **GenAI Model** | • Perplexity (M1)<br>• ChatGPT-4.1 (M2) | Evaluating models' performance is essential to identify tools capable of meeting the stringent reliability requirements of regulated HTA domains. |

---

## Folder structure

* **[UC1/](UC1/)** – Use case 1: Basic prompts with minimal context (PICO + task only). This use case represents early-stage/exploratory modeling, addressing the challenge of conceptualizing a decision problem when specific evidence is scarce

* **[UC2/](UC2/)** – Use case 2: Prompts with conceptual model context. This use case reflects the systematic population of parameters, simulating the transition from an established disease structure to a data-filled model.

* **[UC3/](UC3/)** – Use case 3: Full-context prompts based on a complete HTA report. Thus, it represents the automation of evidence extraction from technical reports, addressing the resource-intensive task of translating extensive HTA documentation into structured computational inputs

All content was generated between 16/04/2025 and 18/05/2025.

