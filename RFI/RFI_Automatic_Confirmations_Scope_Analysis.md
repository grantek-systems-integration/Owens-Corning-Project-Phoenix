# RFI SECTION: AUTOMATIC CONFIRMATIONS - SCOPE & INTEGRATION ANALYSIS
## Critical Definition of Field Device Integration Requirements

---

## FUNDAMENTAL SCOPE QUESTIONS

### The Core Ambiguity
The RFQ mentions "automatic confirmations" using PLC data but doesn't clarify:
- Are field devices already integrated somewhere?
- Is this new integration or data routing from existing systems?
- Are confirmations real-time or batch at end-of-line?
- Is this Components division only or all plants eventually?
- Who owns the PLC integration work?

---

## 1. EXISTING INTEGRATION STATUS ASSESSMENT

### 1.1 Current State Discovery
**Critical Question: What already exists vs. what must be built?**

**Scenario A: Field Devices Already Integrated**
```
Current State:
[PLCs] → [Existing System] → [Data Available]
                ↓
Need: [Ignition reads from existing system]
```
If this is true:
- [ ] What system currently collects this data? (PI, SCADA, HMI?)
- [ ] Is data already contextualized with production info?
- [ ] What protocols/APIs available to access it?
- [ ] Is historical data available?
- [ ] **Effort: Low** - Data routing only

**Scenario B: Direct PLC Integration Required**
```
Current State:
[PLCs] → [Nothing/Local HMI Only]

Need: [Ignition connects directly to PLCs]
```
If this is true:
- [ ] How many PLCs across all locations?
- [ ] What PLC types and protocols?
- [ ] Network accessibility from Ignition?
- [ ] Security/firewall implications?
- [ ] **Effort: High** - Full integration project

**Scenario C: Mixed Integration**
```
Current State:
[Some PLCs] → [PI System]
[Some PLCs] → [Local HMI]
[Some PLCs] → [Nothing]

Need: [Ignition integrates via multiple paths]
```
If this is true:
- [ ] Which devices via which path?
- [ ] Standardization opportunity?
- [ ] Phased implementation required?
- [ ] **Effort: Very High** - Complex architecture

### 1.2 Integration Inventory Matrix
**For each confirmation point, determine current state:**

| Plant | Location | Device Type | Currently Integrated? | System | Protocol | Data Available |
|-------|----------|-------------|---------------------|---------|----------|----------------|
| **Laurel** | Brownboard Press | Dropout Sensor | ??? | ??? | ??? | Count only? |
| **Laurel** | Cut Coat | Laser Eye? | ??? | ??? | ??? | Count + Quality? |
| **Laurel** | SMC Bin | CIM PLC | ??? | ??? | ??? | Bin count? |
| **Laurel** | Fiberglass Press | Dropout Sensor | ??? | ??? | ??? | Count only? |
| **Sacopan** | Forming | Sensor | ??? | ??? | ??? | Brownboard count? |
| **Wahpeton** | ??? | ??? | ??? | ??? | ??? | ??? |
| **Verdi** | ??? | ??? | ??? | ??? | ??? | ??? |

### 1.3 Data Availability Assessment
**What data is actually available from these devices?**

**Basic Counting (Minimal Integration):**
- Simple pulse/trigger per item
- No product information
- No quality data
- Just quantity

**Enhanced Counting (Moderate Integration):**
- Count with timestamp
- Production rate
- Running/stopped status
- Fault conditions

**Full Integration (Comprehensive):**
- Product identification
- Quality parameters
- Reject counts
- Process variables
- Batch/lot tracking

---

## 2. CONFIRMATION TIMING & STRATEGY

### 2.1 Real-Time vs. Batch Confirmations
**When should confirmations be posted to SAP?**

**Option A: Real-Time Confirmations**
```
[Sensor Trigger] → [Count++] → [Immediate SAP Post]
```
Implications:
- [ ] High API call volume to SAP
- [ ] Network reliability critical
- [ ] Error handling complex
- [ ] Rollback procedures needed
- [ ] Performance impact on production

**Option B: Buffered Real-Time**
```
[Sensor Triggers] → [Local Buffer] → [Periodic SAP Post (1-5 min)]
```
Implications:
- [ ] Balanced approach
- [ ] Reduces API calls
- [ ] Some delay in SAP visibility
- [ ] Buffer overflow handling needed
- [ ] Recovery after outages

**Option C: End-of-Line Consolidation**
```
[Multiple Sensors] → [Accumulate Counts] → [Single Confirmation at End]
```
Implications:
- [ ] Simplest implementation
- [ ] Minimal API calls
- [ ] Delayed SAP visibility
- [ ] WIP tracking challenges
- [ ] Scrap allocation issues

**Option D: Milestone-Based**
```
[Start Count] → [Intermediate Operations] → [End Count] → [Calculate & Confirm]
```
Implications:
- [ ] Yield calculation possible
- [ ] Scrap identification
- [ ] More complex logic
- [ ] Better visibility
- [ ] Reconciliation required

### 2.2 Production Order Attribution Problem
**Critical Issue: Sensors count items but don't know orders**

**The Challenge:**
```
Reality: [Sensor] → "Item detected" (no order info)
Need: [Sensor] → "Item for Order #12345 detected"
```

**Attribution Strategies:**

**Time-Based Attribution:**
- Order scheduled 8:00-10:00 → All counts in window = that order
- **Issues**: Delays, early starts, overlaps

**Operator-Initiated:**
- Operator selects order → All counts until change = that order
- **Issues**: Requires UI, operator errors, change tracking

**Signal-Based:**
- PLC signals order change → Reset counters
- **Issues**: PLC programming required, signal reliability

**Hybrid Approach:**
- Default time-based with operator override
- **Issues**: Complex logic, training required

### 2.3 Confirmation Scope Definition
**What exactly is being confirmed?**

- [ ] **Quantity Only**: Simple count of items produced
- [ ] **Quantity + Quality**: Good vs. reject counts
- [ ] **Full Production**: All operation confirmations
- [ ] **Material Consumption**: Backflush raw materials
- [ ] **Labor**: Operator time tracking
- [ ] **Machine Time**: Equipment utilization

---

## 3. PLANT-SPECIFIC VS. COMMON INFRASTRUCTURE

### 3.1 Components Division Analysis
**Is this truly Components-only or a pilot for all divisions?**

**Components-Only Scenario:**
- Limited to 4 sites initially
- Specific to door component production
- Simpler product structure
- May not scale to other divisions

**Universal Platform Scenario:**
- Design for all 30 plants
- Multiple product types
- Scalable architecture required
- Standardization critical

### 3.2 Infrastructure Commonality Assessment

| Aspect | Common Across Plants? | Evidence Needed | Impact on Design |
|--------|----------------------|-----------------|------------------|
| PLC Types | ??? | PLC inventory list | Driver requirements |
| Network Architecture | ??? | Network diagrams | Security design |
| Sensor Types | ??? | Equipment specs | Integration methods |
| Production Flow | ??? | Process flow diagrams | Logic complexity |
| Counting Logic | ??? | Current procedures | Standardization opportunity |
| Data Systems | ??? | IT architecture docs | Integration points |

### 3.3 Plant-Specific Requirements

**Laurel (4 counting points):**
- Most complex implementation
- Multiple equipment types
- CIM PLC mentioned specifically
- Driver check-in system adds complexity

**Sacopan (1 counting point):**
- Simpler implementation
- French language requirements?
- Metric units throughout?
- Different regulatory requirements?

**Wahpeton & Verdi:**
- [ ] Do these plants need automatic confirmations?
- [ ] Different equipment entirely?
- [ ] Future expansion only?

---

## 4. IMPLEMENTATION SCOPE SCENARIOS

### 4.1 Minimum Scope
**Bare minimum to meet requirement:**

```
Scope:
- Read existing counts from PI/SCADA
- Manual order association
- End-of-shift batch confirmation
- Components plants only
- No new PLC integration

Effort: ~200-400 hours
Risk: Low
Value: Low
```

### 4.2 Expected Scope
**Likely interpretation of requirements:**

```
Scope:
- Connect to existing PLCs where accessible
- Semi-automatic order attribution
- Hourly confirmations
- Laurel + Sacopan initially
- Some new integration required

Effort: ~800-1,600 hours
Risk: Medium
Value: Medium
```

### 4.3 Maximum Scope
**Full automation vision:**

```
Scope:
- Direct PLC integration for all devices
- Real-time confirmations
- Automatic order attribution
- All 4 plants
- Quality and scrap tracking
- Full equipment monitoring

Effort: ~2,400-4,800 hours
Risk: High
Value: High
```

### 4.4 Recommended Phased Approach

**Phase 1: Foundation (MVP)**
- Manual confirmations with count display
- Read from existing systems only
- Prove SAP integration works
- **3-4 months**

**Phase 2: Semi-Automation**
- Automated counts with manual attribution
- Hourly batch confirmations
- Laurel pilot
- **2-3 months**

**Phase 3: Full Automation**
- Real-time confirmations
- Automatic attribution
- All plants
- **4-6 months**

---

## 5. CRITICAL TECHNICAL CLARIFICATIONS

### 5.1 Integration Questions

1. **Are the counting devices already integrated into any system?**
2. **If yes, what system and how can Ignition access it?**
3. **If no, is PLC integration part of Grantek's scope?**
4. **Who will handle PLC programming for order signals?**
5. **What network segments are PLCs on?**

### 5.2 Functional Questions

1. **When should confirmations be posted (real-time vs batch)?**
2. **How is production attributed to orders currently?**
3. **What happens with scrap/rework counts?**
4. **Is yield calculation required?**
5. **Must WIP be tracked between operations?**

### 5.3 Scope Questions

1. **Is this Components division only forever?**
2. **Which plants must have automatic confirmations Day 1?**
3. **Can we implement manual first, automatic later?**
4. **Is equipment monitoring (OEE) in scope?**
5. **Should this integrate with existing Panther OEE?**

---

## 6. RISK ASSESSMENT

### 6.1 Technical Risks

| Risk | Impact | Probability | Key Question |
|------|--------|-------------|--------------|
| PLCs not accessible | CRITICAL | ??? | What's current integration status? |
| Attribution logic wrong | HIGH | HIGH | How is it done today? |
| Count accuracy issues | HIGH | MEDIUM | What's acceptable variance? |
| Network reliability | MEDIUM | MEDIUM | What's the infrastructure? |
| Performance impact | MEDIUM | LOW | What's the data volume? |

### 6.2 Functional Risks

| Risk | Impact | Probability | Key Question |
|------|--------|-------------|--------------|
| Wrong scope assumed | CRITICAL | HIGH | What's really needed? |
| SAP rejects confirmations | HIGH | MEDIUM | What are the validation rules? |
| Operators resist system | MEDIUM | MEDIUM | What's the change impact? |
| Reconciliation failures | HIGH | HIGH | How to handle mismatches? |

---

## 7. RECOMMENDED IMMEDIATE ACTIONS

### 7.1 Scope Clarification Workshop
**Agenda items:**
1. Walk through current manual confirmation process
2. Identify what data is available where
3. Define "automatic" - what level of automation?
4. Prioritize plants/equipment
5. Agree on phasing strategy

### 7.2 Technical Discovery
**Required assessments:**
1. PLC inventory and accessibility audit
2. Network architecture review
3. Existing integration points catalog
4. Data flow mapping
5. Order attribution logic documentation

### 7.3 Proof of Concept
**Before committing to full scope:**
1. Pick one device at Laurel
2. Prove data can be accessed
3. Test attribution logic
4. Validate SAP confirmation
5. Measure performance impact

---

## 8. COST IMPACT ANALYSIS

### 8.1 Scope-Driven Cost Variations

| Scope Element | Cost Impact | Critical Decision |
|---------------|-------------|-------------------|
| New PLC integration | +$100-200K | Use existing systems? |
| Real-time vs batch | +$50-100K | How fast is fast enough? |
| All plants Day 1 | +$150-300K | Phased rollout possible? |
| Quality tracking | +$75-150K | Just counts sufficient? |
| Equipment monitoring | +$100-200K | Separate OEE project? |

### 8.2 Hidden Costs
**Often overlooked in scoping:**
- PLC programming (customer or vendor)
- Network infrastructure changes
- Security/firewall modifications
- Training and change management
- Parallel run period
- Reconciliation tools

---

*The automatic confirmations requirement appears to assume field device integration exists or is trivial to implement. This assumption needs immediate validation as it could represent a massive scope increase if new PLC integration is required.*