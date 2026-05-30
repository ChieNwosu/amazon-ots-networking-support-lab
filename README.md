# Amazon OTS Networking and Support Lab

A documentation-first IT support and networking learning lab aligned with Amazon IT Support Engineer I, Ops Tech Solutions (OTS) role preparation.

---

## Purpose

This repository documents structured technical preparation for an Amazon IT Support Engineer I, Ops Tech Solutions (OTS) role. It serves as a learning lab where coursework, troubleshooting practice, and professional documentation converge to demonstrate role-aligned technical growth.

The focus areas reflect core OTS responsibilities: network troubleshooting, endpoint support, Linux system administration, cybersecurity awareness, uptime and availability, root-cause analysis, and technical communication. All content is organized to support incremental learning and artifact collection as Coursera specializations are completed.

This is not a portfolio of production IT work. It is an in-progress technical learning lab built by an Information Technology student and Amazon Fulfillment Center associate preparing to transition into an OTS engineering role. The repository demonstrates structured self-directed learning, professional documentation practice, and deliberate skill development.

---

## Role Alignment

Amazon IT Support Engineer I (OTS) responsibilities include:

- Troubleshooting network connectivity issues in fulfillment center environments
- Supporting PC, endpoint, and peripheral devices
- Maintaining uptime and availability of IT infrastructure
- Performing root-cause analysis on hardware and software failures
- Writing and following SOPs and runbooks
- Communicating technical findings to operations and engineering teams

This repository maps directly to these responsibilities through coursework, troubleshooting runbooks, and role-alignment documentation. See [`docs/role-alignment/amazon-ots-role-map.md`](docs/role-alignment/amazon-ots-role-map.md) for the full mapping.

---

## Learning Roadmap

| Phase | Focus | Courses | Status |
|-------|-------|---------|--------|
| 1 | Networking Fundamentals | Cisco Network Fundamentals, Cisco Solutions and Networking Fundamentals | In Progress |
| 2 | Advanced Networking | Cisco CCNA (200-301), Cisco Data Center Foundations | In Progress |
| 3 | Systems and Security | Linux and Private Cloud Administration, Cybersecurity Operations Fundamentals | In Progress |
| 4 | Automation and Integration | Network Automation Engineering Fundamentals | Not Started |

See [`docs/role-alignment/learning-roadmap.md`](docs/role-alignment/learning-roadmap.md) for detailed phase descriptions and sequencing rationale.

---

## Repository Structure

```
amazon-ots-networking-support-lab/
├── README.md
├── docs/
│   ├── foundational-it/
│   │   ├── freecodecamp-it-fundamentals-summary.md
│   │   ├── it-support-glossary.md
│   │   └── it-fundamentals-to-ots-map.md
│   ├── role-alignment/
│   │   ├── amazon-ots-role-map.md
│   │   ├── learning-roadmap.md
│   │   └── resume-project-summary.md
│   ├── learning-workflow/
│   │   ├── obsidian-notebooklm-kiro-workflow.md
│   │   └── public-release-framework.md
│   ├── troubleshooting-runbooks/
│   │   ├── no-network-connectivity.md
│   │   ├── dhcp-failure.md
│   │   ├── dns-resolution-issue.md
│   │   ├── dns-resolution-diagnostics.md
│   │   ├── workstation-endpoint-support.md
│   │   ├── device-offline-troubleshooting.md
│   │   └── osi-layered-diagnostics.md
│   └── reflections/
│       ├── amazon-operations-to-ots.md
│       ├── technical-growth-log.md
│       ├── week-01-ots-learning-log.md
│       └── week-02-ots-learning-log.md
├── courses/
│   ├── cisco-network-fundamentals/
│   │   └── notes/
│   │       └── subnet-design-lab.md
│   ├── cisco-solutions-networking-fundamentals/
│   ├── packt-ccna-200-301/
│   ├── linux-private-cloud-administration/
│   ├── cybersecurity-operations-fundamentals/
│   ├── cisco-data-center-foundations/
│   └── network-automation-engineering-fundamentals/
├── templates/
│   ├── course-note-template.md
│   ├── troubleshooting-runbook-template.md
│   ├── certificate-entry-template.md
│   ├── screenshot-log-template.md
│   └── weekly-learning-log-template.md
└── assets/
    ├── images/
    ├── certificates/
    └── screenshots/
```

Each course folder contains a README, plus `notes/`, `screenshots/`, and `certificate/` subfolders for organizing learning artifacts.

---

## Current Coursera Programs

| Program | Provider | Platform | Link | Status |
|---------|----------|----------|------|--------|
| Network Fundamentals Specialization | Cisco | Coursera | [Link](https://www.coursera.org/specializations/network-fundamentals) | In Progress |
| Cisco Solutions and Networking Fundamentals | Packt | Coursera | [Link](https://www.coursera.org/specializations/packt-cisco-solutions-and-networking-fundamentals) | In Progress |
| Cisco CCNA (200-301) Specialization | Packt | Coursera | [Link](https://www.coursera.org/specializations/packt-cisco-ccna-200-301) | In Progress |
| Linux and Private Cloud Administration on IBM Power Systems | IBM / Red Hat | Coursera | [Link](https://www.coursera.org/specializations/linux-private-cloud-administration-power-systems) | In Progress |
| Cybersecurity Operations Fundamentals | Cisco | Coursera | [Link](https://www.coursera.org/specializations/cbrops) | In Progress |
| Understanding Cisco Data Center Foundations | Cisco | Coursera | [Link](https://www.coursera.org/specializations/cisco-datacenter-foundations) | Not Started |
| Network Automation Engineering Fundamentals | Cisco | Coursera | [Link](https://www.coursera.org/specializations/networkautomation) | Not Started |

---

## Supplemental Learning Sources

In addition to the Coursera specializations listed below, I used [FreeCodeCamp's Information Technology course](https://www.freecodecamp.org/) as supplemental self-paced learning to strengthen foundational IT concepts. This is not a formal certification, but an additional study material that helped build a broader mental model of hardware, networking, cloud, and security before diving into protocol-level coursework.

Related documentation:
- [`docs/foundational-it/freecodecamp-it-fundamentals-summary.md`](docs/foundational-it/freecodecamp-it-fundamentals-summary.md) — Summary of learning areas and OTS relevance
- [`docs/foundational-it/it-support-glossary.md`](docs/foundational-it/it-support-glossary.md) — Beginner-friendly IT glossary with FC/OTS examples
- [`docs/foundational-it/it-fundamentals-to-ots-map.md`](docs/foundational-it/it-fundamentals-to-ots-map.md) — Role-alignment table connecting IT topics to OTS responsibilities
- [`courses/cisco-network-fundamentals/notes/subnet-design-lab.md`](courses/cisco-network-fundamentals/notes/subnet-design-lab.md) — Hands-on subnetting exercise designing FC network segments

---

## Troubleshooting Runbooks

Structured diagnostic guides built as learning exercises for OTS role preparation:

- [`osi-layered-diagnostics.md`](docs/troubleshooting-runbooks/osi-layered-diagnostics.md) — OSI model-based structured diagnostics with FC-style scenarios and escalation criteria
- [`dns-resolution-diagnostics.md`](docs/troubleshooting-runbooks/dns-resolution-diagnostics.md) — DNS troubleshooting with command references, worked scenarios, and escalation guidance
- [`no-network-connectivity.md`](docs/troubleshooting-runbooks/no-network-connectivity.md) — General network connectivity troubleshooting
- [`dhcp-failure.md`](docs/troubleshooting-runbooks/dhcp-failure.md) — DHCP assignment failure diagnostics
- [`dns-resolution-issue.md`](docs/troubleshooting-runbooks/dns-resolution-issue.md) — DNS resolution troubleshooting
- [`workstation-endpoint-support.md`](docs/troubleshooting-runbooks/workstation-endpoint-support.md) — Workstation and endpoint triage
- [`device-offline-troubleshooting.md`](docs/troubleshooting-runbooks/device-offline-troubleshooting.md) — Device offline diagnostics

---

## Technical Focus Areas

- Network troubleshooting (Layer 1 through Layer 3 diagnostics)
- TCP/IP protocol suite and OSI model application
- IP addressing, subnetting, and VLSM
- Switching fundamentals (VLANs, STP, EtherChannel)
- Routing concepts (static, OSPF, EIGRP)
- IP services (DHCP, DNS, NAT, NTP)
- Linux command-line administration
- System monitoring and log analysis
- Cybersecurity operations and endpoint protection
- Data center infrastructure concepts
- Network automation and programmability awareness
- Structured troubleshooting methodology
- SOP and runbook documentation
- Technical communication and incident reporting

---

## Learning Workflow

This repository publishes only original, polished artifacts. Raw course content, transcripts, quiz answers, and copied lesson text are never committed. The workflow that produces public-facing documentation follows four stages:

1. **Obsidian Web Clipper** — Private raw capture and personal study notes (never published)
2. **NotebookLM** — Source understanding, concept synthesis, and self-testing
3. **ChatGPT** — Transformation of rough notes into polished summaries, runbooks, and professional language
4. **Kiro** — Repository management, formatting, and Git operations

All published content represents original summaries, applied troubleshooting notes, personal lab outputs, certificate proof, and learning reflections. See [`docs/learning-workflow/obsidian-notebooklm-kiro-workflow.md`](docs/learning-workflow/obsidian-notebooklm-kiro-workflow.md) for the full workflow description and content ethics boundaries.

---

## Project Summary

> A documentation-first technical learning lab demonstrating structured preparation for an Amazon IT Support Engineer I, Ops Tech Solutions (OTS) role. Developed professional troubleshooting runbooks, OTS role-alignment documentation, and organized coursework artifacts across seven Coursera specializations covering Cisco networking, Linux administration, cybersecurity operations, and network automation. Connects Amazon Fulfillment Center operations awareness with technical skill development in network diagnostics, endpoint support, and SOP authoring.

See [`docs/role-alignment/resume-project-summary.md`](docs/role-alignment/resume-project-summary.md) for additional formats.

---

## Author

**Chiemela Joseph Nwosu**  
Information Technology Student (Data Analytics Concentration)  
North Carolina Central University

Currently employed at an Amazon Fulfillment Center. Preparing for transition to IT Support Engineer I, Ops Tech Solutions through structured coursework and technical documentation practice.

---

## Disclaimer

This repository represents an active technical learning lab. Content reflects coursework exercises, self-study, and structured preparation. It does not represent production IT support experience or professional consulting work. All troubleshooting runbooks are study exercises developed from course material, not documentation of real incidents.
