# REQUEST FOR INFORMATION (RFI)
## Project Phoenix - Doors Division S4/HANA Integration

### PROJECT INFORMATION
**RFI Number:** [RFI-2025-###]  
**Date Issued:** [Date]  
**Response Due Date:** [Date]  
**Project Name:** Project Phoenix - Doors Conversion to S4  
**Client:** Owens Corning  
**Initial Sites:** Sacopan QC, Laurel MS, Wahpeton ND, Verdi NV  
**Production Deadline:** February 6, 2026  
**Integration Testing Deadline:** December 19, 2025  

---

## 1. EXECUTIVE SUMMARY
This RFI seeks comprehensive technical information for implementing an Ignition-based MES platform supporting Owens Corning's Door Components division migration to S4/HANA. The system will serve as the shop floor standard platform across 4 initial sites with eventual deployment to 30 plants, providing local cache capabilities during WAN/cloud outages and serving as the digital foundation for manufacturing operations.

---

## 2. SAP S4/HANA INTEGRATION

### 2.1 Production Order Data Flow (SAP → Ignition)
Please confirm ability to receive and process:
- [ ] Order Number, Material Number, Quantity
- [ ] Planned Start/End Dates
- [ ] Routing/Operations with Work Center assignments
- [ ] Control Key Data with flags for:
  - Automatic Confirmation
  - Automatic Goods Receipt
  - External Processing
  - Print Control
- [ ] MTO Configuration including:
  - Sales Order Reference
  - Customer-Specific Drawing/Specs
  - Variant Configuration values

### 2.2 Confirmation Interface (Ignition → SAP)
Confirm capability to execute the following 4-step API process:
1. [ ] Production Order Confirmation (generates GR and creates IBD for EWM sloc)
   - API: OP_API_PROD_ORDER_CONFIRMATIO_2_SRV_0001
2. [ ] Handling Unit Creation
   - API: OP_HANDLINGUNIT_0001
3. [ ] Pack HU onto Inbound Delivery
   - API: OP_HANDLINGUNIT_0001
4. [ ] Post Goods Receipt for Inbound Delivery
   - API: OP_API_INBOUND_DELIVERY_SRV_0002

### 2.3 Error Handling Requirements
- [ ] Acknowledge responsibility for complete error handling (S4 "Clean Core" principle)
- [ ] Describe error recovery strategies for failed API calls
- [ ] Detail retry logic and escalation procedures
- [ ] Explain data consistency maintenance during partial failures

---

## 3. AUTOMATION INTEGRATION

### 3.1 OSI PI System
- [ ] Current PI system version and configuration
- [ ] Data points to be integrated
- [ ] Real-time data requirements
- [ ] Historical data access needs

### 3.2 Response File Generation
Please provide details on replacing the current MasterPack response file system:
- [ ] File format specifications (6-digit ticket number naming)
- [ ] Data elements required from S4
- [ ] File location and access methods
- [ ] 60-day purge cycle implementation
- [ ] Integration with trim saw scanning process

### 3.3 Automatic Confirmation Points
Provide PLC integration details for automated counting at:

**Laurel Site:**
- [ ] Brownboard press dropout
- [ ] Cut coat operation
- [ ] SMC Bin production (PLC-CIM)
- [ ] Fiberglass press dropout count

**Sacopan Site:**
- [ ] After forming operation (Brownboards)

---

## 4. LABELING SYSTEM

### 4.1 Loftware Integration
- [ ] API endpoints and authentication methods
- [ ] Label template management
- [ ] Error handling for failed print jobs
- [ ] Queue management capabilities

### 4.2 Customer-Specific Requirements
Confirm ability to handle:
- [ ] Metree/Mitri dimension conversion (millwork to inches)
- [ ] Customer SKU printing (optional field)
- [ ] Purchase Order number printing (customer-level option)
- [ ] Bilingual compliance statements (English/French for Canada)
- [ ] Stock orders under dummy customer (999999)

### 4.3 Printing Specifications
- [ ] Inkjet printer integration at trim saws (DET)
- [ ] Support for Interior and Entry Fiberglass doors only
- [ ] VMI program support for specific customers
- [ ] Make-to-Stock labeling for Menards

### 4.4 Mobile Application
- [ ] Android RF gun compatibility
- [ ] Shop floor UI screen specifications
- [ ] Barcode scanning capabilities
- [ ] Offline operation requirements

---

## 5. DRIVER CHECK-IN APPLICATION (LAUREL SPECIFIC)

### 5.1 Functionality Requirements
- [ ] Driver check-in/check-out workflow
- [ ] Integration with existing load cell system
- [ ] Weight capture for incoming/outgoing trucks
- [ ] Data storage and reporting requirements

### 5.2 Technical Specifications
- [ ] Load cell system interface details
- [ ] User interface requirements
- [ ] Security and access control
- [ ] Integration with transportation management

---

## 6. PRODUCTION DASHBOARDS

### 6.1 Dashboard Requirements
- [ ] Operation-specific KPIs
- [ ] Real-time data refresh rates
- [ ] Mobile responsiveness
- [ ] Role-based dashboard configurations

### 6.2 Data Sources
- [ ] PLC data integration points
- [ ] SAP data requirements
- [ ] PI system metrics
- [ ] Manual entry capabilities

---

## 7. NETWORK & INFRASTRUCTURE

### 7.1 WAN/Cloud Outage Handling
- [ ] Local cache requirements and capacity
- [ ] Data synchronization procedures
- [ ] Offline operation capabilities
- [ ] Recovery procedures after connectivity restoration

### 7.2 Site-Specific Infrastructure
For each site (Sacopan, Laurel, Wahpeton, Verdi):
- [ ] Network topology and bandwidth
- [ ] Server specifications
- [ ] Firewall configurations
- [ ] VPN access requirements

---

## 8. DEPLOYMENT & TIMELINE

### 8.1 Phased Deployment
- [ ] Components division (4 sites) deployment plan
- [ ] Subsequent 12-plant deployment waves
- [ ] Rollback procedures
- [ ] Success criteria for each phase

### 8.2 Critical Milestones
- [ ] Integration testing completion: December 19, 2025
- [ ] Production go-live: February 6, 2026
- [ ] Post-implementation support period
- [ ] Performance benchmarks

---

## 9. TRAINING & DOCUMENTATION

### 9.1 Training Requirements
- [ ] Operator training programs
- [ ] Administrator training
- [ ] Train-the-trainer approach
- [ ] Documentation standards

### 9.2 Support Model
- [ ] Post-implementation support structure
- [ ] SLA requirements
- [ ] Escalation procedures
- [ ] Knowledge transfer plan

---

## 10. ADDITIONAL INFORMATION REQUESTED

### 10.1 Current State Documentation
Please provide:
- [ ] MasterPack system documentation
- [ ] Current response file samples
- [ ] Label format specifications
- [ ] PLC communication protocols
- [ ] Network architecture diagrams

### 10.2 Testing Requirements
- [ ] Test environment specifications
- [ ] Test data requirements
- [ ] User acceptance testing procedures
- [ ] Performance testing criteria

---

## RESPONSE INSTRUCTIONS

### Submission Requirements
- **Format:** PDF with supporting documentation
- **Deadline:** [Date]
- **Submit to:** [Email]
- **Subject Line:** "RFI Response - Project Phoenix - [Company Name]"

### Questions & Clarifications
- **Question Deadline:** [Date]
- **Submit questions to:** [Email]
- **Response distribution:** [Date]

### Evaluation Criteria
Responses evaluated on:
1. Technical capability alignment
2. S4/HANA integration experience
3. Error handling approach
4. Timeline feasibility
5. Multi-site deployment experience

---

## APPENDICES

### Appendix A: Division Structure
- **Door Components** (Phase 1)
- **Entry Doors** (Future Phase)
- **Interior Doors** (Future Phase)
- **DorFab** (Future Phase)

### Appendix B: Technical Acronyms
- **DET**: Double End Tenoner (Trim Saw)
- **EWM**: Extended Warehouse Management
- **GR**: Goods Receipt
- **HU**: Handling Unit
- **IBD**: Inbound Delivery
- **MTO**: Make-to-Order
- **MTS**: Make-to-Stock
- **PI**: Process Information (OSI PI)
- **PLC**: Programmable Logic Controller
- **SMC**: Sheet Molding Compound
- **STO**: Stock Transfer Order
- **VMI**: Vendor Managed Inventory
- **WAN**: Wide Area Network

### Appendix C: Contact Information
**Project Manager:**  
[Name, Title, Email, Phone]

**Technical Lead:**  
[Name, Title, Email, Phone]

**Procurement Contact:**  
[Name, Title, Email, Phone]

---

*This RFI contains proprietary and confidential information. All responses will be treated as confidential.*