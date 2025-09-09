# Developer Competency & Rate Reference Table
## For Project Phoenix Commercial Proposal Accuracy

---

## Document Information
**Created:** 2025-09-08  
**Purpose:** Establish realistic developer capabilities and hourly rates for accurate commercial proposals  
**Status:** Active reference for Project Phoenix proposal development  

---

## 1. DEVELOPER COMPETENCY MATRIX

### 1.1 Task Complexity Categories

**Low Complexity:**
- Simple CRUD operations
- Basic UI components
- Standard configuration tasks
- Straightforward data queries
- Basic Ignition Screen Design
- Basic PLC Integration
- Documentation Building and Testing
- Minor Commissioning and Onsite Tasks
- Basic Troubleshooting

**Medium Complexity:**
- Business logic implementation
- API integrations with error handling
- Database stored procedures with logic
- Multi-screen workflows
- Database Schema implementation
- Ignition gateway scripts for basic MES execution
- Ignition integration for basic HMI and SCADA functionality
- Basic Data structure design - UDTs, database tables, simple PLC sub routines

**High Complexity:**
- Complex business rules and validation
- Advanced error recovery and compensation logic
- Performance optimization
- Integration architecture design
- Full HMI Design and Architecture
- Full MES Design and Architecture
- Simple Database Schema Design
- Simple Database Integration and Testing Tasks
- Full Controls migration and cutover
- Full PLC Routine design and integration
- Consulting and Requirements Derivation
- ERP Integration and Design

**Very High Complexity:**
- System architecture design
- Advanced performance tuning
- Complex transaction management
- Cross-system integration orchestration
- Multi site architecture and deployment
- Multi Stage design and requirements derivation
- Custom integrations outside of normal operating mode of Ignition, Python, SQL
- Regulatory compliance frameworks for MES
- Advanced execution logic and timing for MES type functionality

### 1.2 Developer Classifications

#### Junior Developer (0-2 years experience)
- **Hourly Rate:** $115 - $165
- **Supervision Required:** High
- **Code Review:** Mandatory
- **Rework Factor:** 20-30%

#### Mid-Level Developer (2-5 years experience)
- **Hourly Rate:** $165 - $185
- **Supervision Required:** Moderate
- **Code Review:** Standard
- **Rework Factor:** 10-15%

#### Senior Developer (5-10 years experience)
- **Hourly Rate:** $185 - $205
- **Supervision Required:** Low
- **Code Review:** Peer review
- **Rework Factor:** 5-10%

#### Principal/Architect (10+ years experience)
- **Hourly Rate:** $205 - $245
- **Supervision Required:** None
- **Code Review:** Reviews others
- **Rework Factor:** <5%

---

## 2. TASK-SPECIFIC EFFORT ESTIMATES

### 2.1 Database Development

| Task Type | Junior Developer | Senior Developer | Complexity Factor |
|-----------|-----------------|------------------|------------------|
| **Simple Query/View** | 1-2 hours | 0.5-1 hour | Low |
| **Medium Stored Procedure** | 4-6 hours | 1-3 hours | Medium |
| **Complex Stored Procedure** | 8-12 hours | 3-6 hours | High |
| **Database Schema Design** | N/A (not qualified) | 4-8 hours | High |
| **Performance Optimization** | N/A (not qualified) | 6-12 hours | Very High |

### 2.2 API Integration Development

| Task Type | Junior Developer | Senior Developer | Complexity Factor |
|-----------|-----------------|------------------|------------------|
| **Simple REST API Call** | 2-3 hours | 1 hour | Low |
| **ERP API with Error Handling** | 10-12 hours | 8-10 hours | Medium |
| **Complex Multi-Step API** | 24-40 hours | 16-32 hours | High |
| **Transaction Compensation Logic** | N/A (not qualified) | 40-60 hours | Very High |
| **API Performance Optimization** | N/A (not qualified) | 32-40 hours | High |

### 2.3 Ignition Development

| Task Type | Junior Developer | Senior Developer | Complexity Factor |
|-----------|-----------------|------------------|------------------|
| **Simple Display Screen** | 6-8 hours | 3-4 hours | Low |
| **Medium Perspective Screen** | 12-16 hours | 6-8 hours | Medium |
| **Complex Dashboard** | 20-30 hours | 12-16 hours | High |
| **Gateway Script (Simple)** | 4-6 hours | 2-3 hours | Medium |
| **Gateway Script (Complex)** | 12-18 hours | 6-10 hours | High |
| **Custom Module Development** | N/A (not qualified) | 80-160 hours | Very High |

### 2.4 Testing & Quality Assurance

| Task Type | Junior Developer | Senior Developer | Complexity Factor |
|-----------|-----------------|------------------|------------------|
| **Unit Test Creation** | 2-3 hours | 1-2 hours | Low |
| **Integration Test Suite** | 8-12 hours | 4-6 hours | Medium |
| **Performance Testing** | N/A (not qualified) | 24-40 hours | High |
| **Load Testing Setup** | N/A (not qualified) | 40-80 hours | Very High |

### 2.5 Documentation

| Task Type | Junior Developer | Senior Developer | Complexity Factor |
|-----------|-----------------|------------------|------------------|
| **Code Documentation** | 1-2 hours | 0.5-1 hour | Low |
| **User Manual (per screen)** | 2-4 hours | 1-2 hours | Medium |
| **Technical Architecture Doc** | N/A (not qualified) | 12-16 hours | High |
| **System Integration Guide** | N/A (not qualified) | 24-32 hours | Very High |

---

## 3. PROJECT-SPECIFIC ADJUSTMENTS

### 3.1 Technology Stack Complexity Multipliers

**Ignition 8.3 (Event Streams Module):**
- **Multiplier:** 2.0x - 3.0x (bleeding edge technology)
- **Risk Factor:** High (limited documentation/examples)
- **Applies to:** All Event Streams related development

**SAP S4/HANA API Integration:**
- **Multiplier:** 2x - 4x (complex enterprise integration)
- **Risk Factor:** Medium (well-documented but complex)
- **Applies to:** All SAP API development

**Multi-Step Transaction Management:**
- **Multiplier:** 3x - 4x (complex error handling)
- **Risk Factor:** High (compensation logic complexity)
- **Applies to:** 4-step confirmation process

**APIM Error Handling:**
- **Multiplier:** 2x - 2.5x (limited visibility into errors)
- **Risk Factor:** High (unknown error patterns)
- **Applies to:** All APIM integration work

### 3.2 Timeline Compression Adjustments

**Compressed Timeline (November Start):**
- **Multiplier:** 1.5x - 3x (pressure, parallel development)
- **Risk Factor:** Critical (no buffer for rework)
- **Quality Impact:** May require additional review/testing time

**Parallel Development Streams:**
- **Coordination Overhead:** +15-20% for communication/integration
- **Integration Risk:** +10-15% for merging parallel work
- **Testing Overhead:** +25% due to compressed testing cycles

---

## 4. ROLE-SPECIFIC HOURLY RATES (2025)

### 4.1 Development Roles

| Role | Experience Level | Hourly Rate | Usage Guidelines |
|------|-----------------|-------------|------------------|
| **Junior Developer** | 0-2 years | $165 | Simple tasks, supervised work |
| **Mid-Level Developer** | 2-5 years | $185 | Standard development tasks |
| **Senior Developer** | 5-10 years | $195 | Complex development, mentoring |
| **Technical Lead** | 10+ years | $205 | Architecture, complex problems |

### 4.2 Specialized Roles

| Role | Experience Level | Hourly Rate | Usage Guidelines |
|------|-----------------|-------------|------------------|
| **System Architect** | 20+ years | $245| Advanced requirements derivation and design |

### 4.3 Project Management & Support

| Role | Experience Level | Hourly Rate | Usage Guidelines |
|------|-----------------|-------------|------------------|
| **Project Manager** | 5+ years | $175 | Project coordination |
| **Technical Lead** | 8+ years | $195 | Technical leadership |
| **EW Engineer** | 3+ years | $115 | Testing and validation |


---

## 5. EFFORT ESTIMATION GUIDELINES

### 5.1 Estimation Best Practices

1. **Use Most Qualified Resource:** Don't assign junior developers to high-complexity tasks
2. **Include Rework Factor:** Add 5-30% based on developer experience level
3. **Account for Integration Time:** Add 15-25% for parallel development coordination
4. **Include Code Review:** Add 10-15% for mandatory review cycles
5. **Factor Technology Risk:** Apply appropriate complexity multipliers

### 5.2 Quality Gates Impact

**Code Review Requirements:**
- Junior: 2-3 review cycles = +20-30% time
- Mid-Level: 1-2 review cycles = +10-15% time
- Senior: 1 review cycle = +5-10% time

**Testing Requirements:**
- Unit Testing: +25-40% of development time
- Integration Testing: +15-25% of development time
- Performance Testing: +10-15% of development time

### 5.3 Project Phoenix Specific Considerations

**MVP-Only Scope (November Timeline):**
- Focus on Senior/Principal developers for speed
- Minimize junior developer involvement
- Accept higher hourly rates for compressed timeline
- Include substantial coordination overhead

**Compressed Testing Cycles:**
- Automated testing essential
- Parallel testing development required
- Higher QA resource allocation needed

---

## 6. COMMERCIAL PROPOSAL RECOMMENDATIONS

### 6.1 Resource Mix for Project Phoenix



**Avoid for Compressed Timeline:**
- Heavy junior developer reliance
- Complex tasks assigned to mid-level developers
- Insufficient senior oversight
- Unrealistic effort estimates

### 6.2 Rate Blending Strategy

**Blended Rate Calculation:**
```
Weighted Average = (Senior Hours × $205) + (Mid Hours × $195) + (Specialized Hours × $245) + (Junior Hours × $165) / Total Hours

Target Blended Rate: $225/hour for Project Phoenix
```

---

## 7. VALIDATION CHECKPOINTS

Before finalizing commercial proposal:

1. ✅ **Verify task complexity assignments** - No junior developers on high-complexity tasks
2. ✅ **Validate effort estimates** - Compare against competency matrix
3. ✅ **Apply technology multipliers** - Account for bleeding-edge technology risks
4. ✅ **Include quality overhead** - Code review, testing, documentation time
5. ✅ **Factor timeline compression** - Apply appropriate pressure multipliers
6. ✅ **Validate blended rates** - Ensure realistic resource mix

---

*This reference document provides the foundation for accurate, defensible commercial proposals that reflect material reality of development capabilities and market rates.*