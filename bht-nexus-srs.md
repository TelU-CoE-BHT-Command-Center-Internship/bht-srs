# Software Requirements Specification
## For {{project name}}

Version 0.1  
Prepared by {{author}}  
{{organization}}  
{{date_modified}}

## Table of Contents
<!-- TOC -->
* [1. Introduction](#1-introduction)
    * [1.1 Document Purpose](#11-document-purpose)
    * [1.2 Product Scope](#12-product-scope)
    * [1.3 Definitions, Acronyms, and Abbreviations](#13-definitions-acronyms-and-abbreviations)
    * [1.4 References](#14-references)
    * [1.5 Document Overview](#15-document-overview)
* [2. Product Overview](#2-product-overview)
    * [2.1 Product Perspective](#21-product-perspective)
    * [2.2 Product Functions](#22-product-functions)
    * [2.3 Product Constraints](#23-product-constraints)
    * [2.4 User Characteristics](#24-user-characteristics)
    * [2.5 Assumptions and Dependencies](#25-assumptions-and-dependencies)
    * [2.6 Apportioning of Requirements](#26-apportioning-of-requirements)
* [3. Requirements](#3-requirements)
    * [3.1 External Interfaces](#31-external-interfaces)
    * [3.2 Functional](#32-functional)
    * [3.3 Quality of Service](#33-quality-of-service)
    * [3.4 Compliance](#34-compliance)
    * [3.5 Design and Implementation](#35-design-and-implementation)
    * [3.6 AI/ML](#36-aiml)
* [4. Verification](#4-verification)
* [5. Appendixes](#5-appendixes)
<!-- TOC -->

## Revision History

| Name | Date | Reason For Changes | Version |
|------|------|--------------------|---------|
| Fa Ainama Caldera S   | 14 Juli 2026     | Pembuatan dokumen srs v1                    |  v1.0.0         |
|      |      |                    |         |

## 1. Introduction
<!-- overview of the SRS: purpose, scope, audience, and organization of the document; avoid detailed requirements -->

### 1.1 Document Purpose

Dokumen *Software Requirements Specification* (SRS) ini mendefinisikan kebutuhan perangkat lunak **BHT-Nexus**, yaitu aplikasi web internal yang dikembangkan untuk mendukung pengelolaan data, kegiatan, serta informasi strategis di lingkungan *Center of Excellence Biomedical and Healthcare Technologies* (CoE BHT). Dokumen ini disusun sebagai acuan utama dalam proses pengembangan perangkat lunak dan digunakan sebagai referensi bersama oleh seluruh stakeholder selama siklus pengembangan sistem.

SRS ini mendokumentasikan kebutuhan perangkat lunak secara terstruktur, meliputi ruang lingkup sistem, kebutuhan fungsional, kebutuhan nonfungsional, karakteristik pengguna, batasan sistem, serta informasi lain yang diperlukan untuk memastikan seluruh stakeholder memiliki pemahaman yang konsisten terhadap sistem yang akan dibangun. Dokumen ini juga menjadi dasar dalam proses perancangan, implementasi, pengujian, validasi, serta evaluasi perangkat lunak sehingga setiap tahapan pengembangan dapat mengacu pada kebutuhan yang telah disepakati.

Cakupan dokumen ini terbatas pada spesifikasi kebutuhan perangkat lunak BHT-Nexus dan tidak membahas secara rinci aspek implementasi, desain arsitektur, struktur basis data, maupun prosedur operasional sistem. Pembahasan mengenai aspek tersebut disajikan pada dokumen teknis lain yang disusun sesuai kebutuhan selama proses pengembangan.


### 1.2 Product Scope

BHT-Nexus merupakan aplikasi web internal yang dikembangkan untuk mendukung pengelolaan informasi, dokumentasi kegiatan, serta pemantauan aktivitas di lingkungan *Center of Excellence Biomedical and Healthcare Technologies* (CoE BHT). Sistem ini dirancang sebagai pusat pengelolaan informasi yang mengintegrasikan data dari berbagai aktivitas organisasi sehingga informasi dapat dikelola secara lebih terstruktur, terdokumentasi, dan mudah diakses oleh stakeholder yang berwenang.

Pengembangan BHT-Nexus dilatarbelakangi oleh kebutuhan akan sistem yang mampu mengatasi pengelolaan data dan informasi yang masih tersebar pada berbagai media, sehingga proses dokumentasi, pemantauan, serta penyusunan laporan memerlukan konsolidasi secara manual. Kondisi tersebut berpotensi menimbulkan inkonsistensi data, meningkatkan beban administrasi, dan menyulitkan penyediaan informasi yang akurat sebagai dasar pengambilan keputusan.

Melalui BHT-Nexus, CoE BHT diharapkan memiliki platform terintegrasi yang mendukung pengelolaan informasi organisasi secara konsisten, meningkatkan kualitas dokumentasi kegiatan, serta menyediakan informasi yang lebih mudah diakses untuk kebutuhan monitoring, evaluasi, dan pelaporan. Kehadiran sistem ini juga mendukung pengelolaan data yang lebih terstruktur sehingga proses operasional dapat dilaksanakan secara lebih efektif dan berkelanjutan.

Pengembangan BHT-Nexus sejalan dengan upaya CoE BHT dalam memperkuat tata kelola informasi organisasi melalui pemanfaatan teknologi informasi. Dengan menyediakan sumber informasi yang terpusat dan terdokumentasi dengan baik, sistem ini mendukung pengambilan keputusan yang berbasis data serta menjadi fondasi bagi pengelolaan aktivitas organisasi yang lebih transparan, terdokumentasi, dan berkesinambungan.


### 1.3 Intended Audience and Reading Suggestions

Dokumen *Software Requirements Specification* (SRS) ini ditujukan bagi seluruh stakeholder yang terlibat dalam proses analisis, pengembangan, pengujian, validasi, dan pengelolaan perangkat lunak BHT-Nexus. Setiap stakeholder memiliki kebutuhan informasi yang berbeda sesuai dengan peran dan tanggung jawabnya. Oleh karena itu, dokumen ini disusun secara terstruktur agar setiap pembaca dapat mengakses bagian yang paling relevan tanpa kehilangan konteks terhadap keseluruhan sistem.

Seluruh pembaca disarankan untuk memulai dengan membaca **Bab 1 Introduction** guna memahami tujuan penyusunan dokumen, ruang lingkup perangkat lunak, serta konteks pengembangan BHT-Nexus. Setelah memahami gambaran umum tersebut, pembaca dapat melanjutkan ke bagian lain sesuai dengan kebutuhan dan tanggung jawabnya selama siklus pengembangan perangkat lunak. Pendekatan ini bertujuan membangun pemahaman yang konsisten antarstakeholder serta meminimalkan perbedaan interpretasi terhadap kebutuhan sistem.

| Stakeholder                       | Tujuan Membaca Dokumen                                                                                                                     | Bagian yang Direkomendasikan                                                                                 |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------ |
| Project Supervisor                | Memastikan kebutuhan perangkat lunak telah sesuai dengan tujuan proyek dan kebutuhan organisasi.                                           | Seluruh dokumen, dengan fokus pada **Introduction**, **Overall Description**, dan **Specific Requirements**. |
| Project Manager / Koordinator Tim | Memahami ruang lingkup proyek, mengoordinasikan proses pengembangan, serta memastikan kebutuhan telah terdokumentasi secara lengkap.       | Seluruh dokumen.                                                                                             |
| System Analyst                    | Memverifikasi kelengkapan, konsistensi, dan keterlacakan kebutuhan perangkat lunak sebelum proses implementasi dimulai.                    | **Overall Description**, **Specific Requirements**, dan **Appendix** (apabila tersedia).                     |
| UI/UX Designer                    | Memahami karakteristik pengguna, kebutuhan sistem, dan batasan yang memengaruhi perancangan antarmuka pengguna.                            | **Overall Description** dan **Specific Requirements**.                                                       |
| Frontend Developer                | Menggunakan spesifikasi kebutuhan sebagai acuan dalam mengimplementasikan antarmuka dan interaksi pengguna sesuai dengan kebutuhan sistem. | **Specific Requirements** serta bagian yang berkaitan dengan antarmuka pengguna.                             |
| Backend Developer                 | Menggunakan spesifikasi kebutuhan sebagai acuan dalam mengimplementasikan logika bisnis, pengelolaan data, dan layanan sistem.             | **Overall Description** dan **Specific Requirements**.                                                       |
| Database Engineer                 | Memahami kebutuhan data serta batasan yang memengaruhi perancangan dan pengelolaan basis data.                                             | **Overall Description** dan **Specific Requirements** yang berkaitan dengan data.                            |
| Quality Assurance (QA)            | Menyusun skenario pengujian dan memvalidasi kesesuaian implementasi terhadap kebutuhan yang telah ditetapkan.                              | **Specific Requirements** dan kriteria yang berkaitan dengan kebutuhan sistem.                               |
| Technical Writer / Dokumentasi    | Menjaga konsistensi antara dokumentasi pengguna, dokumentasi teknis, dan kebutuhan perangkat lunak yang telah disepakati.                  | Seluruh dokumen sesuai kebutuhan dokumentasi.                                                                |
| Pengurus CoE BHT                  | Memastikan kebutuhan sistem telah merepresentasikan kebutuhan operasional organisasi dan menjadi dasar dalam proses validasi kebutuhan.    | **Introduction**, **Overall Description**, dan **Specific Requirements**.                                    |

### 1.4 Document Conventions

Dokumen *Software Requirements Specification* (SRS) ini disusun mengikuti struktur dan prinsip yang direkomendasikan oleh standar IEEE 29148 untuk memastikan informasi disajikan secara konsisten, mudah dipahami, dan dapat digunakan sebagai acuan bersama selama proses analisis, pengembangan, pengujian, dan validasi perangkat lunak. Seluruh isi dokumen menggunakan bahasa Indonesia yang mengacu pada Pedoman Umum Ejaan Bahasa Indonesia (PUEBI/ EYD Edisi V) dan Kamus Besar Bahasa Indonesia (KBBI) untuk menjaga kejelasan, konsistensi, dan ketepatan penggunaan istilah.

Konvensi penulisan diterapkan secara konsisten pada seluruh dokumen agar setiap stakeholder memiliki pemahaman yang sama terhadap struktur informasi dan interpretasi kebutuhan perangkat lunak. Selain meningkatkan keterbacaan dokumen, konvensi ini juga mendukung proses penelusuran kebutuhan (*requirement traceability*), pengelolaan perubahan dokumen, serta komunikasi antaranggota tim selama siklus pengembangan perangkat lunak.

| Elemen               | Konvensi                                                                                                                                                                                                  |
| -------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Struktur dokumen     | Dokumen disusun secara hierarkis mengikuti struktur IEEE 29148. Setiap bab dan subbab menggunakan sistem penomoran bertingkat untuk menunjukkan hubungan antarbagian dokumen.                             |
| Istilah              | Istilah yang digunakan memiliki makna yang konsisten di seluruh dokumen. Istilah teknis, nama sistem, atau istilah asing ditulis secara seragam untuk menghindari perbedaan interpretasi.                 |
| **Bold**             | Digunakan untuk menekankan istilah, nama bagian, atau informasi penting yang perlu memperoleh perhatian khusus dari pembaca.                                                                              |
| *Italic*             | Digunakan untuk istilah asing, nama dokumen, atau istilah yang pertama kali diperkenalkan sesuai dengan kaidah penulisan bahasa Indonesia.                                                                |
| Tabel                | Digunakan untuk menyajikan informasi yang bersifat terstruktur, seperti klasifikasi, perbandingan, atau daftar kebutuhan. Seluruh tabel diberikan nomor dan judul sebagai identitas referensi.            |
| Gambar               | Digunakan untuk mendukung penjelasan apabila representasi visual memberikan pemahaman yang lebih baik dibandingkan uraian tekstual. Seluruh gambar diberikan nomor dan judul sebagai identitas referensi. |
| Pernyataan kebutuhan | Setiap kebutuhan perangkat lunak ditulis sebagai satu pernyataan yang jelas, spesifik, tidak ambigu, serta dapat diverifikasi melalui proses implementasi maupun pengujian.                               |
| Penomoran kebutuhan  | Setiap kebutuhan diberikan identitas atau penomoran yang unik sesuai struktur dokumen untuk memudahkan penelusuran, pengelolaan perubahan, dan keterkaitan dengan proses pengembangan maupun pengujian.   |

Konvensi yang ditetapkan pada dokumen ini berlaku untuk seluruh bagian SRS dan menjadi pedoman dalam penyusunan setiap kebutuhan perangkat lunak. Dengan menerapkan konvensi yang konsisten, setiap stakeholder diharapkan dapat memahami isi dokumen secara seragam, mengurangi potensi perbedaan interpretasi, serta mendukung proses pengembangan perangkat lunak yang terdokumentasi dengan baik.

### 1.5 References

Dokumen *Software Requirements Specification* (SRS) ini disusun dengan mengacu pada standar, pedoman, dan referensi resmi yang mendukung penyusunan spesifikasi kebutuhan perangkat lunak. Referensi berikut digunakan sebagai dasar dalam penyusunan struktur dokumen, penerapan konvensi penulisan, serta penggunaan istilah yang konsisten selama proses dokumentasi.

**[1]** ISO/IEC/IEEE. *ISO/IEC/IEEE 29148:2018 Systems and Software Engineering—Life Cycle Processes—Requirements Engineering*. First Edition, ISO/IEC/IEEE, 2018.

**[2]** IEEE Computer Society. *IEEE Recommended Practice for Software Requirements Specifications (IEEE Std 830-1998)*. IEEE, 1998. *(Digunakan sebagai referensi historis dan pelengkap terhadap IEEE 29148.)*

**[3]** Object Management Group (OMG). *Unified Modeling Language (UML) Specification*, Version 2.5.1. Object Management Group, 2017.

**[4]** Badan Pengembangan dan Pembinaan Bahasa. *Pedoman Umum Ejaan Bahasa Indonesia (PUEBI/ EYD Edisi V)*. Kementerian Pendidikan, Kebudayaan, Riset, dan Teknologi, Republik Indonesia.

**[5]** Badan Pengembangan dan Pembinaan Bahasa. *Kamus Besar Bahasa Indonesia (KBBI) Daring*. Kementerian Pendidikan, Kebudayaan, Riset, dan Teknologi, Republik Indonesia.


## 2. Product Overview
<!-- background and context that shape the product's requirements -->

### 2.1 Product Perspective
<!-- context of the system: a new product, a replacement, or part of a family; note relationships to other systems -->

### 2.2 Product Functions
<!-- major functional areas or features the product provides in 5–10 concise bullets -->

### 2.3 Product Constraints
<!-- design and implementation constraints that affect the solution -->

### 2.4 User Characteristics
<!-- classes, roles, expertise, access levels, frequency of use, and accessibility or localization needs -->

### 2.5 Assumptions and Dependencies
<!-- assumptions about environment, third-party services, usage patterns, and other external factors; note potential impact/risk. -->

### 2.6 Apportioning of Requirements
<!-- map major requirements to subsystems, services, or releases/iterations -->

## 3. Requirements
<!-- identifiable, verifiable, testable requirements; avoid implementation details -->

### 3.1 External Interfaces
<!-- inputs/outputs (formats, protocols, timing, etc); reference interface schemas where available. -->

#### 3.1.1 User Interfaces
<!-- user interactions (UI elements, dialogs, flows); reference design/style guides -->

#### 3.1.2 Hardware Interfaces
<!-- interactions with physical devices (types, signals, etc) -->

#### 3.1.3 Software Interfaces
<!-- integrations with other systems (APIs, contracts, owner, etc) -->

### 3.2 Functions
<!-- externally observable behaviors organized by feature/use case -->

### 3.3 Quality of Service
<!-- measurable non-functional attributes section -->

#### 3.3.1 Performance
<!-- time (latency, throughput, etc.) and space (memory, storage, bandwidth, etc.) -->

#### 3.3.2 Security
<!-- protection of data, identities, and operations (transit/rest, auth, encryption, etc); safety, confidentiality, privacy, integrity, and availability -->

#### 3.3.3 Reliability
<!-- ability to consistently perform as specified (MTBF, redundancy/failover, caches, etc) -->

#### 3.3.4 Availability
<!-- readiness to deliver service (target SLAs, maintenance windows, recovery/restore, etc) -->

#### 3.3.5 Observability
<!--  logs, metrics, traces, alerting and dashboards -->

### 3.4 Compliance
<!-- laws, standards, contracts, or policies; cite the authority and verifiable criteria. -->

### 3.5 Design and Implementation
<!-- constraints and mandates on design, deployment, and maintenance section -->

#### 3.5.1 Installation
<!-- ensure software runs smoothly in its target environments (supported platforms, prerequisites, configuration, etc) -->

#### 3.5.2 Build and Delivery
<!-- controls for building and delivering (dependency management, automation, integrity/traceability, etc) -->

#### 3.5.3 Distribution
<!-- distributed deployments, data, and devices (topologies, replication/placement, etc) -->

#### 3.5.4 Maintainability
<!-- measurable attributes that make the software easier to modify, fix, and evolve (modularity, standards, documentation, observability, etc) -->

#### 3.5.5 Reusability
<!-- components intended for reuse -->

#### 3.5.6 Portability
<!-- ability to run on multiple environments (supported OSs/runtimes, cloud providers, etc) -->

#### 3.5.7 Cost
<!-- targets/budgets that influence design or implementation (cloud spend, per-transaction, licensing, etc) -->

#### 3.5.8 Deadline
<!-- milestones, delivery dates, and readiness criteria -->

#### 3.5.9 Proof of Concept
<!-- objectives, scope, timebox, and success criteria for any POC -->

#### 3.5.10 Change Management
<!-- how changes are introduced and communicated (categories, required artifacts and workflow, etc) -->

### 3.6 AI/ML
<!-- ML-specific requirements section -->

#### 3.6.1 Model Specification
<!-- model purpose, inputs/outputs, performance targets, validation data, versioning -->

#### 3.6.2 Data Management
<!-- lifecycle of datasets (origin, labeling, anonymization, etc) -->

#### 3.6.3 Guardrails
<!-- controls that the system operates within approved boundaries (validation/sanitation, output filtering, action limits, etc) -->

#### 3.6.4 Ethics
<!-- fairness, transparency, and accountability metrics/enforcement -->

#### 3.6.5 Human-in-the-Loop
<!-- human oversight (review points, escalations, feedback, etc) -->

#### 3.6.6 Model Lifecycle and Operations
<!-- deployment, monitoring, retraining, and retiring -->

## 4. Verification

| Requirement ID | Verification Method | Test/Artifact Link | Status | Evidence |
|----------------|---------------------|--------------------|--------|----------|
|                |                     |                    |        |          |
|                |                     |                    |        |          |

## 5. Appendixes