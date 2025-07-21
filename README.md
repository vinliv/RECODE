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

1. Background and Motivation
RECODE (REmote stimulation for COgnitive DEcline) addresses the global public health challenge of neurocognitive disorders, which have a profound impact on patients, caregivers, and healthcare systems. With rising life expectancy, the incidence of cognitive impairment and dementia is expected to increase sharply, creating an urgent need for effective, scalable treatments. While non-pharmacological interventions like Computerized Cognitive Training (CCT) have shown promise in improving cognitive function, their widespread adoption faces significant barriers. Many existing CCT tools are proprietary and expensive, suffer from poor design, and lack flexibility for customization. Furthermore, there is a scarcity of tools that are validated and culturally adapted for the Italian healthcare context.

To address these gaps, RECODE was developed as an open-access, browser-based platform to deliver CCT to older adults with Mild Cognitive Impairment (MCI) or mild-to-moderate dementia. Inspired by clinically validated paper-and-pencil tasks, RECODE translates established cognitive stimulation exercises into an accessible, web-native format. The platform is designed for remote, in-person, or hybrid delivery, requiring only a standard web browser and basic input devices. It is deployed using JATOS, an open-source research server, to ensure secure data management and ethical compliance. This repository contains the "frozen" code version used in the validation pilot study at the University of Padua's Hospital (Ethics Protocol #4246), which demonstrated the platform's feasibility and potential to produce clinically meaningful cognitive improvements.

2. Project Goals
RECODE was conceived to digitize and adapt established cognitive stimulation exercises, with the primary goal of making CCT more accessible, affordable, and effective for older adults with neurocognitive decline. Its core aims include:

Overcoming Barriers: To address the key challenges limiting the adoption of CCT in clinical practice, including high costs, poor usability, lack of customization, and insufficient validation in specific populations like Italian older adults.

Open-Source and Scalable: To provide a free, open-access tool that does not require expensive proprietary software or specialized hardware, making it a cost-effective and scalable option for healthcare institutions operating under budget constraints.

Clinical Usability and Accessibility: To implement an intuitive, user-friendly interface grounded in age-friendly design principles, featuring large text, high contrast, and simple point-and-click interactions to accommodate the cognitive and physical limitations of older adults.

Flexible Delivery Model: To support in-person, fully remote, or hybrid administration, allowing clinicians to tailor the intervention to the specific needs of the patient and enhance continuity of care.

Adaptive and Engaging Training: To deploy an adaptive difficulty mechanism that automatically adjusts task challenge based on user performance, keeping the training engaging and maintaining participant motivation and self-efficacy.

Reproducibility and Data Control: To build the platform on open-source technologies like jsPsych and JATOS, ensuring high temporal precision in measurements, transparent research protocols, and full institutional control over secure, pseudonymized data.

3. System Requirements
The platform is designed to run efficiently on widely available hardware and software.

Client-Side (Participant) Requirements:

Processor: Dual-core (minimum), Quad-core (recommended)

RAM: 4 GB (minimum), 8 GB (recommended)

Operating System: Windows 10+, macOS 10.14+, modern Linux distributions

Browser: Chrome 109+, Firefox 108+, Edge 109+

Screen Resolution: 1366×768 (minimum), 1920×1080 (preferred)

Input: A standard mouse and keyboard are required. Touch input is experimental and not recommended for clinical use.

Connectivity: An active internet connection is needed to run each training session.

Server-Side (Deployment) Requirements:

JATOS: Version 3.5+ (3.7+ recommended)

Java: A Java 11 Runtime Environment (JRE) is required to run JATOS.

Server RAM: At least 2 GB

Security: An HTTPS endpoint is mandatory for secure data transmission in a clinical or research setting.

4. Installation and Deployment
RECODE is a web-based experiment designed to be run through JATOS (Just Another Tool for Online Studies), a free, open-source server for managing behavioral research. You must have a working JATOS instance to deploy and run this project.

Prerequisites
Before deploying RECODE, you need a JATOS installation. You can set it up on your personal computer (a "local" installation for development and testing) or on an internet-facing server (a "global" installation for live data collection).

For detailed, step-by-step instructions on how to install JATOS on your specific operating system, please refer to the ().

Deploying and Running RECODE
Once your JATOS instance is running, deploying RECODE is a straightforward process:

Download the Study Package: Download the latest RECODE.jzip file from this repository's "Releases" section. This is a JATOS Study Archive that contains all the necessary code, assets, and configuration files. Do not unzip this file. Manually altering the archive will corrupt it and cause import errors.

Import into JATOS:

Log into your JATOS instance (e.g., at http://localhost:9000 for a local setup).

In the left-hand sidebar, click the + button and select Import Study.

In the file dialog, select the RECODE.jzip file you just downloaded.

Run the Study:

After a successful import, "RECODE" will appear in your list of studies in the sidebar.

Click on the study's name to open its main page.

Click the Run button in the top toolbar to begin a test session.

The recommended workflow is to use a local JATOS installation for development and testing. Once the study is finalized, use the Export function in your local JATOS to create an updated .jzip file, and then import this file into your server installation for data collection.

5. Architecture Overview
RECODE is built on a modern, open-source technology stack designed for flexibility and scientific rigor. The architecture consists of a browser-based frontend that runs on the participant's computer and a backend managed by JATOS for study administration and data collection.

Frontend: The user-facing experiment is implemented using HTML, CSS, and jsPsych (v7.2), a JavaScript library for creating web-based behavioral experiments. This allows for precise control over stimulus presentation and accurate measurement of responses and reaction times. The platform also utilizes the 

jspsych-psychophysics plugin for enhanced timing accuracy and stimulus flexibility.

Backend and Data Management: The study is hosted and managed by a JATOS instance. JATOS handles participant management by generating unique, personal links for each session, ensuring data is stored separately and securely. It also manages the flow between different components of the study (e.g., introduction, exercises, debrief). All behavioral data is sent from the client to the JATOS server and stored in both JSON and CSV formats, which can be accessed by authorized researchers through the JATOS graphical user interface.

Configuration: All experimental parameters—including stimuli, instructions, difficulty settings, and UI elements—are centralized in external JSON files. This allows researchers to modify the study's content and behavior without altering the core JavaScript code.

6. Exercise Taxonomy and Logic
The cognitive exercises in RECODE are digital adaptations inspired by the validated paper-and-pencil tasks in Demenza: 100 esercizi di stimolazione cognitiva (Bergamaschi et al., 2008). The initial version of the platform used in the pilot study included a total of 17 distinct exercises distributed across six core cognitive domains :

Attention (e.g., selective and sustained attention tasks)

Visual Memory

Language (e.g., naming, semantic categorization)

Executive Functions (e.g., Stroop-like and go/no-go paradigms)

Working Memory

Spatial-Temporal Orientation

Each domain contained multiple exercises, most with two predefined difficulty levels: "basic" and "advanced." The system logs participant responses, reaction times, and accuracy for each trial, which are then used to calculate block-level statistics for the adaptive progression mechanism.

7. Session Flow and Adaptive Progression
A typical RECODE session is designed to be structured yet flexible, guiding the participant through the training while adapting to their individual performance. The session flow is as follows :

Onboarding: The session begins with participant onboarding, demographic data collection, and avatar selection.

Exercise Blocks: A standard session consists of up to twelve exercises, with two exercises randomly selected from each of the six cognitive domains.

Instructions and Practice: Each exercise starts with an instruction screen. Participants can choose to read the instructions and proceed directly to the task or complete a short, interactive practice block with immediate feedback (a thumbs-up for correct answers, a thumbs-down for incorrect ones).

Adaptive Difficulty: All exercises begin at the "basic" level. The platform features an individual-specific adaptive staircase mechanism to keep the tasks challenging but not overwhelming.

Progression: If a participant achieves an accuracy of 80% or higher on both exercises within a cognitive domain, subsequent exercises from that domain will be presented at the "advanced" level.

Regression: If performance is below the threshold, the exercises remain at the "basic" level.

Feedback and Breaks: No feedback is provided during the main task. At the end of each exercise, overall performance is summarized with an emoji and a short message (e.g., a smiling emoji with "Great job!" for high accuracy). Participants can take short breaks after each exercise and a longer break at the session's midpoint.

Session Summary: The session concludes with a global report summarizing the average performance for each cognitive domain and a total score, followed by subjective feedback collection on fatigue and satisfaction.

8. Avatar and Feedback System
To enhance participant engagement, especially for older users who may have limited digital literacy, RECODE incorporates a user-friendly feedback system guided by an animated avatar. Participants can choose from one of four visual identities for their avatar (young/male, young/female, senior/male, senior/female).

The avatar guides participants through the session, provides instructions, and delivers motivational feedback. Performance is reinforced through both visual and verbal cues. For example, positive performance triggers celebratory animations like confetti and applause, while lower accuracy is met with constructive encouragement (e.g., an encouraging emoji with the message "You can do better!"). All avatar behaviors are synchronized with the trial timeline using jsPsych event hooks to ensure a seamless and engaging user experience.

9. Data Handling and Privacy
The RECODE platform was designed with strict privacy-by-design principles to ensure compliance with GDPR and Italian privacy law. All procedures were approved by the University of Padua Ethics Committee for Psychological Research (Protocol no. 4246).

Data Collection: Only essential, pseudonymized behavioral data (trial responses, reaction times, accuracy, and progression metrics) are collected. No personally identifying information (such as name, IP address, or email) is stored by the platform.

Data Security: Data transmission between the participant's browser and the server is encrypted using HTTPS/TLS. The JATOS server and the protected database are hosted and owned by the Department of General Psychology of the University of Padua, ensuring full institutional control over the data.

Data Access: Access to the raw data is restricted to authorized research staff via the secure JATOS interface. Session logs are available in both JSON and CSV formats for analysis and auditing.

Participant Rights: Participants provide informed consent and can withdraw from the study at any time, at which point any partial data is immediately deleted.

10. Known Bugs and Limitations
This repository contains a research build used for a pilot study. While functionally stable, it has several limitations and known issues:

Pilot Study Scope: The validation was a pilot study with a small sample size (n=12 per group). While results were promising, they require confirmation in larger, multicenter trials.

Supervised Setting: Although designed for remote use, the pilot study was conducted in a clinical setting under the supervision of neuropsychologists. This was necessary to gather real-time usability feedback but means its feasibility in a fully unsupervised home environment has not yet been established.

Alpha Stage Software: The platform is an evolving project. This pilot study helped identify areas for improvement, and subsequent versions have incorporated refinements to session duration, difficulty progression, and the exercise library. As such, this "frozen" version is considered an alpha build.

Accessibility Challenges: While designed to be user-friendly, feedback suggests that individuals with more severe cognitive impairments (e.g., MMSE score of 13) may find the exercises too demanding. Accessibility for screen readers is also limited.

Technical Issues:

Rapid sequential clicks can sometimes cause overlapping audio feedback.

Touch input is only partially supported and not intended for clinical use.

In case of a network interruption, only data from fully completed blocks are saved.

Content Limitations: All content is currently in Italian. Some visual stimuli may be duplicated across different exercise categories.

Open issues and progress toward future versions are tracked on the project’s .

11. Citation and Acknowledgments
If you use RECODE for research or clinical purposes, please cite the pilot study manuscript:

Ravelli, A., Livoti, V., Contemori, G., Macchia, E., Romeo, Z., Noale, M., Lucchi, E., Ghilardotti, G., Morandi, A., De Rui, M., Sergi, G., Maggi, S., Mapelli, D., Bonato, M., & Devita, M. (2025). RECODE Pilot Study: Preliminary Testing of a New Open, Computer-Based Platform for Cognitive Stimulation in Italian. Behavior Research Methods. Submitted. (*These authors contributed equally to the work) 

Additionally, please cite the foundational works and technologies:

Bergamaschi, S., Iannizzi, P., Mondini, S., & Mapelli, D. (2008). Demenza: 100 esercizi di stimolazione cognitiva. Raffaello Cortina Editore. 

de Leeuw, J. R. (2015). jsPsych: a JavaScript library for creating behavioral experiments in a Web browser. Behavior research methods, 47(1), 1–12.

Lange, K., Kühn, S., & Filevich, E. (2015). "Just another tool for online studies" (JATOS): an easy solution for setup and management of web servers supporting online studies. PLoS ONE, 10(6), e0130834.

We express our sincere gratitude to all participants and caregivers, the clinical and administrative staff at CDCD Padua, and the entire development team.

12. Contact, Author Contributions, and Funding
The RECODE project is the result of a multi-institutional collaboration between the University of Padua, the National Research Council (CNR), Cremona Solidale, and the University of Brescia.

Full Author List:
Adele Ravelli¹, Vincenzo Livoti²,³, Giulio Contemori³, Eleonora Macchia⁴, Zaira Romeo⁴, Marianna Noale⁴, Elena Lucchi⁵, Giorgia Ghilardotti⁵, Alessandro Morandi⁵,⁶, Marina De Rui¹, Stefania Maggi⁴, Giuseppe Sergi¹, Daniela Mapelli³, Mario Bonato²,³, Maria Devita¹,³ 

Affiliations:

Geriatrics Unit, Department of Medicine (DIMED), University of Padua, Padua, Italy

Padua Neuroscience Center (PNC), University of Padua, Padua, Italy

Department of General Psychology, University of Padua, Padua, Italy

Neuroscience Institute, Aging Branch, National Research Council (CNR), Padua, Italy

Azienda Speciale Cremona Solidale, Cremona, Italy

Department of clinical and experimental science, University of Brescia, Italy

Correspondence:
All correspondence should be addressed to:

Adele Ravelli: adele.ravelli@studenti.unipd.it

Vincenzo Livoti: vincenzo.livoti@phd.unipd.it

Dr. Giulio Contemori: giulio.contemori@unipd.it

Department of General Psychology, University of Padua, Via Venezia 8, 35131 Padua, Italy 

Funding:

Eleonora Macchia, Zaira Romeo, Marianna Noale, and Stefania Maggi acknowledge co-funding from Next Generation EU, in the context of the National Recovery and Resilience Plan, Investment PE8– Project Age-It: “Ageing Well in an Ageing Society” (DM 1557 11.10.2022).

Vincenzo Livoti is funded by the Ministerial Decree No. 118/2023 within the National Recovery and Resilience Plan (PNRR), as part of Mission 4, Component 1, Investment 3.4: "Digital and Environmental Transition", financed by European Union (EU) – NextGenerationEU.

The views and opinions expressed are only those of the authors and do not necessarily reflect those of the European Union or the European Commission. Neither the European Union nor the European Commission can be held responsible for them.

This repository is distributed under the MIT License. See(LICENSE.md) for details.
For technical issues, feature requests, or collaborative inquiries, please use the GitHub issue tracker or contact the corresponding authors.
