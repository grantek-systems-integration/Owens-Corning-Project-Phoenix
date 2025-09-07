# REQUEST FOR INFORMATION (RFI)
# PROJECT PHOENIX - DOORS DIVISION S4/HANA MIGRATION
## Comprehensive Technical Requirements & Risk Assessment

---

### DOCUMENT INFORMATION
**RFI Number:** RFI-2025-PHOENIX-001  
**Date Issued:** [Date]  
**Response Due Date:** [Date + 14 days]  
**Project Name:** Project Phoenix - Doors Conversion to S4  
**Client:** Owens Corning  
**Prepared By:** [Your Company Name]  

### KEY DATES
**Integration Testing Deadline:** December 19, 2025  
**Production Go-Live:** February 6, 2026  
**Initial Sites:** Sacopan QC, Laurel MS, Wahpeton ND, Verdi NV  

---

## EXECUTIVE SUMMARY

### Purpose
This RFI seeks to clarify critical technical requirements and identify significant risks for the Project Phoenix implementation. Our analysis has revealed several areas where the current requirements contain assumptions that could dramatically impact project scope, timeline, and cost.

### Critical Risk Summary
Based on our preliminary analysis, we have identified the following critical risks that require immediate clarification:

| Risk Area | Impact | Key Concern |
|-----------|--------|-------------|
| **Ignition 8.3 Dependency** | CRITICAL | Event Streams module is unproven in production |
| **APIM/Kafka Architecture** | CRITICAL | External team dependencies and unclear boundaries |
| **Response File Format** | HIGH | Unknown format, potential plant variability |
| **PLC Integration** | HIGH | Existing integration status unknown |
| **PI System Role** | MEDIUM | Integration direction and purpose undefined |
| **Driver Check-in** | MEDIUM | Infrastructure and supervision model unclear |

### Scope Variability
Depending on the answers to this RFI, the project scope could vary by **300-500%**. Key variables include:
- Whether field devices are already integrated (Unknown)
- Response file standardization across plants (1x vs 5x complexity)
- Driver check-in supervision model
- Fallback strategies if Kafka/APIM delayed

### Recommendation
We strongly recommend a **Discovery Phase** before committing to fixed scope and timeline.

---

## SECTION 1: SAP S4/HANA INTEGRATION

### 1.1 Architecture Clarification

#### Kafka Event Streams Dependency
**CRITICAL: Ignition 8.3 with Event Streams module is bleeding-edge technology**

- [ ] **Has Owens Corning validated Event Streams in ANY production environment?**
- [ ] **What is the fallback strategy if Event Streams exhibits instability?**
- [ ] **Is OC mandating Ignition 8.3 despite limited field deployment history?**
- [ ] **Will OC accept liability for Event Streams module issues?**

#### APIM Orchestration & Boundaries
**CRITICAL: Multiple teams with unclear responsibilities**

**We need a clear RACI matrix for:**
| Component | OC Team | External Team | Grantek | Currently Undefined |
|-----------|---------|---------------|---------|---------------------|
| APIM Configuration | ? | ? | NO | ✓ |
| Kafka Topic Creation | ? | ? | ? | ✓ |
| Message Schema Definition | ? | ? | ? | ✓ |
| Error Handling | ? | ? | YES? | ✓ |
| APIM↔SAP Failures | ? | ? | ? | ✓ |

**Specific Questions:**
- [ ] Who owns APIM configuration and Kafka endpoint exposure?
- [ ] When APIM cannot reach SAP, how is this communicated to Ignition?
- [ ] Who monitors and handles APIM↔SAP handshake failures?
- [ ] What is Datasphere's role in the transaction chain?

#### Error Handling Complexity
**The "Clean Core" principle pushes all error handling to Ignition, but:**

- [ ] How does Ignition detect APIM↔SAP failures it cannot see?
- [ ] Who provides the complete error catalog for all layers?
- [ ] How are partial transactions rolled back across 4 API calls?
- [ ] What happens when Ignition posts to Kafka but APIM fails?

### 1.2 Integration Approach

#### Primary Architecture Decision Required
**Choose ONE before proceeding:**

- [ ] **Option A:** Kafka+APIM only (high risk, unproven)
- [ ] **Option B:** Direct REST APIs as fallback (moderate risk)
- [ ] **Option C:** Both paths in parallel (low risk, higher cost)

**If Kafka/APIM is delayed past October 2025, what is the fallback plan?**

#### Performance & Scalability
For each site, please provide:
- [ ] Production orders per day (average/peak)
- [ ] Expected Kafka message volume (msgs/second)
- [ ] API rate limits from SAP
- [ ] Acceptable latency requirements
- [ ] WAN/Cloud outage frequency and duration

### 1.3 Existing Infrastructure

#### Masonite/Doors Division Assessment
- [ ] What version of Ignition (if any) exists at the four sites?
- [ ] Do these sites follow OC's Advanced Manufacturing Stack?
- [ ] What infrastructure is Grantek responsible for providing?
- [ ] What infrastructure will OC provide?

---

## SECTION 2: AUTOMATION INTEGRATION

### 2.1 OSI PI System Integration

#### Fundamental Question: What IS the PI Integration?
**The RFQ states "Pi system will be integrated" without defining:**

- [ ] **Direction:** Is Ignition reading FROM Pi, writing TO Pi, or BOTH?
- [ ] **Purpose:** Why integrate? Compliance? Reporting? Operations?
- [ ] **Architecture:** Will Pi remain system of record or be replaced?
- [ ] **Regulatory:** Is Pi validated for compliance (cannot be replaced)?

#### PI Infrastructure Assessment
Please provide:
- [ ] PI System version and architecture at each site
- [ ] Whether PI Asset Framework (AF) is implemented
- [ ] Current OPC server capabilities (DA/UA/HDA)
- [ ] List of systems dependent on PI data
- [ ] Whether Grantek should traverse AF or access points directly

### 2.2 Response File System

#### Critical: Unknown File Format
**The RFQ mentions "Response Files" but provides no specification:**

- [ ] **What is the EXACT file format?** (Provide multiple samples per plant)
- [ ] **Is the format standardized across all plants?**
- [ ] **What is "legacy Matcom language"?**
- [ ] **Which equipment types consume these files?**

#### Plant Variability Assessment
For each plant, specify:
| Plant | File Format | Equipment Types | Parser Type | Updatable? |
|-------|-------------|-----------------|-------------|------------|
| Laurel | ? | Trim saw, printer, etc. | ? | ? |
| Sacopan | ? | ? | ? | ? |
| Wahpeton | ? | ? | ? | ? |
| Verdi | ? | ? | ? | ? |

**If formats vary by plant/equipment, scope increases by 300-500%**

### 2.3 Automatic Confirmations

#### Integration Starting Point
**CRITICAL: Are counting devices already integrated?**

- [ ] **Scenario A:** PLCs already in PI/SCADA (low effort)
- [ ] **Scenario B:** New PLC integration required (high effort)
- [ ] **Scenario C:** Mixed integration (very high effort)

#### Confirmation Strategy
Please clarify:
- [ ] When should confirmations post? (real-time vs batch)
- [ ] How are counts attributed to production orders?
- [ ] Is this Components division only or all plants?
- [ ] Can we start with manual confirmations as MVP?

#### Equipment Integration Matrix
For each counting point:
| Location | Equipment | Currently Integrated? | In What System? | Protocol |
|----------|-----------|---------------------|-----------------|----------|
| Laurel - Brownboard | Dropout sensor | ? | ? | ? |
| Laurel - Cut Coat | Laser eye | ? | ? | ? |
| Laurel - SMC Bin | CIM PLC | ? | ? | ? |
| Laurel - Fiberglass | Dropout sensor | ? | ? | ? |
| Sacopan - Forming | Sensor | ? | ? | ? |

---

## SECTION 3: DRIVER CHECK-IN SYSTEM (LAUREL)

### 3.1 Operational Model

#### Supervision Approach
**Choose ONE model:**
- [ ] **Attended:** Traditional scale house operator
- [ ] **Semi-Attended:** Remote supervision via video
- [ ] **Unattended:** Full self-service (24/7)

**Each model has vastly different costs ($30K vs $350K)**

### 3.2 Technical Requirements

#### Scale System
- [ ] Make/model of existing scales
- [ ] Communication protocol available
- [ ] Current integration (if any)

#### Infrastructure
- [ ] Network availability at scale location
- [ ] Environmental conditions (outdoor kiosk needs?)
- [ ] Power availability

#### Failure Handling
- [ ] Manual fallback process when system down
- [ ] Data backflush procedures
- [ ] Acceptable downtime

---

## SECTION 4: TIMELINE & RESOURCES

### 4.1 Critical Path Dependencies

| Dependency | Required By | Current Status | Risk Level |
|------------|-------------|----------------|------------|
| Ignition 8.3 Stable | Sept 2025 | Unproven | CRITICAL |
| APIM/Kafka Ready | Oct 2025 | Unknown | CRITICAL |
| Response File Specs | July 2025 | Missing | HIGH |
| PLC Integration | Nov 2025 | Unknown | HIGH |

### 4.2 Resource Requirements

From Owens Corning:
- [ ] S4/HANA test environment
- [ ] Kafka cluster access
- [ ] Network infrastructure
- [ ] Equipment access for testing
- [ ] Subject matter experts

From Grantek:
- [ ] Ignition development team
- [ ] Integration specialists
- [ ] PLC programmers (if required)
- [ ] Project management
- [ ] Testing resources

---

## SECTION 5: TESTING & VALIDATION

### 5.1 Test Environment Requirements

- [ ] Will OC provide S4/HANA test instance?
- [ ] Test Kafka cluster availability?
- [ ] Response file test equipment?
- [ ] Scale system test capability?
- [ ] Network outage simulation?

### 5.2 Acceptance Criteria

Please define measurable success criteria for:
- [ ] Message processing success rate
- [ ] Confirmation accuracy
- [ ] Response file compatibility
- [ ] System availability
- [ ] Performance benchmarks

---

## SECTION 6: RISK MITIGATION

### 6.1 Recommended Phased Approach

**Phase 1: Foundation (Reduce Risk)**
- Direct REST API integration (proven technology)
- Response file emulation (maintain compatibility)
- Manual confirmations with count display
- Attended driver check-in

**Phase 2: Enhancement (Add Complexity)**
- Add Kafka/APIM when stable
- Optimize response files
- Semi-automatic confirmations
- Kiosk-based check-in

**Phase 3: Full Vision (If Successful)**
- Full event streaming
- Modern file formats
- Automatic confirmations
- Unattended operations

### 6.2 Go/No-Go Decision Points

**September 2025:** Kafka/APIM ready? If no → REST API only
**October 2025:** Response files tested? If no → Delay equipment integration
**November 2025:** Confirmations tested? If no → Manual process
**December 2025:** Integration complete? If no → Phased go-live

---

## SECTION 7: COMMERCIAL CONSIDERATIONS

### 7.1 Scope Variability

Based on RFI responses, effort could range:

| Component | Minimum Scope | Maximum Scope | Variable Factor |
|-----------|---------------|---------------|-----------------|
| SAP Integration | 400 hrs | 1,200 hrs | 3x |
| Response Files | 200 hrs | 1,000 hrs | 5x |
| Auto Confirmations | 200 hrs | 2,400 hrs | 12x |
| Driver Check-in | 100 hrs | 800 hrs | 8x |
| **TOTAL** | **900 hrs** | **5,400 hrs** | **6x** |

### 7.2 Recommended Commercial Structure

Given the high uncertainty:
1. **Discovery Phase:** Fixed price assessment (4-6 weeks)
2. **Design Phase:** T&M based on discovery findings
3. **Implementation:** Fixed price after design complete
4. **Support:** Separate maintenance agreement

---

## RESPONSE REQUIREMENTS

### Response Format
Please provide responses in the following structure:
1. **Direct answers** to all checkbox items
2. **Supporting documentation** (architecture diagrams, sample files, etc.)
3. **Risk acknowledgment** for identified issues
4. **Decision authority** confirmation for key choices

### Response Components Required
- [ ] Completed response to all questions in this RFI
- [ ] Network architecture diagrams
- [ ] Sample response files (5+ per plant)
- [ ] PI system documentation
- [ ] Current process workflows
- [ ] Infrastructure assessments
- [ ] Decision on architecture options
- [ ] Risk mitigation preferences

### Timeline
- **RFI Issued:** [Date]
- **Questions Due:** [Date + 7 days]
- **Responses Due:** [Date + 14 days]
- **Clarification Meeting:** [Date + 17 days]
- **Go/No-Go Decision:** [Date + 21 days]

---

## APPENDICES

### Appendix A: Technical Detail Documents
The following detailed analyses are available upon request:
1. SAP Integration Architecture & Boundaries 
2. PI System Integration Deep Dive 
3. Response File Format Analysis 
4. Automatic Confirmations Scope Analysis 
5. Driver Check-in System Analysis

### Appendix B: Risk Register

| Risk | Probability | Impact | Mitigation Required |
|------|------------|--------|-------------------|
| Event Streams instability | HIGH | CRITICAL | Alternative architecture |
| APIM not ready | MEDIUM | CRITICAL | REST API fallback |
| Response file complexity | HIGH | HIGH | Discovery phase |
| PLC integration scope | HIGH | HIGH | Early assessment |
| Timeline aggressive | HIGH | CRITICAL | Phased approach |

### Appendix C: Glossary
- **AF:** Asset Framework (PI System)
- **APIM:** API Management (Azure)
- **DET:** Double End Tenoner (Trim Saw)
- **Event Streams:** Ignition 8.3 Kafka module
- **Matcom:** Unknown legacy system/language
- **UNS:** Unified Namespace

---

## CONTACT INFORMATION

**Project Manager:**  
Name: [PM Name]  
Email: [PM Email]  
Phone: [PM Phone]  

**Technical Lead:**  
Name: [Tech Lead Name]  
Email: [Tech Lead Email]  
Phone: [Tech Lead Phone]  

**Commercial Contact:**  
Name: [Commercial Name]  
Email: [Commercial Email]  
Phone: [Commercial Phone]  

---

*This RFI represents our professional assessment of the Project Phoenix requirements. The significant uncertainties identified must be resolved before accurate scoping, pricing, and scheduling can be provided. We recommend treating this as a multi-phase project with clear decision gates to manage risk.*

---

**END OF RFI DOCUMENT**