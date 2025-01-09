# Site Reliability Engineering: Service-Level Agreements and Objectives

## Service-Level Indicators (SLIs)

### Defining Reliability
- **Service-Level Indicator (SLI):** A metric used to measure the level of service provided, typically expressed as a ratio of good events to total events.
- **Understanding User Needs:** Identify user behaviors, critical tasks, and satisfaction drivers by defining user personas and interactions.
- **Selecting and Measuring SLIs:** Choose simple, relevant metrics like the ratio of successful HTTP requests to total requests. Continuously refine metrics for better alignment with user satisfaction and system performance.

### Implementing Measurements
- **SLI Implementation:** Use application server logs, load balancer logs, black-box monitoring, or client-side instrumentation.
- **Considerations:** Evaluate quality, coverage, and cost when selecting implementation methods.
- **Iterative Improvement:** Begin with basic measurements and iteratively improve them over time.

### Common Measurements
- **Request-Driven Systems:** Measure availability, latency, and throughput to gauge user request handling.
- **Big Data Systems:** Focus on throughput and end-to-end latency for data processing efficiency.
- **Storage Systems:** Key SLIs include latency, availability, and durability to ensure data accessibility and integrity.
- **Correctness:** Ensure all systems provide accurate data and responses.

### Measurement and Calculation
- **User-Centric SLIs:** Focus on user-critical metrics, even if challenging to measure. Collaborate with product teams to define meaningful indicators.
- **Metrics and Calculations:** Use tools like Prometheus for monitoring. Employ histograms and percentiles to analyze metrics distributions.
- **Standardization:** Develop reusable templates for common metrics to ensure consistency across teams.

## Service-Level Objectives (SLOs)

### Objectives vs. Indicators
- **Service-Level Objectives (SLOs):** Add thresholds and time windows to SLIs, enabling periodic evaluation.
- **Importance of SLOs:** Define service-performance expectations to reduce complaints and avoid under- or over-utilization.
- **Effectiveness of SLOs:** Ensure relevance to user satisfaction, defensibility, utility for decision-making, and ownership by actionable stakeholders.

### Making Measurements Meaningful
- **Turning SLIs into SLOs:** Add a threshold (e.g., "90% of requests < 500ms") and a time window (e.g., "30 days").
- **Choosing Time Windows:** Use rolling windows for real-time reflections and calendar-based windows for business alignment.

### Stakeholder Agreement
- **Buy-In:** Secure consensus from product managers, developers, and SREs.
- **Threshold Agreement:** Ensure thresholds satisfy users and are achievable without excessive manual effort.
- **Defensible SLOs:** Create achievable SLOs supported by production teams.

### Documenting SLOs
- **Centralized Documentation:** Maintain accessible, updateable documentation for all stakeholders.
- **Essential Information:** Include authors, reviewers, approvers, approval dates, service descriptions, SLI details, thresholds, and time windows.
- **Explanation and Error Budgets:** Provide rationale for SLI and SLO calculations and include error budget policies.

### Dashboards and Reporting
- **Purpose:** Offer real-time SLO performance snapshots.
- **Usefulness:** Identify trends, correlations, and problem areas.
- **Examples:** Use dashboards for SLO compliance and SLI trends.

### Continuous Improvement
- **Gathering Data:** Use support tickets, social media sentiment, user interviews, and surveys to understand satisfaction.
- **Adjusting SLOs:** Refine SLOs based on data, satisfaction levels, and workload requirements.
- **Iterative Improvement:** Regularly revisit SLOs to ensure relevance and reduce toil.

## Error Budgets

### Consequences of Poor Service Performance
- **Error Budgets:** Define acceptable failure levels over a time frame.
- **Incentives and Accountability:** Balance innovation and reliability through consequences for missing SLOs.
- **Operational Impact:** Trigger stability-focused actions, like halting releases, when budgets are depleted.

### Creating an Error Budget
- **Definition and Calculation:** Subtract the SLO threshold from one to define acceptable failure levels.
- **Stakeholder Approval:** Gain consensus from product, SRE, and engineering teams.
- **Adjustments:** Relax or tighten thresholds based on feasibility and impact.

### Drafting an Error Budget Policy
- **Enforce the Budget:** Take corrective actions when budgets are exhausted.
- **Policy Content:** Include documentation details, service descriptions, and escalation paths.
- **Action Plan:** Specify roles and responsibilities for restoring system stability.

## Service-Level Agreements (SLAs)

### Definition and Purpose
- **Service-Level Agreements (SLAs):** Contracts with users outlining SLO consequences, often with financial penalties for violations.
- **Role of Business and Legal Teams:** These teams handle SLAs, ensuring alignment with business goals and legal compliance.
- **SLA vs. SLO:** SLAs are lenient, covering fewer metrics with broader thresholds.

### Components of Agreements
- **Summary:** Overview of the product or feature and success metrics.
- **Goals:** Measurable and achievable goals aligned with user needs.
- **Consequences:** Specify financial penalties or other outcomes for SLA violations.
- **Points of Contact:** Identify responsible parties and escalation paths.
- **Conditions of Cancellation:** Define conditions for SLA invalidation.

## Real-World Use Cases

1. **E-commerce Platform:**
   - **SLI Example:** Measure successful payment transactions vs. total payment attempts.
   - **SLO Example:** 99.9% of payments processed within 1 second over 30 days.
   - **Error Budget:** Allow a 0.1% failure rate, halting new feature deployments if exceeded.
   - **SLA Example:** Refund customers for downtime exceeding 99.5% uptime.

2. **Streaming Service:**
   - **SLI Example:** Measure video playback starts without buffering.
   - **SLO Example:** 95% of videos start within 3 seconds in a rolling 7-day window.
   - **Error Budget:** Use budget breaches to prioritize stability-focused engineering.
   - **SLA Example:** Provide service credits for significant buffering issues.

3. **Cloud Storage Provider:**
   - **SLI Example:** Measure data retrieval latency and durability.
   - **SLO Example:** 99.999% durability and retrieval within 100ms for 98% of requests over 90 days.
   - **Error Budget:** Limit innovation projects if reliability dips below thresholds.
   - **SLA Example:** Financial compensation for data loss or significant delays.
