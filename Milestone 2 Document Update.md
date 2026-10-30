# THOMPSON RIVERS UNIVERSITY

**Department of Computing Science**

**COMP 4910 – CAPSTONE PROJECT**

**Zero Trust Engine for WSN and IoT | COMP 4910 | Thompson Rivers University**

Magdala Nwuke (T00707082) | Kpughur-Tule Tertsegha Moses (T00721068)

Pranay Sethi (T00724613) | Ashima Bhasin (T00729543)

## 1. System Architecture

The system uses a local architecture centred on a Raspberry Pi gateway server, integrating known Zigbee transmitters, an ADALM-PLUTO software-defined radio (SDR), a Python backend, a browser-based dashboard, and local storage. The SDR captures Zigbee transmissions as raw in-phase and quadrature (I/Q) samples and transfers them to the Raspberry Pi through USB. Designated transmitters and controlled transmission schedules maintain accurate recording labels.

The Python backend manages acquisition settings, sessions, metadata, preprocessing, and preliminary feature extraction. The dashboard is hosted on the Raspberry Pi and accessed through a computer browser over the local network. It communicates with the backend to start or stop captures and display acquisition status, waveforms, metadata, and processing results.

A microSD card flashed with the Raspberry Pi operating system provides boot and local storage for the application, recordings, processed outputs, and metadata. Separate directories organize raw and processed files, while metadata links device identifiers, sessions, acquisition parameters, and file locations. Original recordings are preserved for repeated analysis. Card capacity and sustained write performance will be validated against the sampling rate, recording format, and capture schedule.

Scheduled acquisition, error logging, storage monitoring, and backups to a separate device support the intended two-to-four-day collection period, subject to hardware and storage validation. Saved configurations and organized datasets support reproducibility. All core services operate locally without cloud dependencies; device classification and automated security enforcement remain outside the current scope.

![Figure 1. System Architecture Diagram](Milestone%202%20Document%20Update_assets/figure-1-system-architecture.png)

*Figure 1. System Architecture Diagram*

## 2. Data Model

The proposed data model defines how information generated during radio-frequency (RF) data acquisition is organized, stored, and managed. The system associates captured in-phase and quadrature (I/Q) samples with known Zigbee transmitters while preserving relevant acquisition parameters, timestamps, and signal-processing results. The logical data model consists of five principal entities: Device, CaptureSession, Recording, CaptureMetadata, and ProcessedOutput.

The Device entity stores information identifying each known Zigbee transmitter, while the CaptureSession entity maintains details of individual data-collection sessions. The Recording entity links captured RF signals to their corresponding transmitters and collection sessions. The CaptureMetadata entity stores relevant acquisition parameters, including sampling rate, operating frequency, channel information, and receiver gain. Finally, the ProcessedOutput entity maintains references to the results generated through signal preprocessing and preliminary feature extraction.

Figure 2 Diagram Url: Diagram

![Figure 2. Proposed Entity Relationship Diagram for the RF Data Acquisition System](Milestone%202%20Document%20Update_assets/figure-2-entity-relationship.png)

*Figure 2. Proposed Entity Relationship Diagram for the RF Data Acquisition System*

The proposed storage architecture separates raw RF recordings from structured metadata. Raw I/Q samples and processed output files will be retained in an organized filesystem, while their corresponding identifiers, acquisition parameters, and file locations will be maintained in a structured metadata repository. This design supports dataset organization, reproducibility, and subsequent RF fingerprinting.

## 3. Interface Design

### 3.1 User-facing Interfaces

The main user-facing interface is the RF Data Collection Dashboard, which provides a centralized platform for monitoring Zigbee devices and managing RF signal captures. The dashboard displays each device's current status (connected, active, idle, or disconnected), its last recorded activity, and any potential communication failures. It also allows for selecting a Zigbee device, starting RF data collection, and visualizing the captured I/Q signal components.

![Figure 3. Wireframe of the Data Collection Dashboard](Milestone%202%20Document%20Update_assets/figure-3-dashboard-wireframe.png)

*Figure 3. Wireframe of the Data Collection Dashboard*

### 3.2 Navigation and Workflow

To begin data collection, select a Zigbee device from the dropdown menu in the RF Monitoring section and click the "Start Capture" button. The Raspberry Pi then initiates the data collection process using the ADALM-PLUTO SDR. Once the capture is completed, the recorded I/Q samples are saved and displayed as a waveform. Extracted statistical features and associated capture metadata can be reviewed in the “Capture Metadata” table. The Device Connection Monitoring section also allows for checking device activity and identifying potential communication failures throughout the process.

### 3.3 APIs and Communication Between Components

As shown in Figure 1, Zigbee devices transmit RF signals that are captured as raw I/Q samples by the ADALM-PLUTO SDR, while the coordinator receives Zigbee packet data. Both components communicate with the Raspberry Pi, which manages data collection, labeling, and feature extraction through Python scripts. The collected data is then stored in the RF dataset. The dashboard communicates with the Raspberry Pi's data collection software to initiate captures, retrieve device statuses, and display collected RF data.

### 3.4 How the Interface Supports Functional Requirements

The dashboard supports the system's functional requirements by providing an interface to monitor device connectivity, initiate RF data collection, and visualize captured I/Q signals. It also displays relevant metadata and extracted statistical features for RF fingerprint analysis. Connection alerts help identify communication failures, while the device selection feature supports controlled data collection from individual Zigbee devices.

## 4. Technology Selection and Justification

For this project, we chose technologies that are affordable, easy to work with, and compatible with the Raspberry Pi and ADALM-PLUTO SDR. Our goal is to build a system that can collect, process, and store RF signals locally without relying on cloud services.

### 4.1 Frontend Technologies

We plan to use HTML, CSS, and JavaScript to develop the RF Data Collection Dashboard. These technologies will allow users to select Zigbee devices, start or stop recordings, monitor device activity, and view captured I/Q signals. We chose them because they provide the features needed for our dashboard without requiring a complex framework.

### 4.2 Backend Technologies

We plan to use Python with Flask to connect the dashboard to the data collection system. The pyadi-iio library will allow Python to communicate with the ADALM-PLUTO SDR, while NumPy and SciPy will help process I/Q signals and extract statistical features such as variance, skewness, and kurtosis.

### 4.3 Database Technology

We plan to use SQLite to store device information, recording sessions, timestamps, capture settings, and file locations. The raw I/Q recordings and processed outputs will be stored separately in organized folders on the Raspberry Pi. SQLite was selected because it is lightweight and does not require a separate database server.

### 4.4 Hosting and Deployment Choices

The system will run locally on a Raspberry Pi using Raspberry Pi OS. Users will access the dashboard through a web browser on the same local network. The microSD card will store the operating system, application files, and recorded data, while backups will be kept on a separate device. Storage capacity and write performance will be tested before extended data collection.

### 4.5 Reasons for Choosing the Technologies

We selected these technologies because they work well together, are widely documented, and are suitable for our hardware and budget. Python provides useful libraries for signal processing, while Flask and SQLite help keep the system simple. The browser-based dashboard also allows users to access the system without installing additional software.

### 4.6 Alternative Technologies Considered and Why They Were Rejected

We considered React for the frontend, Node.js for the backend, MySQL and PostgreSQL for the database, and AWS for hosting. React would add complexity that our dashboard does not need. Node.js was less suitable because Python provides established scientific computing and SDR libraries. MySQL and PostgreSQL would require additional setup, while AWS would introduce cloud dependency and possible hosting costs.

### 4.7 How the Technology Choices Satisfy Client Constraints and Functional/Non-Functional Requirements

These technologies support the project's functional requirements by allowing users to manage RF recordings, monitor acquisition activity, view captured signals, and organize metadata. They also support non-functional requirements such as affordability, local operation, maintainability, and reproducibility. Keeping the original I/Q recordings and associated metadata will allow repeated analysis and support future RF fingerprinting research.

## 5. Licence Inventory

Our project mainly uses open-source technologies to reduce development costs. Table 1 lists the proposed software dependencies, their licences, and how they will be used.

**Table 1. Software Licence Inventory**

| Technology | Version | Licence | Purpose |
|---|---|---|---|
| Python | 3.13 | PSF | Backend programming |
| Flask | 3.1.3 | BSD-3-Clause | Dashboard communication |
| NumPy | 2.5.4 | BSD-3-Clause | I/Q data processing |
| SciPy | 1.18.1 | BSD-3-Clause | Statistical feature extraction |
| pyadi-iio | 0.0.21 | ADI BSD-style | SDR communication |
| libiio | 0.26 or 1.0 | LGPL-2.1-or-later* | Low-level SDR interface |
| SQLite | 3.46.1 | Public domain | Metadata storage |
| Raspberry Pi OS | Debian 13 (Trixie), 64-bit | Multiple open-source licences | Local hosting and storage |

**Licence Obligations and Risks:** Most of these technologies can be used without licensing fees, but we must follow their licence conditions, including preserving required copyright notices when redistributing software. Some libraries, including libiio, have additional conditions that depend on how they are distributed. We will check the final dependency versions and their licences before deployment. The current system does not require any external cloud APIs.
