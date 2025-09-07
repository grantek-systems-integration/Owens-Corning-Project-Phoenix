# RFI SECTION: OSI PI SYSTEM INTEGRATION - DETAILED ANALYSIS
## Critical Definition of Scope, Direction, and Purpose

---

## FUNDAMENTAL QUESTION: WHAT IS THE PI INTEGRATION?

### The Problem
The RFQ states "Pi system will be integrated with Ignition" without defining:
- Direction of data flow
- Purpose of integration  
- Current PI role
- Future PI role
- Coexistence or replacement strategy

---

## 1. INTEGRATION DIRECTIONALITY & PURPOSE

### 1.1 Potential Scenario A: Ignition as Data Consumer from PI
**Ignition READS from existing PI System**

If this is the case, clarify:
- [ ] **Purpose**: Why does Ignition need PI data?
  - Historical production data for dashboards?
  - Real-time process values for logic?
  - Calculated/aggregated values?
  - Event frames for batch tracking?
  - Asset Framework context?

- [ ] **Data Volume**: What scale of data consumption?
  - Number of tags to read
  - Historical data depth (months, years?)
  - Query frequency
  - Real-time vs periodic updates

- [ ] **Use Cases**: Specific functionality requiring PI data?
  - Production order correlation with process data?
  - Quality parameter validation?
  - OEE calculations using PI metrics?
  - Yield calculations from PI totalization?

### 1.2 Potential Scenario B: Ignition as Data Source to PI
**Ignition WRITES to existing PI System**

If this is the case, clarify:
- [ ] **Purpose**: Why send Ignition data to PI?
  - Maintain PI as system of record?
  - Corporate reporting requirements?
  - Integration with other PI-dependent systems?
  - Regulatory/compliance needs?

- [ ] **Data Types**: What Ignition data goes to PI?
  - Production confirmations?
  - Calculated OEE metrics?
  - SAP transaction results?
  - Operator entries?

- [ ] **Architecture**: How would Ignition write to PI?
  - PI OPC DA/UA Server?
  - PI Web API?
  - PI SQL Data Access?
  - PI Connector/Interface?
  - Custom interface development?

### 1.3 Potential Scenario C: Bidirectional Integration
**Ignition both READS FROM and WRITES TO PI**

If bidirectional, define:
- [ ] Data flows in each direction
- [ ] Synchronization requirements
- [ ] Conflict resolution rules
- [ ] Master system for each data type
- [ ] Performance implications

### 1.4 Potential Scenario D: PI Replacement
**Ignition REPLACES PI functionality**

If replacement is considered:
- [ ] Migration strategy for historical data
- [ ] Parallel run requirements
- [ ] Downstream system impacts
- [ ] Licensing implications
- [ ] Corporate standards compliance

---

## 2. CURRENT PI SYSTEM ARCHITECTURE ASSESSMENT

### 2.1 PI Infrastructure Footprint
**What currently exists at each site?**

**Core Components:**
- [ ] **PI Data Archive**: Version, size, tag count
- [ ] **PI AF Server**: Version, database count
- [ ] **PI Interfaces**: Types and count
  - OPC Interfaces?
  - Custom interfaces?
  - RDBMS interfaces?
  - File-based interfaces?
- [ ] **PI Connectors**: Modern connectors in use?
- [ ] **Client Tools**: ProcessBook, Vision, DataLink usage?

**Architecture Pattern:**
- [ ] Single site servers or collective architecture?
- [ ] Centralized vs distributed deployment?
- [ ] Cloud components (PI Cloud Connect)?
- [ ] High availability configuration?
- [ ] Disaster recovery setup?

### 2.2 Current Data Collection Architecture
**How does data currently flow into PI?**

- [ ] **PLC/DCS Integration**:
  - Direct OPC connections?
  - Intermediate SCADA/HMI?
  - Gateway/protocol converters?
  - Scan rates and collection frequencies?

- [ ] **Manual Data Entry**:
  - PI Manual Logger usage?
  - Web entry forms?
  - Excel imports?

- [ ] **System Integrations**:
  - ERP/MES data into PI?
  - Lab systems integration?
  - Other business systems?

### 2.3 OPC Server Capabilities
**Critical: What OPC infrastructure exists?**

- [ ] **PI OPC DA Server**: Installed and configured?
  - Version and licensing
  - Current client connections
  - Security configuration
  - Performance limits

- [ ] **PI OPC UA Server**: Available?
  - Version and capabilities
  - Certificate management
  - Security profiles supported

- [ ] **PI OPC HDA Server**: For historical data?
  - Query performance
  - Concurrent connection limits
  - Historical data access patterns

### 2.4 Asset Framework (AF) Implementation
**Critical: Is AF in use and how extensively?**

**AF Structure Assessment:**
- [ ] **AF Deployment**: Is AF implemented?
- [ ] **Hierarchy Depth**: How many levels?
- [ ] **Element Count**: Number of assets modeled?
- [ ] **Template Usage**: Standardized templates?
- [ ] **Attribute Types**:
  - PI Point references?
  - Calculated attributes?
  - Table lookups?
  - Event frame generation?

**AF Content Mapping:**
- [ ] **Equipment Model**: Does AF model the production equipment?
- [ ] **Product Hierarchy**: Products/materials in AF?
- [ ] **Organizational Structure**: Sites/areas/lines in AF?
- [ ] **Batch/Lot Tracking**: Event frames for production runs?

**Critical Question for Ignition:**
- [ ] Should Ignition traverse AF structure or access PI points directly?
- [ ] Will Ignition need to understand AF context?
- [ ] Should Ignition maintain parallel asset model or use AF?
- [ ] Will Ignition create/update AF elements?

---

## 3. INTEGRATION TECHNICAL APPROACHES

### 3.1 Option Analysis Matrix

| Approach | Pros | Cons | Best For | Complexity |
|----------|------|------|----------|------------|
| **OPC DA** | Mature, widely supported | Windows-only, DCOM issues | Real-time data only | Medium |
| **OPC UA** | Platform independent, secure | Newer, less PI deployment | Real-time + some history | Medium |
| **OPC HDA** | Historical data access | Complex queries, performance | Reporting/analytics | High |
| **PI Web API** | RESTful, modern, full features | Authentication complexity | Full functionality | High |
| **PI ODBC/OLEDB** | SQL-like queries | Read-only, limited | Simple reporting | Low |
| **PI SDK** | Full functionality | Deprecated, Windows-only | Legacy integration | High |
| **AF SDK** | Full AF access | Complex, Windows-only | AF-centric design | Very High |

### 3.2 Data Synchronization Patterns

**Pattern 1: Real-Time Streaming**
- OPC subscription model
- Change-based updates
- Low latency
- Higher resource usage

**Pattern 2: Periodic Polling**
- Scheduled queries
- Batch updates
- Predictable load
- Potential data lag

**Pattern 3: Event-Driven**
- Triggered by production events
- On-demand queries
- Efficient resource use
- Complex orchestration

**Pattern 4: Hybrid Approach**
- Real-time for critical tags
- Periodic for reporting data
- Event-driven for batch data
- Most complex

---

## 4. FUNCTIONAL ROLE OF PI IN NEW ARCHITECTURE

### 4.1 Validation & Compliance Role
**Is PI the system of record for regulatory compliance?**

- [ ] **FDA/GMP Requirements**: 21 CFR Part 11 compliance?
- [ ] **Audit Trail**: PI provides immutable audit trail?
- [ ] **Data Integrity**: PI ensures data hasn't been modified?
- [ ] **Retention Policies**: Long-term data archival in PI?
- [ ] **Electronic Signatures**: PI-based approval workflows?

**If YES, implications for Ignition:**
- Cannot replace PI (regulatory risk)
- Must write critical data to PI
- Must maintain PI validation status
- Synchronization critical

### 4.2 Operational Role
**What operational decisions depend on PI?**

- [ ] **Production Scheduling**: Based on PI metrics?
- [ ] **Quality Release**: PI data drives release decisions?
- [ ] **Maintenance**: Predictive maintenance from PI?
- [ ] **Energy Management**: Utility monitoring in PI?
- [ ] **Corporate KPIs**: Executive dashboards from PI?

### 4.3 Integration Hub Role
**What other systems depend on PI?**

- [ ] **Corporate Reporting**: BI tools reading PI?
- [ ] **ERP Integration**: SAP reading PI data?
- [ ] **LIMS Integration**: Lab systems linked to PI?
- [ ] **Cloud Analytics**: PI Cloud Connect in use?
- [ ] **Third-Party Tools**: Custom applications using PI?

---

## 5. SPECIFIC INTEGRATION REQUIREMENTS

### 5.1 Production Confirmation Correlation
**How do PI process values relate to production orders?**

Critical Questions:
- [ ] Does PI currently track production runs?
- [ ] How are PI tags correlated to orders?
- [ ] Time-based or event-based correlation?
- [ ] Who maintains this correlation today?
- [ ] Will Ignition take over this function?

### 5.2 Automatic Confirmation Data Flow
**For PLC-based confirmations, what role does PI play?**

Potential Architectures:
1. **PLCs → PI → Ignition → SAP**
   - PI remains data collector
   - Ignition reads from PI
   - Ignition correlates and confirms

2. **PLCs → Ignition → SAP + PI**
   - Ignition primary collector
   - Ignition writes to PI for history
   - Parallel data paths

3. **PLCs → PI + Ignition → SAP**
   - Both collect independently
   - Data validation between systems
   - Complex but robust

### 5.3 Response File Data Requirements
**Does the Response File need PI data?**

- [ ] Process parameters for printing?
- [ ] Quality metrics for release?
- [ ] Production conditions for tracking?
- [ ] Calculated values from PI?

---

## 6. PERFORMANCE & SCALABILITY CONSIDERATIONS

### 6.1 Data Volume Estimates
**Quantify the integration scale:**

Per Site Estimates Needed:
- [ ] Total PI tags in system
- [ ] Tags Ignition needs to access
- [ ] Historical queries per day
- [ ] Real-time subscription count
- [ ] Concurrent users
- [ ] Network bandwidth available

### 6.2 Performance Requirements
**Define acceptable performance:**

- [ ] Real-time data latency (<1 sec, <5 sec, <30 sec?)
- [ ] Historical query response time
- [ ] Bulk data transfer windows
- [ ] Peak load periods
- [ ] Degraded mode operation

### 6.3 Scalability Path
**Future growth considerations:**

- [ ] Adding more sites to PI
- [ ] Increasing tag counts
- [ ] More Ignition nodes
- [ ] Cloud migration potential
- [ ] Long-term architecture vision

---

## 7. CRITICAL DECISIONS REQUIRED

### 7.1 Strategic Direction
**Fundamental architecture decision needed:**

**Option 1: PI-Centric Architecture**
- PI remains primary historian
- Ignition complements PI
- All critical data flows through PI
- Higher complexity, lower risk

**Option 2: Ignition-Centric Architecture**
- Ignition becomes primary platform
- PI maintained for legacy/compliance
- Gradual PI retirement path
- Lower complexity, higher change management

**Option 3: Parallel Systems**
- Both systems fully deployed
- Redundant data collection
- Complex but highly available
- Highest cost

### 7.2 Implementation Priority
**What must be delivered for February 2026?**

**Minimum Viable Integration:**
- [ ] Read-only access sufficient initially?
- [ ] Which specific tags/data points?
- [ ] Real-time only or historical needed?
- [ ] Which sites must have PI integration?

**Future Enhancements:**
- [ ] Full bidirectional integration
- [ ] AF model synchronization
- [ ] Advanced analytics
- [ ] Complete PI replacement

---

## 8. INFORMATION NEEDED IMMEDIATELY

### 8.1 PI System Documentation
Please provide:
1. **PI Architecture diagrams** showing all components
2. **PI AF models** exported as XML
3. **Tag naming conventions** and structure
4. **Interface list** with data sources
5. **Current PI usage reports** (user access, query patterns)
6. **PI licensing details** and constraints

### 8.2 Specific Answers Required
1. **Primary Question**: Is Ignition reading from, writing to, or replacing PI?
2. **Regulatory Role**: Is PI validated/required for compliance?
3. **Data Ownership**: Who owns PI infrastructure going forward?
4. **Success Metrics**: What constitutes successful PI integration?
5. **Timeline**: Must PI integration go live with initial deployment?

---

## 9. RISK ASSESSMENT

### 9.1 Integration Risks

| Risk | Impact | Mitigation Question |
|------|--------|-------------------|
| Undefined requirements | CRITICAL | Can we proceed without clear PI role? |
| AF complexity | HIGH | Should we bypass AF initially? |
| Performance impacts | HIGH | Have load tests been performed? |
| Licensing constraints | MEDIUM | Are sufficient licenses available? |
| Skills gap | MEDIUM | Who has PI expertise? |

### 9.2 Go/No-Go Criteria
**PI integration should be deferred if:**
- [ ] Requirements remain undefined
- [ ] No clear business value identified
- [ ] Technical approach undecided
- [ ] Resources not available
- [ ] Not critical for initial go-live

---

*This analysis reveals that "PI integration" is mentioned but completely undefined in scope, direction, and purpose. These fundamental questions must be answered before any meaningful technical design can begin.*