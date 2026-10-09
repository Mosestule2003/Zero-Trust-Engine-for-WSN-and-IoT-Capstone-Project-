


THOMPSON RIVERS UNIVERSITY
Department of Computing Science
COMP 4910 – CAPSTONE PROJECT
MILESTONE 1: REQUIREMENTS AND ANALYSIS
ZERO-TRUST ENGINE FOR WSN AND IoT
Team Size: 4 Members
Team Members:
Magdala Nwuke (T00707082)
Kpughur-Tule Tertsegha Moses (T00721068)

Pranay Sethi (T00724613)
Ashima Bhasin (T00729543)



1. Project Overview
Traditional cryptographic authentication methods rely primarily on digital credentials such as cryptographic keys. This poses a problem: if these credentials are compromised, an attacker can impersonate or clone a legitimate device. This project lays the data foundation for a multi-layered radio-fingerprinting approach to defend against such attacks, reducing reliance on traditional keys. This project is broken into Phase 1 and Phase 2, of which Phase 1 is the core focus for the project.
The main objective of Phase 1 is to deploy a robust RF data-acquisition and signal-processing pipeline capable of capturing raw In-Phase and Quadrature (I/Q) samples from wireless transmitters. These manufacturing defects introduce subtle, device-specific variations in transmitted radio-frequency signals, providing the groundwork for future zero-trust recognition and device classification.
The experimental setup will consist of several Zigbee mesh nodes communicating through a central wireless gateway (Raspberry Pi), with a Software-Defined Radio (SDR) used to record their RF (radio frequency) transmissions. Using Python-based scripts, we will process the captured I/Q data and extract key statistical features such as variance, skewness, and kurtosis. These features will form an RF fingerprint for each transmitter to support future classification research.
In Phase 2 context, analyzing RF signals from numerous devices in real time creates a trade-off between processing speed and accuracy. To address this, future research may explore a proposed two-tier detection architecture. Under this future conceptual model, Tier 1 would utilize Mahalanobis Distance as a lightweight gatekeeper to quickly process matching signals with minimal computational overhead, routing anomalous signals to Tier 2.
Tier 2 is envisioned for future Phase 2 work to employ a Support Vector Machine (SVM) as a precision classifier for detailed anomaly analysis and attack simulation. While Phase 1 focuses exclusively on data collection, dataset curation, and feature extraction, this two-tier classification architecture serves as the conceptual framework for Phase 2 research.
The Phase 1 data collection system will be evaluated based on pipeline reliability, metadata recording accuracy, dataset completeness, and throughput performance. Experiments will measure data capture consistency, processing latency, CPU utilization, and system performance as wireless traffic increases.
The primary outcome of Phase 1 is a working prototype pipeline and empirical dataset of captured RF signals, accompanied by a Python feature-extraction workflow. This deliverable provides the essential dataset and baseline analysis to enable future Phase 2 evaluation of multi-tier zero-trust machine-learning models.
2. Project Requirements
The primary requirement of the proposed Zero-Trust Engine is to establish a reliable radio-frequency data-acquisition process for identifying device-specific characteristics in wireless sensor networks. The initial development priority is the collection, organization, and preparation of raw In-phase signal data from Zigbee devices using Software-Defined Radio hardware. The resulting dataset will provide the foundation for subsequent device-identification and machine-learning research.
2.1 Functional Requirements

ID
Requirements
Priority
FR-01
The system will capture raw I/Q signals transmitted by Zigbee devices using SDR hardware.
High
FR-02
The project will establish a repeatable procedure for capturing RF transmissions from known devices.
High
FR-03
Captured signals will be correlated with the transmitting device and relevant data.
High
FR-04
The project will provide a structured method for storing, organizing, and labeling captured RF data.
High
FR-05
Captured data will undergo appropriate preprocessing to ensure it is suitable for subsequent analysis.
High
FR-06
The project will produce an initial RF dataset containing multiple recordings from the Zigbee devices.
High
FR-07
The collected data will support analysis of RF characteristics that can distinguish individual transmitting devices.
High
FR-08
The collection pipeline will label captured transmissions with the transmitting device ID and capture session details.
High
FR-09
The data collection pipeline will record environmental and session metadata alongside the I/Q data, including timestamps, temperature, and capture parameters.
High
FR-10
The collection system must be capable of running reliably for 2 to 4 days autonomously, including a mechanism to automatically report faults or notify the team if a node stops working.
High
FR-11
The pipeline will format the preprocessed data into standardized formats (e.g., CSV, Parquet) and store it in a centralized database or remote server, enabling streamlined integration with analysis toolkits such as Pandas.
High

2.2 Non-Functional Requirements

ID
Non-Functional Requirement
Priority
NFR-01
Data Integrity: The capture pipeline must maintain high fidelity and integrity of raw I/Q samples without unrecorded packet loss during acquisition sessions.
High
NFR-02
Traceability: Every recorded RF dataset must include accurate metadata mapping signal samples to specific transmitter IDs, channels, and timestamps.
High
NFR-03
Scalability: The database and dataset architecture must easily accommodate additional devices, higher sample rates, and extended recording durations.
Medium
NFR-04
Reliability: The autonomous collection pipeline must operate continuously for 2 to 4 days without system crashes or data corruption.
High
NFR-05
Usability & Standardization: Formatted outputs (CSV/Parquet) must follow standard data structures for seamless ingestion into Python analytics tools.
High
NFR-06
Repeatability: Capture configuration scripts must allow identical data collection procedures to be replicated across different physical setups.
High

2.3 Requirement Analysis & External Dataset Research
To ensure a robust methodology for data collection and feature extraction, the team will conduct research on existing standards, open-source datasets, and hardware capabilities prior to building the collection pipeline.
Open-Source Dataset Review
The team will review publicly available datasets containing RF recordings captured using software-defined radios. These datasets will be used to study data formatting, preprocessing, RF feature extraction, device identification, and machine-learning evaluation before and alongside the project's own Zigbee data collection.

Dataset
Zigbee Data
SDR / Capture
Main Contents
Access
Use in Project
SDR4IoT BLE & Zigbee RF Dataset
Yes
USRP N210 + GNU Radio
Raw I/Q and demodulated Zigbee traffic collected from 2.4 GHz IoT nodes in the w-iLab.2 testbed; different emitter positions and numbers of emitters
Free download via Zenodo
Primary external dataset for testing the RF/IQ collection and preprocessing pipeline
ZIGBEE_RFF_DATASET
Yes
USRP N210
RF recordings from 60 CC2530 Zigbee devices at 2505 MHz and 10 MS/s; >34,000 frames across indoor/outdoor and LOS/NLOS conditions
IEEE DataPort
Primary benchmark for device fingerprinting, feature extraction and classifier testing
ZigBee Signals Under Different SNRs
Yes
SDR-based RF recordings
Public Zigbee RF data specifically organized for evaluating performance under different SNR/noise conditions
IEEE DataPort
Useful for testing whether extracted fingerprints remain distinguishable as signal quality changes
ZigBee Signals Across Different Scenarios
Yes
SDR-based RF recordings
Public Zigbee RF data collected across different propagation/capture scenarios
IEEE DataPort
Useful for testing environment/channel robustness
Sparclab RF-PUF / XBee S2C Dataset
Yes
HackRF One + GNU Radio
Raw binary RF data from 30 XBee S2C transmitters, captured both with and without the wireless channel; approximately 155.4 GB
Sparclab RF-PUF dataset
Useful for testing I/Q processing, CFO-related features and transmitter-specific fingerprints

3. Dataset and Feature Extraction
3.1 Dataset Structure
The Phase 1 dataset will contain RF recordings collected from the XBee3 transmitters using the ADALM-PLUTO SDR. The data will be organized so that each recording, processed signal segment, and extracted feature can be traced back to its source device and collection session.
Each transmitter will have a device ID linked to its physical XBee module and serial address. Every collection session and recording will also receive a unique ID.

Dataset component
Organization and format
Raw I/Q files
Original complex I/Q samples stored as NumPy .npy files, organized by session and device. Original recordings will remain unchanged.
Metadata table
A CSV table containing one row per recording, including device identity, acquisition settings, environmental information, and the raw filename.
Preprocessed data
Usable signal segments stored separately from raw recordings, with references to their source recording and segment boundaries.
Extracted features
A table containing one row per processed segment, with device, session, recording, and segment IDs alongside the calculated features.
CSV/Parquet output
CSV will be the initial format for metadata and feature tables. Parquet will be used when larger tables require more efficient storage and loading.

Files will follow a consistent naming convention. For example, S001_XB01_R0001.npy identifies session S001, device XB01, and recording R0001. Processing scripts will preserve these identifiers throughout the workflow.
Completed data will be backed up to the project’s designated centralized storage or remote server. A short dataset description will document file formats, sample types, units, preprocessing settings, and feature definitions.
Preprocessed I/Q segments will also be exported as Parquet tables containing recording IDs, segment IDs, sample indices, and separate I and Q values, and stored on the designated centralized server.
3.2 Example Metadata Table
The following table illustrates one example recording. The values are illustrative rather than actual measurements or confirmed acquisition settings.

The full metadata table will also include the XBee serial address, temperature-reading timestamp, sample count, capture status, and dataset source. Missing values will be marked as unavailable. Recordings with uncertain transmitter identity will be flagged and excluded from the verified dataset until their labels can be resolved.
3.3 Initial Features to Extract
Phase 1 will begin with statistical features calculated from the amplitude of each usable signal segment. Amplitude will be calculated from the I/Q samples as:
Amplitude = √(I² + Q²)
Using the same feature definitions and processing settings across recordings will allow consistent comparisons.

Feature
Description
Variance
Measures how much the amplitude varies around its mean.
Skewness
Measures the asymmetry of the amplitude distribution.
Kurtosis
Describes the tail behaviour of the amplitude distribution.
Mean amplitude
Measures the average amplitude within a segment.
RMS amplitude
Summarizes the overall amplitude level.
Peak amplitude
Records the maximum amplitude within a segment.
Mean squared amplitude
Provides a relative signal-power measure in recorded sample units.

The preprocessing procedure will identify transmission segments, check for invalid samples, and apply a consistent segment-length policy. The selected settings will be documented so that feature extraction can be repeated.
Amplitude features will be calculated before amplitude normalization. Receiver gain, distance, and other collection conditions will remain available in the metadata because they can affect these measurements.
The scripts will document the variance convention and the settings used for skewness and kurtosis, including whether kurtosis is reported as excess kurtosis. Undefined statistics, such as those arising from constant-amplitude segments, will be flagged rather than replaced with arbitrary values.
No additional RF features are treated as confirmed at this stage. Features such as frequency-offset estimates may be added after team agreement and validation of the required processing method. Phase 1 will produce feature tables and exploratory analysis; it will not claim that the initial features uniquely identify devices.
3.4 Use of External Datasets
The external datasets listed in Section 2.3 will support development and testing of the data-processing workflow before and alongside local data collection. They will not replace the dataset collected using the project’s own XBee3 transmitters and ADALM-PLUTO SDR.
Before using an external dataset, the team will check its access conditions, licence, file format, device labels, and available acquisition metadata.

Activity
How external datasets will be used
Data loading
Develop readers for the supplied formats and verify sample structure, labels, and recording parameters.
Preprocessing
Test signal segmentation and checks for incomplete or invalid recordings. Processing will account for each dataset’s sampling settings.
Feature extraction
Apply the initial statistical features to compatible I/Q recordings and check that outputs are consistent and reproducible.
File formatting
Convert available metadata and extracted features into the project’s CSV/Parquet table structure while retaining original identifiers.
Analysis scripts
Test plots, summary statistics, missing-value checks, and comparisons across devices or recording conditions.

SDR4IoT will be considered for loading and preprocessing tests. The listed CC2530 and XBee S2C datasets may support feature-extraction testing where access and formats permit. Datasets containing different SNRs or recording scenarios may support exploratory analysis of how signal conditions affect the features.
External recordings will be stored separately from local recordings and identified through a dataset-source field. Missing environmental or acquisition information will be marked as unavailable. Differences in transmitters, SDR hardware, sample rates, and environments will be documented before results are compared.
3.5 Environmental Metadata
Environmental information will be recorded alongside the signals to support repeatable collection and help explain differences between sessions.

Metadata
What will be recorded
Measurement method
Temperature
Ambient temperature in °C and measurement timestamp.
Proposed digital ambient-temperature sensor connected to the Raspberry Pi.
Distance
Distance between each transmitter antenna and the SDR receiving antenna, in metres.
Measuring tape, recorded before each session and after movement.
Device position
Position marker and antenna orientation for each transmitter.
A labelled testbed layout and session notes.
Channel
Configured Zigbee channel and corresponding SDR center frequency.
Network and SDR configuration logs.
Location/environment
Collection location, indoor/outdoor setting, and relevant environmental changes.
Session notes and timestamped event logs.

Because the existing requirements specify temperature metadata, Phase 1 will include ambient-temperature measurement. The proposed method is a Raspberry Pi-compatible digital temperature sensor placed near the testbed. Its model, accuracy, and interface will be documented when selected.
An initial temperature logging interval of one minute is proposed. Each recording will be associated with the nearest temperature reading within one minute; readings outside that interval or missing readings will be flagged as unavailable. Ambient temperature will be kept distinct from Raspberry Pi CPU temperature or internal transmitter temperature.
Device positions will be marked to help reproduce the setup. Movement, changes in antenna orientation, and noticeable changes in the surrounding environment will be logged when they occur.
Phase 1 will record these conditions during normal data collection. Controlled experiments that deliberately vary temperature, distance, or positioning will remain future work, consistent with the existing project scope.
4. Stakeholder Analysis
4.1 Stakeholder Identification & Influence
The project involves developing an RF data-acquisition pipeline that captures, organizes, and labels Zigbee radio signals for device identification. The following stakeholders are relevant to the project's development, implementation, testing, and successful delivery.

Stakeholder
Influence
Project team members
High
Project supervisor / instructor
High
Hardware and equipment providers
Medium
Future researchers / ML developers
Low
IoT network administrators / end users
Low
Academic evaluators
High

4.2 Stakeholder Interests and Responsibilities
Project Team Members
Coordinate development and integration of project components.
Ensure the RF data-collection process is repeatable and reliable.
Maintain consistent device labels, metadata, and dataset formats.
Share progress, technical challenges, and test results.
Project Supervisor / Instructor
Ensure the project follows its defined scope and requirements.
Review technical decisions, methodology, and progress.
Provide feedback on experimental design and deliverables.
Verify that project outcomes meet academic expectations.
Hardware and Equipment Providers
Provide access to compatible Zigbee devices, SDR equipment, and Raspberry Pi hardware.
Supply technical documentation and configuration details.
Help establish compatibility between the radio hardware and recording software.
Future Researchers / ML Developers
Require clean, correctly labeled, and well-documented RF recordings.
Need consistent metadata and reproducible data-collection procedures.
Use the dataset for feature extraction, classification, and anomaly detection.
IoT Network Administrators / End Users
Need dependable identification of known wireless devices.
Have an interest in reducing device impersonation and unauthorized access.
Benefit from research that evaluates RF fingerprinting alongside traditional security mechanisms.
Academic Evaluators
Assess whether the project meets its functional and non-functional requirements.
Review the dataset, implementation, experimental methodology, and final report.
Evaluate documentation, evidence of testing, and achievement of project objectives.
4.3 Key Stakeholder Risks
Project Team Members
Poor communication could cause delays or duplicated work.
Uneven workload may affect project progress.
Project Supervisor / Instructor
Delayed feedback could slow important technical decisions.
Misunderstanding project expectations could lead to scope changes.
Hardware and Equipment Providers
Hardware may be unavailable or delayed.
SDR, Zigbee, and Raspberry Pi components may have compatibility issues.
Future Researchers / ML Developers
Incomplete or improperly formatted Phase 1 RF datasets could hinder future classification research.
Missing metadata could reduce the usefulness of the dataset.
IoT Network Administrators / End Users
Inconsistent RF signal collection under varying environments could impair dataset validity.
The system may not perform consistently in different environments.
Academic Evaluators
Insufficient documentation or testing evidence could affect project evaluation.
Incomplete project requirements could affect the final assessment.
5. Required Hardware
For this stage of the project, four main types of hardware are required to establish a Zigbee network, transmit Zigbee RF signals, capture the raw RF signals, and automatically store and label the collected recordings.
Zigbee Devices – Digi XBee3 Zigbee Mesh Development Kit
The project will use Digi XBee3 Zigbee 3.0 modules operating at 2.4 GHz. Two Digi XK3-Z8S-WZM development kits will be purchased, providing six XBee3 modules. Three will be used as primary test devices, while the remaining modules can support coordination, additional testing, or serve as spares.
Zigbee Coordinator
One XBee3 module will be configured as the Zigbee coordinator to manage communication with the Zigbee end devices. Coordinator information will be used by the Raspberry Pi to help associate captured RF data with the correct device and metadata.
Software-Defined Radio – ADALM-PLUTO
One ADALM-PLUTO (PlutoSDR) will be used to capture raw I/Q samples from the 2.4 GHz Zigbee channel. Sampling and acquisition settings will be determined during integration testing.
Raspberry Pi
The Raspberry Pi will run the Python-based data collection pipeline, including capture control, monitoring, labelling, metadata collection, and storage. It will combine SDR captures with coordinator information to organize the RF dataset by device, channel, timestamp, and related metadata.
Finalized Hardware Procurement Cost

Hardware
Quantity
Role
Selected Model
Supplier
Cost/Price
URL
Software-Defined Radio
1
RF/IQ Capture
ADALM-PLUTO SDR
Newark
$466.81 CAD
Link
Zigbee Dev Kit
2 kits (6 modules)
Transmitters & Coordinator
Digi XBee3 XK3-Z8S-WZM
Mouser
$274.89 CAD
Link
Single-board Computer
1
Gateway & Pipeline Host
Raspberry Pi
Lab Provided
$0.00 CAD
N/A

The Digi XBee 3 Zigbee Mesh Kit was selected as the primary platform because it provides three matching Zigbee modules within a single kit, making it suitable for collecting and comparing RF signals from consistent hardware. The modules can also be configured to play different roles, allowing one module to act as the coordinator while the others operate as end devices. This reduces the need for a separate third-party coordinator and provides a reliable, cost-effective platform for the initial testbed.
6.3 Proposed Architecture
Following the hardware validation and analysis, the proposed architecture, shown in the diagram below, connects the Zigbee network and RF capture system through the Raspberry Pi. When a Zigbee device transmits, the communication is received by the Zigbee coordinator while the ADALM-PLUTO independently captures the RF transmission as raw I/Q samples. Both the coordinator and SDR are connected to the Raspberry Pi, allowing network information and RF recordings to be collected within the same test environment.
Python scripts running on the Raspberry Pi will automate the collection process, configure the SDR, monitor the connected hardware and network traffic, and organize the captured data. The resulting raw I/Q recordings, device labels, collection metadata, and extracted signal features will then be stored as part of the RF dataset.

Figure 1: Proposed RF Data Collection and Hardware Architecture
7. Risk Register

ID
Risk
Likelihood
Impact
Mitigation
1
Insufficient/ poor quality RF data
Medium
High
Collect multiple samples from each device under controlled conditions and verify signal quality before dataset finalization.
2
RF interference/noise
High
High
Conduct experiments in a controlled environment and apply signal preprocessing/filtering.
3
SDR/Zigbee hardware failure
Low
High
Test equipment before experiments, maintain backups, and document configurations.
4
Similar RF fingerprints between devices
Medium
High
Evaluate additional signal features and collect larger datasets to improve device differentiation.
5
Gateway Processing bottleneck
Medium
High
Use the two-tier architecture so Mahalanobis Distance handles normal traffic and the SVM is invoked only when necessary.
6
Excessive latency when network traffic increases
Medium
High
Test the gateway at different traffic levels and optimize the feature-extraction/classification pipeline.
7
Insufficient training data
Medium
High
Capture repeated transmissions from multiple devices across extended collection sessions.
8
Environmental changes distort RF signals
Medium
Medium/High
Collect data across structured variations in distance, positioning, and environment to ensure dataset robustness.
9
Project scope exceeds available time
Medium
High
Prioritize a minimum working prototype before adding additional devices, features, or attack scenarios.
10
Integration issues between SDR, Raspberry Pi, Python, and ML components
Medium
Medium
Develop and test each hardware component and acquisition script separately before full pipeline integration.
11
Loss/corruption of experimental data
Low
High
Maintain organized backups of raw I/Q data, extracted features, model configurations, and results.

8. Conclusion
Milestone 1 establishes the foundational requirements, project scope, stakeholder requirements, hardware validation, and risk analysis for the Zero-Trust Engine for WSN and IoT project. The core focus is to develop a repeatable and automated RF data-acquisition pipeline for capturing, labeling, and preprocessing raw I/Q signal data from Zigbee transmitters. Through comprehensive research into open-source datasets and Software-Defined Radio (SDR) hardware platforms (such as ADALM-PLUTO and HackRF One), the project has defined key functional priorities and risk mitigation strategies to ensure an effective experimental workflow. Phase 1 strictly isolates data acquisition, feature extraction, and dataset curation; all machine-learning model implementation, classification, and attack simulations are explicitly reserved for future Phase 2 research.


