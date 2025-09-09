# Project Phoenix - Comprehensive Project Summary and Understanding
## Owens Corning Doors Division S4/HANA Migration

---

## Document Information
**Created:** 2025-09-07  
**Purpose:** Consolidated understanding of Project Phoenix requirements, risks, and proposal strategy  
**Status:** Active analysis and proposal development  

---

## 1. PROJECT OVERVIEW

### 1.1 Executive Summary
Project Phoenix is Owens Corning's initiative to migrate their Doors Division (formerly Masonite facilities) from legacy systems to SAP S4/HANA. This represents a critical digital transformation affecting four manufacturing sites with aggressive timelines and significant technical uncertainties.

### 1.2 Key Project Parameters
- **Client:** Owens Corning - Doors Division
- **Sites:** 4 intial manufacturing facilities, 30 additional
  - Sacopan, QC (Canada)
  - Laurel, MS
  - Wahpeton, ND
  - Verdi, NV
- **Critical Dates:**
  - Integration Testing Deadline: December 19, 2025
  - Production Go-Live: February 6, 2026
- **Platform:** Migration to SAP S4/HANA with Ignition SCADA/MES integration

### 1.3 High-Level Scope
The project encompasses four major integration areas:
1. **SAP S4/HANA Integration** - Production order management and confirmations
2. **Response File Generation** - Equipment-specific production instructions
3. **Automatic Confirmations** - Real-time production counting and reporting
4. **Driver Check-in System** - Truck scale integration at Laurel facility

---

## 2. CRITICAL RISKS AND UNCERTAINTIES

### 2.1 Risk Summary Matrix
| Risk Area | Severity | Impact on Scope | Key Concern |
|-----------|----------|-----------------|-------------|
| Ignition 8.3 Event Streams | CRITICAL | 3-5x | Unproven bleeding-edge technology |
| APIM/Kafka Architecture | CRITICAL | 2-3x | External dependencies, unclear boundaries |
| Response File Format | HIGH | 5x | Unknown format, potential plant variability |
| PLC Integration Status | HIGH | 12x | Unknown if devices already integrated |
| PI System Role | MEDIUM | 2x | Integration direction undefined |
| Driver Check-in Model | MEDIUM | 8x | Supervision model impacts architecture |

### 2.2 Scope Variability Analysis
Based on current unknowns, the project scope could vary by **300-500%** depending on:
- Whether field devices are already integrated into existing systems
- Response file standardization across plants (1 format vs 5 different formats)
- Driver check-in supervision model (attended vs unattended)
- Fallback requirements if Kafka/APIM infrastructure is delayed

### 2.3 Technology Stack Concerns
**Ignition 8.3 with Event Streams Module:**
- Released Q4 2025 - extremely new technology
- No known production deployments at scale
- Critical dependency for Kafka integration
- No proven fallback strategy identified

**Azure APIM + Kafka Architecture:**
- Multiple team dependencies (OC IT, External teams, Grantek)
- Unclear responsibility boundaries (RACI matrix undefined)
- Complex error handling across 4 API calls per transaction
- "Clean Core" principle pushes all error handling to Ignition

---

## 3. TECHNICAL ARCHITECTURE UNDERSTANDING

### 3.1 Integration Architecture
The proposed architecture involves a complex multi-tier integration:

```
S4/HANA Cloud ←→ Azure APIM ←→ Kafka ←→ World HQ Ignition ←→ Site Ignition Systems
```

**Key Architectural Components:**
- **S4/HANA:** Core ERP system (cloud-based)
- **Azure APIM:** API management and orchestration layer
- **Kafka:** Event streaming platform for message distribution
- **World HQ Backend:** Central Ignition instance for message routing
- **Site MES Systems:** Local Ignition instances at each facility
- **PI System:** Existing historian (role TBD)

### 3.2 Message Flow Pattern
**Downstream (S4 → Plants):**
1. Production orders published from S4/HANA
2. Routed through APIM to Kafka topics
3. World HQ Ignition consumes from Kafka
4. Messages distributed to site databases
5. Site gateways process local messages

**Upstream (Plants → S4):**
1. Production confirmations generated at sites
2. Written to site outbound tables
3. World HQ polls and aggregates
4. Posts to Kafka/APIM
5. Four-step SAP confirmation process

### 3.3 Four-Step Confirmation Process
Each production confirmation requires:
1. Production Order Confirmation
2. Handling Unit (HU) Creation
3. Pack HU onto Inbound Delivery
4. Post Goods Receipt

**Critical Issue:** All error handling must be managed in Ignition despite limited visibility into APIM↔SAP failures.

---

## 4. FUNCTIONAL REQUIREMENTS ANALYSIS

### 4.1 Response File System
**Current Understanding:**
- Legacy system generates files for production equipment
- References "Matcom language" (undefined legacy format)
- File format unknown and potentially varies by plant/equipment
- Critical for equipment operation (trim saws, printers, etc.)

**Major Uncertainties:**
- Exact file format(s) - could be fixed-width, delimited, or proprietary
- Standardization across plants - may require 4-5 different parsers
- Equipment compatibility requirements
- Update frequency and delivery mechanism

### 4.2 Automatic Confirmations
**Requirement:** Automatic production counting from equipment sensors

**Critical Unknown:** Are counting devices already integrated?
- **Best Case:** PLCs already in PI/SCADA (low effort)
- **Worst Case:** New PLC integration required (12x effort increase)
- **Likely:** Mixed integration status across sites

**Equipment Types Mentioned:**
- Dropout sensors (Brownboard, Fiberglass)
- Laser eyes (Cut Coat)
- CIM PLCs (SMC Bin)
- Various forming sensors

### 4.3 PI System Integration
**Fundamental Issue:** Integration purpose and direction undefined

**Possible Scenarios:**
1. Ignition reads from PI (consume existing data)
2. Ignition writes to PI (maintain as system of record)
3. Bidirectional integration (complex synchronization)
4. PI replacement (migration strategy required)

**No clarity on:**
- Why PI integration is needed
- Current PI infrastructure and version
- Regulatory/compliance requirements
- Downstream system dependencies

### 4.4 Driver Check-in System (Laurel Only)
**Requirement:** Truck scale integration with driver check-in

**Major Design Decision Required:**
- **Attended:** Traditional scale house with operator ($30K)
- **Semi-Attended:** Remote supervision via video ($150K)
- **Unattended:** Full self-service kiosk system ($350K)

**Technical Requirements:**
- Load cell/scale integration (manufacturer unknown)
- Network infrastructure at truck scales
- Weather-resistant equipment if outdoor
- Failure handling and manual fallback

---

## 5. INFRASTRUCTURE AND DEPLOYMENT

### 5.1 Owens Corning Advanced Manufacturing Stack (AMS)
- Sites expected to follow OC AMS architecture
- Includes Site Critical Servers and Site MES Servers
- Network segmentation and security zones required
- Unclear division of infrastructure responsibilities

### 5.2 Current Site Infrastructure
**Unknown Status:**
- Existing Ignition versions (if any) at Doors sites
- Migration path from legacy systems
- Coexistence requirements during transition
- Historical data migration needs

### 5.3 Infrastructure Responsibilities
**Needs Clarification:**
- Who provides Ignition servers?
- Who manages Kafka infrastructure?
- Network configuration ownership?
- Development/test environment provisioning?
- Backup and disaster recovery?

---

## 6. PROPOSAL STRATEGY

### 6.1 Recommended Approach
Given the high uncertainty and aggressive timeline, we recommend:

**Phase 1: Discovery and Risk Reduction (4-6 weeks)**
- Fixed-price assessment phase
- Document all file formats and integration points
- Assess current infrastructure
- Define clear RACI matrix
- Validate technology choices

**Phase 2: Dual-Path Architecture Development**
- Implement REST API integration (proven technology)
- Prepare Kafka integration (when stable)
- Build with fallback capability
- Incremental testing approach

**Phase 3: Phased Deployment**
- Start with single pilot site
- Manual processes as backup
- Gradual automation increase
- Full rollout only after validation

### 6.2 Commercial Structure
**Recommended Pricing Model:**
1. **Discovery:** Fixed price
2. **Design:** Time & Materials (based on discoveries)
3. **Implementation:** Fixed price (after design complete)
4. **Support:** Separate maintenance agreement

**Estimated Effort Range:** 900-5,400 hours depending on RFI responses

### 6.3 Critical Success Factors
1. Complete RFI responses before commitment
2. Stable Ignition 8.3 and Event Streams module
3. APIM/Kafka infrastructure ready by October 2025
4. Clear escalation path for cross-team issues
5. Flexible go/no-go decision points

---

## 7. KEY QUESTIONS REQUIRING ANSWERS

### 7.1 Immediate Clarifications Needed
1. **Response File Formats:** Provide samples from all plants
2. **PLC Integration Status:** Document what's already connected
3. **PI System Purpose:** Define integration requirements
4. **Driver Check-in Model:** Choose supervision approach
5. **Infrastructure Ownership:** Clear RACI matrix

### 7.2 Technology Decisions Required
1. Kafka+APIM only vs REST API fallback vs Both
2. PI system role (read/write/replace)
3. Response file standardization approach
4. Confirmation timing (real-time vs batch)
5. Error handling strategy for "Clean Core"

### 7.3 Risk Acceptance Required
1. Ignition 8.3 production readiness
2. External team dependency management
3. Aggressive timeline feasibility
4. Scope variability tolerance
5. Fallback strategy approval

---

## 8. NEXT STEPS

### 8.1 Immediate Actions
1. Submit comprehensive RFI for client response
2. Schedule technical deep-dive sessions
3. Request access to sample files and systems
4. Establish communication with external teams
5. Begin risk mitigation planning

### 8.2 Conditional Planning
- **If RFI responses favorable:** Proceed with full proposal
- **If significant unknowns remain:** Mandate discovery phase
- **If timeline unrealistic:** Propose phased go-live
- **If technology unstable:** Implement proven alternatives

### 8.3 Documentation Requirements
- Complete RFI responses
- Network architecture diagrams
- Sample response files (5+ per plant)
- Current process workflows
- Infrastructure assessments
- Decision authority confirmations

---

## 9. REFERENCE DOCUMENTATION

### 9.1 Source Documents Analyzed
- Project_Phoenix_-_Ignition_RFQ_Supplemental.docx (Reference folder)
- Comprehensive_Project_Phoenix_RFI.md
- Multiple detailed RFI sections covering:
  - SAP Integration Architecture
  - PI System Integration
  - Response File Format Analysis
  - Automatic Confirmations Scope
  - Driver Check-in System Analysis
- SAP_S4HANA_Integration_Proposal.md (In progress)

### 9.2 Similar Project References
- Various Grantek projects referenced in Reference folder:
  - J&J MW Line Inspections
  - Mondelez IDF Integration
  - KHC Pallet Manifest
  - Conagra Label Verification
  - Tyson Pallet Manifest projects

### 9.3 Key Technical Standards
- Owens Corning Advanced Manufacturing Stack (AMS)
- SAP S4/HANA "Clean Core" principle
- Ignition 8.3 with Event Streams module
- Azure APIM for API management
- Kafka for event streaming

---

## 10. RISK MITIGATION SUMMARY

### 10.1 Technical Risks
- **Primary Mitigation:** Dual-path architecture with proven fallbacks
- **Secondary Mitigation:** Phased deployment with pilot site
- **Tertiary Mitigation:** Manual processes as emergency backup

### 10.2 Schedule Risks
- **Primary Mitigation:** Parallel development paths
- **Secondary Mitigation:** Reduced initial scope (MVP approach)
- **Tertiary Mitigation:** Phased go-live by site

### 10.3 Commercial Risks
- **Primary Mitigation:** Discovery phase before fixed pricing
- **Secondary Mitigation:** Clear change management process
- **Tertiary Mitigation:** Defined scope boundaries and assumptions

---

## DOCUMENT MAINTENANCE

This document will be updated as new information becomes available through:
- RFI responses from Owens Corning
- Technical discovery sessions
- Architecture decisions
- Risk assessments
- Proposal development progress

**Last Updated:** 2025-09-09  
**Next Review:** Upon RFI response receipt  
**Document Owner:** Grantek Proposal Team

---

*END OF SUMMARY DOCUMENT*