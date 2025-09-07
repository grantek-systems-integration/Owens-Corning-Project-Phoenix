# RFI SECTION: SAP S4/HANA INTEGRATION - DETAILED REQUIREMENTS
## Critical Architecture Decisions & Risk Assessment

---

## 1. KAFKA EVENT STREAMS ARCHITECTURE

### 1.1 Ignition 8.3 Event Streams Module Dependencies
**CRITICAL RISK ASSESSMENT REQUIRED**

Given that the Event Streams module is exclusive to Ignition 8.3 (released Q4 2024), please address:

- [ ] **Production Readiness**: Has Owens Corning validated the Event Streams module in any production environment?
- [ ] **Risk Mitigation**: What is the fallback strategy if Event Streams module exhibits instability?
- [ ] **Version Commitment**: Is OC mandating Ignition 8.3 despite its recent release and limited field deployment history?
- [ ] **Support Model**: What level of support is Inductive Automation providing for this new module?
- [ ] **Performance Benchmarks**: Have load tests been conducted with expected message volumes?

### 1.2 Kafka Infrastructure Details
Please provide comprehensive Kafka architecture information:

**Kafka Cluster Configuration:**
- [ ] Kafka version and deployment model (on-premise, cloud, hybrid)
- [ ] Number of brokers and replication factor
- [ ] Expected topic structure for Production Orders
- [ ] Message retention policies and disk space allocation
- [ ] Zookeeper or KRaft configuration
- [ ] Security model (SASL, SSL, ACLs)

**Message Flow Specifications:**
- [ ] Expected message volume (messages/second per site)
- [ ] Peak load scenarios (shift changes, month-end)
- [ ] Message size estimates (average and maximum)
- [ ] Guaranteed delivery requirements (at-least-once, exactly-once)
- [ ] Ordering guarantees required

**Integration Points:**
- [ ] Who publishes to Kafka from S4/HANA? (SAP PO, middleware, custom)
- [ ] Message format (JSON, Avro, Protobuf)
- [ ] Schema registry implementation
- [ ] Consumer group strategy for Ignition instances

### 1.3 Alternative Integration Paths
**If Kafka/Event Streams is not viable, what are the approved alternatives?**

- [ ] Direct REST API integration to S4/HANA
- [ ] Traditional message queue (RabbitMQ, ActiveMQ)
- [ ] File-based integration
- [ ] SAP PI/PO middleware
- [ ] Azure Service Bus or Event Hub

---

## 2. EXISTING INFRASTRUCTURE ASSESSMENT

### 2.1 Current Masonite/Doors Division Infrastructure
**Critical Questions for Site Readiness:**

- [ ] **Current Ignition Deployment**: What version of Ignition (if any) exists at the four Doors sites?
- [ ] **Migration Path**: If sites have existing Ignition, what is the upgrade/migration strategy?
- [ ] **Coexistence Requirements**: Must new system coexist with legacy systems during transition?
- [ ] **Data Migration**: What historical data must be preserved from existing systems?

### 2.2 Owens Corning Advanced Manufacturing Stack (AMS)
**Infrastructure Compliance Requirements:**

Please clarify the AMS architecture and Grantek's responsibilities:

- [ ] **AMS Components**: Provide detailed AMS architecture documentation including:
  - Site Critical Servers specifications
  - Site MES Servers requirements
  - Network segmentation standards
  - Security zones and DMZ requirements
  
- [ ] **Grantek Scope**: Explicitly define what infrastructure Grantek is responsible for:
  - [ ] Ignition Gateway servers (specification, procurement, installation)
  - [ ] Kafka infrastructure setup and configuration
  - [ ] Network configuration and firewall rules
  - [ ] Database servers (PostgreSQL, SQL Server, other)
  - [ ] Redundancy setup (active/passive, active/active)
  - [ ] Backup infrastructure
  - [ ] Development and test environments

- [ ] **OC Provided Infrastructure**: What will Owens Corning provide:
  - [ ] Virtual or physical servers
  - [ ] Network connectivity to S4/HANA
  - [ ] Kafka cluster access
  - [ ] VPN/Remote access
  - [ ] Active Directory/LDAP integration

### 2.3 World Headquarters Integration
**Clarify HQ Backend/Frontend Connectivity:**

- [ ] **Communication Protocol**: How do site systems communicate with World HQ?
- [ ] **Latency Requirements**: Maximum acceptable latency for HQ communication
- [ ] **Bandwidth Allocation**: Dedicated bandwidth for site-to-HQ traffic?
- [ ] **Failover Procedures**: What happens when HQ connection is lost?

---

## 3. API INTEGRATION ARCHITECTURE

### 3.1 S4/HANA API Gateway
**Technical Implementation Details:**

- [ ] **API Management Platform**: Is Azure APIM confirmed, or are alternatives considered?
- [ ] **Authentication Method**: OAuth 2.0, API keys, certificates, or SAML?
- [ ] **Rate Limiting**: API call limits per minute/hour/day?
- [ ] **Throttling Strategy**: How to handle rate limit exceeded scenarios?
- [ ] **API Versioning**: How will API version changes be managed?

### 3.2 The Four-Step Confirmation Process
**Detailed Error Handling Requirements:**

Given OC's "Clean Core" principle placing all error handling on Ignition, provide detailed requirements for each step:

**Step 1: Production Order Confirmation**
- [ ] Retry strategy for failed confirmations
- [ ] Partial confirmation handling
- [ ] Duplicate confirmation prevention
- [ ] Timeout values and handling

**Step 2: HU Creation**
- [ ] HU number generation logic
- [ ] Failed HU creation rollback procedures
- [ ] HU status tracking requirements

**Step 3: Pack HU onto Inbound Delivery**
- [ ] Validation before packing
- [ ] Unpacking/correction procedures
- [ ] Multiple HU handling

**Step 4: Post Goods Receipt**
- [ ] GR reversal procedures
- [ ] Quantity mismatch handling
- [ ] Quality status management

### 3.3 Transaction Integrity
**Critical Data Consistency Requirements:**

- [ ] **Transaction Boundaries**: How to ensure all 4 steps complete or all rollback?
- [ ] **Compensation Logic**: Detailed requirements for reversing partial transactions
- [ ] **Audit Trail**: What level of logging is required for compliance?
- [ ] **Recovery Procedures**: How to recover from mid-transaction failures?
- [ ] **Idempotency**: How to handle duplicate messages/confirmations?

---

## 4. OFFLINE OPERATION & CACHING

### 4.1 Local Cache Requirements
**Define Scope of Offline Functionality:**

- [ ] **Cache Duration**: How long must sites operate without S4/HANA connection?
- [ ] **Data Volume**: Storage requirements for cached production orders
- [ ] **Cache Technology**: Database, in-memory, file-based?
- [ ] **Selective Sync**: Which data elements must be cached vs. optional?

### 4.2 Synchronization Strategy
**Data Reconciliation After Outage:**

- [ ] **Conflict Resolution**: How to handle conflicts when connection restored?
- [ ] **Priority Rules**: Which transactions take precedence (local vs. S4)?
- [ ] **Batch Processing**: Maximum batch size for catch-up synchronization
- [ ] **Validation Requirements**: Pre-sync validation procedures

---

## 5. PERFORMANCE & SCALABILITY

### 5.1 Load Requirements
**Site-Specific Volume Estimates:**

For each site (Sacopan, Laurel, Wahpeton, Verdi), provide:
- [ ] Production orders per day (average/peak)
- [ ] Confirmations per hour (average/peak)
- [ ] Label printing volume per shift
- [ ] Concurrent user counts
- [ ] Response time requirements (API calls, UI updates)

### 5.2 Infrastructure Sizing
**Hardware/Software Requirements:**

- [ ] Minimum Ignition gateway specifications
- [ ] Database sizing requirements
- [ ] Network bandwidth requirements (site to S4/HANA)
- [ ] Storage requirements for 90-day data retention

---

## 6. TESTING & VALIDATION

### 6.1 Integration Testing Environment
**Test Environment Requirements:**

- [ ] Will OC provide S4/HANA test instance?
- [ ] Test Kafka cluster availability
- [ ] Test data requirements (volume and variety)
- [ ] Performance testing tools and criteria
- [ ] User acceptance testing procedures

### 6.2 Rollback Strategy
**Risk Mitigation Planning:**

- [ ] Rollback procedures if go-live fails
- [ ] Parallel run requirements
- [ ] Data backup and recovery procedures
- [ ] Maximum acceptable downtime

---

## 7. SPECIFIC CLARIFICATIONS NEEDED

### 7.1 Architecture Decisions
**Please provide definitive answers to:**

1. **Is Ignition 8.3 with Event Streams module a firm requirement, or are alternatives acceptable?**

2. **Has Owens Corning successfully deployed Kafka+Ignition 8.3 Event Streams in any production environment?**

3. **Is Grantek responsible for the complete Kafka infrastructure, or will OC provide Kafka as a service?**

4. **Do the acquired Masonite sites follow OC's Advanced Manufacturing Stack standards?**

5. **What is the fallback plan if Event Streams module is not production-ready by December 2025?**

### 7.2 Contractual Boundaries
**Explicitly define Grantek vs. OC responsibilities:**

- [ ] Infrastructure procurement and setup
- [ ] Kafka cluster administration
- [ ] Network configuration
- [ ] Security implementation
- [ ] Performance tuning
- [ ] Ongoing support model

---

## 8. RISK REGISTER

Please acknowledge and provide mitigation strategies for:

| Risk | Impact | Mitigation Strategy Required |
|------|--------|------------------------------|
| Ignition 8.3 Event Streams instability | Critical | Alternative integration path |
| Kafka infrastructure complexity | High | Simplified architecture option |
| "Clean Core" error handling complexity | High | Detailed error handling patterns |
| Legacy Masonite system incompatibility | Medium | Phased migration approach |
| Network latency to S4/HANA | Medium | Local caching strategy |
| API rate limiting | Medium | Batch processing approach |

---

*This section requires detailed technical responses to proceed with accurate solution design and risk assessment.*