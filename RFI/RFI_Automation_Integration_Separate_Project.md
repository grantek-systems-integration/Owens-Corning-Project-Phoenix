# RFI SECTION: AUTOMATION INTEGRATION PROJECT
## Separate Project Scope Definition & Requirements

---

## PROJECT SEPARATION & DEPENDENCIES

### Critical Clarification Required
**Is Automation Integration a separate project with independent timeline and budget?**

- [ ] Separate project number/budget allocation?
- [ ] Independent timeline (can it go live after February 2026)?
- [ ] Different project team/resources?
- [ ] Separate acceptance criteria?
- [ ] Independent change control process?

### Dependencies Between Projects
**How do the SAP Integration and Automation Integration projects interact?**

| Dependency | SAP Integration Needs | Automation Needs | Coupling Level |
|------------|----------------------|------------------|----------------|
| Production Orders | Must receive from SAP | Must consume for Response File | TIGHT |
| Confirmations | Must send to SAP | Must generate from PLCs | TIGHT |
| Label Data | Gets from SAP | Triggers from automation | MEDIUM |
| Infrastructure | Ignition platform | Same Ignition instance? | SHARED |
| Go-Live | Feb 6, 2026 | Same date required? | ??? |

---

## 1. OSI PI SYSTEM INTEGRATION

### 1.1 Current PI System Assessment
**Existing Infrastructure Discovery:**

- [ ] **PI System Version**: Currently deployed version at each site
- [ ] **Architecture**: Single server, collective, or distributed?
- [ ] **Tag Count**: Number of tags per site
- [ ] **Scan Classes**: Current collection frequencies
- [ ] **Historians**: Local vs centralized data storage
- [ ] **Licensing**: Current license model and limits

### 1.2 Integration Approach
**How will Ignition interface with PI?**

**Option 1: Direct PI Connection**
- [ ] Use Ignition's OPC UA/DA connection to PI
- [ ] Real-time data access only
- [ ] Limited historical data access
- [ ] Simpler architecture

**Option 2: PI Web API**
- [ ] RESTful API integration
- [ ] Full historical data access
- [ ] More complex authentication
- [ ] Better for analytics

**Option 3: SQL Data Access**
- [ ] Via PI SQL Data Access Server (if licensed)
- [ ] Direct SQL queries
- [ ] Good for reporting
- [ ] Additional licensing cost?

**Option 4: Replace PI**
- [ ] Migrate tags directly to Ignition Historian
- [ ] Eliminate PI dependency
- [ ] Major scope change
- [ ] Migration complexity

### 1.3 Data Flow Requirements
**What PI data does Ignition need?**

- [ ] **Real-time Process Values**: Which tags/parameters?
- [ ] **Historical Trends**: Time range requirements?
- [ ] **Calculations**: Performed in PI or Ignition?
- [ ] **Events**: Batch/event frame data needed?
- [ ] **Alarms**: PI system alarms integration?

### 1.4 Performance & Architecture
**Technical Requirements:**

- [ ] Update frequency from PI (seconds, minutes)?
- [ ] Number of concurrent PI connections
- [ ] Network bandwidth between Ignition and PI
- [ ] Redundancy requirements
- [ ] Failover behavior if PI unavailable

---

## 2. RESPONSE FILE REPLACEMENT SYSTEM

### 2.1 Current MasterPack Response File Analysis
**Detailed understanding of existing system:**

**File Generation:**
- [ ] Exact trigger for file creation (schedule finalization)
- [ ] Source data elements from MasterPack
- [ ] File naming convention (6-digit ticket number)
- [ ] File format (CSV, fixed-width, XML, custom?)
- [ ] Sample files from each plant (with data dictionary)

**File Distribution:**
- [ ] Current file locations (network shares per plant)
- [ ] Access permissions/security
- [ ] Replication or sync mechanisms
- [ ] Backup procedures

**File Consumption:**
- [ ] Equipment reading files (trim saws, printers, Jetta Mark)
- [ ] Parsing logic (VB scripts, PLC code?)
- [ ] Error handling if file not found
- [ ] Manual override procedures

### 2.2 Ignition-Based Replacement Design
**Key Design Decisions:**

**Architecture Options:**

**Option A: File Emulation**
- Generate identical files in same locations
- Zero change to downstream equipment
- Maintains 60-day purge cycle
- Lowest risk, fastest implementation

**Option B: Database-Driven**
- Store data in Ignition database
- Equipment queries via API/SQL
- Requires downstream changes
- More scalable, modern approach

**Option C: Hybrid Approach**
- Database storage with file generation
- Gradual migration path
- Backwards compatibility
- Higher complexity

### 2.3 Data Mapping Requirements
**MasterPack to S4/HANA field mapping:**

| MasterPack Field | S4/HANA Source | Transformation Required | Critical? |
|-----------------|----------------|------------------------|-----------|
| Ticket Number | Production Order | Format conversion? | YES |
| Material Code | Material Master | Direct mapping? | YES |
| Customer Info | Sales Order | Special handling for 999999? | YES |
| Dimensions | Product Specs | Metric/Imperial conversion? | YES |
| Print Fields | ??? | Complex logic per customer | YES |
| [Complete mapping needed] | | | |

### 2.4 Special Requirements
**Customer-specific logic that must be preserved:**

**Metree/Mitri Requirements:**
- [ ] Dimension conversion algorithm (millwork to inches)
- [ ] When to apply conversion (by customer code)
- [ ] VMI-specific fields
- [ ] Special formatting rules

**Menards Requirements:**
- [ ] Stock order handling (customer 999999)
- [ ] Label suppression rules
- [ ] Packaging information
- [ ] Make-to-stock indicators

**Printing Requirements:**
- [ ] Equipment-specific formatting (trim saw vs Jetta Mark)
- [ ] Bilingual text handling (Canada)
- [ ] Compliance statements
- [ ] Optional field logic

### 2.5 Migration Strategy
**How to transition from MasterPack to Ignition?**

- [ ] Parallel run period (both systems generating files)?
- [ ] Plant-by-plant cutover?
- [ ] Equipment-by-equipment migration?
- [ ] Rollback procedures?
- [ ] Data validation approach?

---

## 3. AUTOMATIC CONFIRMATION VIA PLC INTEGRATION

### 3.1 PLC Infrastructure Assessment
**Per-site equipment inventory:**

**Laurel Equipment:**
| Equipment | PLC Type | Protocol | Current Integration | Count Logic |
|-----------|----------|----------|-------------------|-------------|
| Brownboard Press | ??? | ??? | Dropout sensor | Items/minute? |
| Cut Coat Operation | ??? | ??? | Laser eye? | Sheets? |
| SMC Bin Production | CIM PLC | ??? | Existing count? | Bins? |
| Fiberglass Press | ??? | ??? | Dropout sensor | Items/minute? |

**Sacopan Equipment:**
| Equipment | PLC Type | Protocol | Current Integration | Count Logic |
|-----------|----------|----------|-------------------|-------------|
| Forming Operation | ??? | ??? | Sensor type? | Brownboards/minute? |

### 3.2 Integration Architecture
**How will Ignition connect to PLCs?**

**Connection Methods:**
- [ ] Direct OPC UA/DA to each PLC
- [ ] Through existing SCADA/HMI
- [ ] Via PI System tags
- [ ] Through middleware/gateway
- [ ] Mixed approach per equipment type

**Network Architecture:**
- [ ] PLC network segmentation
- [ ] Firewall rules required
- [ ] DMZ requirements
- [ ] Bandwidth requirements
- [ ] Redundant paths needed?

### 3.3 Counting Logic & Attribution
**Critical: How to attribute counts to production orders?**

**Current State:**
- Sensors count physical items
- No order information at sensor level
- How is count currently attributed?

**Required Logic:**
- [ ] Order start/stop signals from where?
- [ ] Multiple orders on same line handling
- [ ] Scrap vs good product differentiation
- [ ] Rework handling
- [ ] Order changeover procedures

**Data Collection Requirements:**
- [ ] Count frequency (real-time, batch)?
- [ ] Buffer/accumulator logic
- [ ] Confirmation trigger (time, quantity, event)?
- [ ] Manual adjustment capability
- [ ] Audit trail requirements

### 3.4 Edge Cases & Exceptions
**Scenarios requiring special handling:**

- [ ] Partial runs/interrupted production
- [ ] Quality holds/quarantine
- [ ] Equipment maintenance periods
- [ ] Sensor failures/miscounts
- [ ] Multiple products same production run
- [ ] Setup/teardown waste
- [ ] Sample production

### 3.5 Validation & Reconciliation
**How to ensure count accuracy?**

- [ ] Manual count verification procedures
- [ ] Reconciliation with warehouse receipts
- [ ] Variance tolerance levels
- [ ] Investigation triggers
- [ ] Historical accuracy metrics

---

## 4. PROJECT EXECUTION CONSIDERATIONS

### 4.1 Phasing Options
**Given this is a separate project, what's the optimal approach?**

**Option 1: Sequential Execution**
- Complete SAP integration first
- Then add automation integration
- Lower resource conflict
- Longer overall timeline

**Option 2: Parallel Execution**
- Both projects simultaneously
- Faster overall delivery
- Higher resource needs
- More complex coordination

**Option 3: Phased by Function**
- Response files first (critical path)
- PI integration second
- Auto confirmations last
- Risk mitigation approach

### 4.2 Resource Requirements
**Specialized skills needed:**

- [ ] PLC programming (per platform type)
- [ ] PI System administration
- [ ] Network engineering (OT/IT convergence)
- [ ] Legacy system reverse engineering
- [ ] Equipment-specific expertise

### 4.3 Testing Requirements
**Unique to automation integration:**

- [ ] PLC simulation environment
- [ ] Response file validation tools
- [ ] Count accuracy testing
- [ ] Load testing with real equipment
- [ ] Failover testing
- [ ] Equipment vendor coordination

---

## 5. RISKS & MITIGATION STRATEGIES

### 5.1 Technical Risks

| Risk | Impact | Probability | Mitigation |
|------|--------|------------|------------|
| Legacy parsing code unavailable | HIGH | HIGH | Reverse engineer from files |
| PLC protocols proprietary | HIGH | MEDIUM | Vendor engagement required |
| PI System version incompatible | MEDIUM | LOW | Upgrade path assessment |
| Count attribution logic complex | HIGH | HIGH | Extensive testing required |
| Network segmentation issues | MEDIUM | MEDIUM | Early architecture review |

### 5.2 Operational Risks

| Risk | Impact | Probability | Mitigation |
|------|--------|------------|------------|
| Production disruption during cutover | CRITICAL | MEDIUM | Parallel run strategy |
| Operator resistance to new system | MEDIUM | HIGH | Training and change management |
| Data loss during migration | HIGH | LOW | Comprehensive backup plan |
| Equipment vendor support | MEDIUM | MEDIUM | Early engagement required |

---

## 6. CRITICAL INFORMATION NEEDED

### 6.1 Immediate Requirements
To properly scope this project, provide:

1. **Complete MasterPack response file samples** with data dictionary
2. **PLC inventory** with types, protocols, and current integration methods
3. **PI System architecture** diagrams and tag lists
4. **Network diagrams** showing PLC, PI, and IT networks
5. **Current count attribution logic** documentation

### 6.2 Decisions Required

1. **Project Structure**: Separate project or integrated with SAP?
2. **Timeline**: Must this go live with SAP integration or can it follow?
3. **Architecture**: File emulation vs modern replacement?
4. **PI Strategy**: Integrate with or replace?
5. **Confirmation Logic**: Automatic only or manual override capability?

---

## 7. RECOMMENDED APPROACH

### 7.1 Minimum Viable Product (MVP)
**Phase 1: Critical Path Items**
- Response file generation (emulation mode)
- Basic PI integration (read-only)
- Manual confirmation capability

**Phase 2: Automation Enhancement**
- Automatic confirmations from PLCs
- Advanced PI analytics
- Response file optimization

**Phase 3: Full Modernization**
- API-based equipment integration
- Real-time dashboards
- Predictive analytics

### 7.2 Success Criteria
**Measurable objectives:**
- Response files available within X seconds
- 99.9% count accuracy
- Zero production disruption
- Operator acceptance rate > 90%

---

*This section requires detailed technical information about existing automation infrastructure to accurately scope the effort and identify integration risks.*