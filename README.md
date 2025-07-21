# RECODE

*Frozen implementation (jsPsych 7.2, JATOS 3.x) deployed during the 2024–2025 pilot trial at the University of Padua, Italy*

> ⚠️ **Preliminary Research Build**
> This repository contains the exact codebase used for participant sessions in the RECODE pilot study (January–June 2025, University of Padua). Although functionally stable for research, this version remains a development build and may include minor bugs, stylistic inconsistencies, or incomplete modules. It is intended for replication, peer review, and archival use only. Production or clinical deployment should await version 1, which will feature full QA, expanded accessibility, and enhanced reliability.

---

## Table of Contents

1. [Background and Motivation](#background-and-motivation)
2. [Project Goals](#project-goals)
3. [System Requirements](#system-requirements)
4. [Installation and Quickstart](#installation-and-quickstart)
5. [Architecture Overview](#architecture-overview)
6. [Exercise Taxonomy and Logic](#exercise-taxonomy-and-logic)
7. [Session Flow and Adaptive Progression](#session-flow-and-adaptive-progression)
8. [Avatar and Feedback System](#avatar-and-feedback-system)
9. [Data Handling and Privacy](#data-handling-and-privacy)
10. [Known Bugs and Limitations](#known-bugs-and-limitations)
11. [Citation and Acknowledgments](#citation-and-acknowledgments)
12. [Contact, Author Contributions, and Funding](#contact-author-contributions-and-funding)

---

## 1. Background and Motivation

**RECODE** (REmote stimulation for COgnitive DEcline) addresses the global public health challenge of neurocognitive disorders, which have a profound impact on patients, caregivers, and healthcare systems. With rising life expectancy, the incidence of cognitive impairment and dementia is expected to increase sharply, creating an urgent need for effective, scalable treatments. While non-pharmacological interventions like Computerized Cognitive Training (CCT) have shown promise, their widespread adoption faces significant barriers. Many existing CCT tools are proprietary and expensive, suffer from poor design, and lack flexibility for customization. Furthermore, there is a scarcity of tools that are validated and culturally adapted for the Italian healthcare context.

To address these gaps, RECODE was developed as an open-access, browser-based platform to deliver CCT to older adults with Mild Cognitive Impairment (MCI) or mild-to-moderate dementia. Inspired by clinically validated paper-and-pencil tasks, RECODE translates established cognitive stimulation exercises into an accessible, web-native format. The platform is designed for remote, in-person, or hybrid delivery, requiring only a standard web browser and basic input devices. It is deployed using JATOS, an open-source research server, to ensure secure data management and ethical compliance. This repository contains the "frozen" code version used in the validation pilot study at the University of Padua's Hospital (Ethics Protocol #4246), which demonstrated the platform's feasibility and potential to produce clinically meaningful cognitive improvements.

---

## 2. Project Goals

RECODE was conceived to digitize and adapt established cognitive stimulation exercises, with the primary goal of making CCT more accessible, affordable, and effective for older adults with neurocognitive decline. Its core aims include:

* **Overcoming Barriers:** Addressing key challenges limiting the adoption of CCT in clinical practice, including high costs, poor usability, lack of customization, and insufficient validation in specific populations like Italian older adults.
* **Open-Source and Scalable:** Providing a free, open-access tool that does not require expensive proprietary software or specialized hardware, making it a cost-effective and scalable option for healthcare institutions operating under budget constraints.
* **Clinical Usability and Accessibility:** Implementing an intuitive, user-friendly interface grounded in age-friendly design principles, featuring large text, high contrast, and simple point-and-click interactions to accommodate the cognitive and physical limitations of older adults.
* **Flexible Delivery Model:** Supporting in-person, fully remote, or hybrid administration, allowing clinicians to tailor the intervention to the specific needs of the patient and enhance continuity of care.
* **Adaptive and Engaging Training:** Deploying an adaptive difficulty mechanism that automatically adjusts task challenge based on user performance, keeping the training engaging and maintaining participant motivation and self-efficacy.
* **Reproducibility and Data Control:** Building the platform on open-source technologies like jsPsych and JATOS, ensuring high temporal precision in measurements, transparent research protocols, and full institutional control over secure, pseudonymized data.

---

## 3. System Requirements

The platform is designed to run efficiently on widely available hardware and software.

**Client-Side (Participant) Requirements:**

* **Processor:** Dual-core (minimum), Quad-core (recommended)
* **RAM:** 4 GB (minimum), 8 GB (recommended)
* **Operating System:** Windows 10+, macOS 10.14+, modern Linux distributions
* **Browser:** Chrome 109+, Firefox 108+, Edge 109+
* **Screen Resolution:** 1366×768 (minimum), 1920×1080 (preferred)
* **Input:** A standard mouse and keyboard are required. Touch input is experimental and not recommended for clinical use.
* **Connectivity:** An active internet connection is needed to run each training session.

**Server-Side (Deployment) Requirements:**

* **JATOS:** Version 3.5+ (3.7+ recommended)
* **Java:** Java 11 Runtime Environment (JRE) required for JATOS.
* **Server RAM:** At least 2 GB
* **Security:** An HTTPS endpoint is mandatory for secure data transmission in a clinical or research setting.

---

## 4. Installation and Quickstart

RECODE is a web-based experiment designed to be run through JATOS (Just Another Tool for Online Studies), a free, open-source server for managing behavioral research. You must have a working JATOS instance to deploy and run this project.

### Prerequisites

Before deploying RECODE, you need a JATOS installation. You can set it up on your personal computer (for development and testing) or on an internet-facing server (for live data collection).
For detailed, step-by-step instructions on how to install JATOS, refer to the [official JATOS documentation](https://www.jatos.org/).

### Deploying and Running RECODE

1. **Download the Study Package:**
   Download the latest `RECODE.jzip` file from this repository's [Releases](../../releases) section. This is a JATOS Study Archive containing all code, assets, and configuration files. **Do not unzip this file.** Manually altering the archive will corrupt it.

2. **Import into JATOS:**

   * Log into your JATOS instance (e.g., at `http://localhost:9000` for a local setup).
   * In the sidebar, click the "+" button and select *Import Study*.
   * Select the `RECODE.jzip` file.

3. **Run the Study:**

   * After import, "RECODE" will appear in your studies list.
   * Click on the study name, then click *Run* to begin a test session.

The recommended workflow is to use a local JATOS for development. Once finalized, export an updated `.jzip` file and import it into your server installation for data collection.

---

## 5. Architecture Overview

RECODE is built on a modern, open-source technology stack designed for flexibility and scientific rigor. The architecture consists of a browser-based frontend running on the participant's device and a backend managed by JATOS for study administration and data collection.

* **Frontend:**
  The user-facing experiment is implemented using HTML, CSS, and [jsPsych](https://www.jspsych.org/) (v7.2), a JavaScript library for creating web-based behavioral experiments. This allows for precise control over stimulus presentation and accurate measurement of responses and reaction times. The platform utilizes the `jspsych-psychophysics` plugin for enhanced timing and stimulus flexibility.

* **Backend and Data Management:**
  The study is hosted and managed by a JATOS instance. JATOS generates unique links for each session, ensures secure and separate data storage, and manages the flow between study components (e.g., introduction, exercises, debrief). All behavioral data are sent from the client to the JATOS server and stored in both JSON and CSV formats, accessible to authorized researchers via the JATOS GUI.

* **Configuration:**
  All experimental parameters—including stimuli, instructions, difficulty settings, and UI elements—are centralized in external JSON files. This design allows researchers to modify study content and behavior without editing the core JavaScript code.

---

## 6. Exercise Taxonomy and Logic

The cognitive exercises in RECODE are digital adaptations of validated paper-and-pencil tasks from *Demenza: 100 esercizi di stimolazione cognitiva* (Bergamaschi et al., 2008). The initial version of the platform, used in the pilot study, included 17 distinct exercises across six cognitive domains:

* Attention (e.g., selective and sustained attention tasks)
* Visual Memory
* Language (e.g., naming, semantic categorization)
* Executive Functions (e.g., Stroop-like, go/no-go paradigms)
* Working Memory
* Spatial-Temporal Orientation

Each domain contains multiple exercises, most with "basic" and "advanced" difficulty levels. The system logs responses, reaction times, and accuracy for each trial, which are used to calculate block-level statistics for adaptive progression.

---

## 7. Session Flow and Adaptive Progression

A typical RECODE session is structured yet flexible, guiding participants through training while adapting to individual performance.

* **Onboarding:** Session begins with onboarding, demographic data collection, and avatar selection.
* **Exercise Blocks:** Standard sessions consist of up to twelve exercises, with two randomly selected from each cognitive domain.
* **Instructions and Practice:** Each exercise starts with an instruction screen. Participants can read instructions or complete a short practice block with immediate feedback (thumbs-up/down for correct/incorrect).
* **Adaptive Difficulty:** Exercises start at the "basic" level. An individual-specific adaptive staircase mechanism keeps tasks challenging but not overwhelming.
* **Progression/Regression:** If accuracy ≥80% on both exercises within a domain, subsequent exercises in that domain move to "advanced" level; otherwise, they remain at "basic."
* **Feedback and Breaks:** No feedback is given during the main task. At exercise end, performance is summarized with an emoji and message. Short breaks after each exercise and a longer break at session midpoint are available.
* **Session Summary:** Each session concludes with a global report of average performance by domain and total score, followed by subjective feedback collection on fatigue and satisfaction.

---

## 8. Avatar and Feedback System

To enhance participant engagement—especially for older users with limited digital literacy—RECODE includes a feedback system guided by an animated avatar. Participants can choose from four visual identities (young/male, young/female, senior/male, senior/female).

The avatar guides participants, provides instructions, and delivers motivational feedback. Performance is reinforced through both visual and verbal cues: positive performance triggers celebratory animations (e.g., confetti, applause), while lower accuracy yields constructive encouragement (e.g., "You can do better!"). All avatar behaviors are synchronized with the trial timeline using jsPsych event hooks.

---

## 9. Data Handling and Privacy

RECODE was designed with strict privacy-by-design principles to ensure compliance with GDPR and Italian privacy law. All procedures were approved by the University of Padua Ethics Committee for Psychological Research (Protocol no. 4246).

* **Data Collection:** Only essential, pseudonymized behavioral data (trial responses, reaction times, accuracy, progression metrics) are collected. No personally identifying information (e.g., name, IP address, email) is stored.
* **Data Security:** Data transmission between browser and server is encrypted via HTTPS/TLS. The JATOS server and protected database are hosted and owned by the Department of General Psychology, University of Padua, ensuring institutional data control.
* **Data Access:** Raw data access is restricted to authorized research staff through the secure JATOS interface. Session logs are available in both JSON and CSV formats.
* **Participant Rights:** Participants provide informed consent and may withdraw at any time, triggering immediate deletion of partial data.

---

## 10. Known Bugs and Limitations

This repository contains the research build used for a pilot study. While functionally stable, it has several limitations and known issues:

* **Pilot Study Scope:** Validation was conducted in a small pilot sample (n=12 per group). Results are promising but require confirmation in larger, multicenter trials.
* **Supervised Setting:** Although designed for remote use, the pilot study was run in a clinical setting under neuropsychologist supervision. Feasibility in fully unsupervised environments is untested.
* **Alpha Stage Software:** This version is considered an alpha build. The pilot study informed subsequent improvements, which are ongoing.
* **Accessibility:** While user-friendly, participants with severe cognitive impairment (e.g., MMSE \~13) may find exercises too demanding. Screen reader support is limited.
* **Technical Issues:**

  * Rapid sequential clicks may cause overlapping audio feedback.
  * Touch input is only partially supported.
  * In case of network interruption, only completed blocks are saved.
* **Content Limitations:** All content is currently in Italian. Some visual stimuli may be duplicated across exercise categories.

Open issues and progress toward future versions are tracked in the project’s [issue tracker](../../issues).

---

## 11. Citation and Acknowledgments

If you use RECODE for research or clinical purposes, please cite the pilot study manuscript:

> **Ravelli, A.\*, Livoti, V.\*, Contemori, G., Macchia, E., Romeo, Z., Noale, M., Lucchi, E., Ghilardotti, G., Morandi, A., De Rui, M., Sergi, G., Maggi, S., Mapelli, D., Bonato, M., & Devita, M. (2025).**
> RECODE Pilot Study: Preliminary Testing of a New Open, Computer-Based Platform for Cognitive Stimulation in Italian. *Behavior Research Methods*. Submitted. (\*These authors contributed equally.)

Additionally, please cite these foundational works:

* Bergamaschi, S., Iannizzi, P., Mondini, S., & Mapelli, D. (2008). *Demenza: 100 esercizi di stimolazione cognitiva*. Raffaello Cortina Editore.
* de Leeuw, J. R. (2015). jsPsych: a JavaScript library for creating behavioral experiments in a Web browser. *Behavior Research Methods, 47*(1), 1–12.
* Lange, K., Kühn, S., & Filevich, E. (2015). "Just another tool for online studies" (JATOS): an easy solution for setup and management of web servers supporting online studies. *PLoS ONE, 10*(6), e0130834.

Sincere thanks to all participants and caregivers, the clinical and administrative staff at CDCD Padua, and the development team.

---

## 12. Contact, Author Contributions, and Funding

**RECODE** is a collaboration between the University of Padua, the National Research Council (CNR), Cremona Solidale, and the University of Brescia.

**Full Author List:**
Adele Ravelli¹, Vincenzo Livoti²,³, Giulio Contemori³, Eleonora Macchia⁴, Zaira Romeo⁴, Marianna Noale⁴, Elena Lucchi⁵, Giorgia Ghilardotti⁵, Alessandro Morandi⁵,⁶, Marina De Rui¹, Stefania Maggi⁴, Giuseppe Sergi¹, Daniela Mapelli³, Mario Bonato²,³, Maria Devita¹,³

**Affiliations:**

1. Geriatrics Unit, Department of Medicine (DIMED), University of Padua, Padua, Italy
2. Padua Neuroscience Center (PNC), University of Padua, Padua, Italy
3. Department of General Psychology, University of Padua, Padua, Italy
4. Neuroscience Institute, Aging Branch, National Research Council (CNR), Padua, Italy
5. Azienda Speciale Cremona Solidale, Cremona, Italy
6. Department of Clinical and Experimental Science, University of Brescia, Italy

**Correspondence:**

* Adele Ravelli: [adele.ravelli@studenti.unipd.it](mailto:adele.ravelli@studenti.unipd.it)
* Vincenzo Livoti: [vincenzo.livoti@phd.unipd.it](mailto:vincenzo.livoti@phd.unipd.it)
* Dr. Giulio Contemori: [giulio.contemori@unipd.it](mailto:giulio.contemori@unipd.it)
  Department of General Psychology, University of Padua, Via Venezia 8, 35131 Padua, Italy

**Funding:**

* Eleonora Macchia, Zaira Romeo, Marianna Noale, and Stefania Maggi acknowledge co-funding from Next Generation EU, in the context of the National Recovery and Resilience Plan, Investment PE8–Project Age-It: “Ageing Well in an Ageing Society” (DM 1557 11.10.2022).
* Vincenzo Livoti is funded by Ministerial Decree No. 118/2023 within the National Recovery and Resilience Plan (PNRR), Mission 4, Component 1, Investment 3.4: "Digital and Environmental Transition," financed by EU–NextGenerationEU.

> The views and opinions expressed are solely those of the authors and do not necessarily reflect those of the European Union or European Commission. Neither the European Union nor the European Commission can be held responsible for them.

---

This repository is distributed under the MIT License. See [LICENSE.md](LICENSE.md) for details.

For technical issues, feature requests, or collaboration inquiries, please use the [GitHub issue tracker](../../issues) or contact the corresponding authors.

---
