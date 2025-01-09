# Site Reliability Engineering: Service-Level Agreements and Objectives

## Overview

### Service-Level Indicators (SLIs)
Learn how to create SLIs to measure the reliability of a system and make those measurements meaningful with SLOs.

### Service-Level Objectives (SLOs)
Understand how to document SLOs, set up dashboards and reporting, and ensure continuous improvement.

### Error Budgets
Discover how to create and use error budgets to manage poor service performance and its consequences.

### Service-Level Agreements (SLAs)
Gain insights into the main components of SLAs and how to set realistic performance expectations.

---

## Prerequisites

### High-Level Understanding of Software Development and Monitoring
It is important to know how software is developed and monitored.

### Basic Understanding of Microservices or Service-Oriented Architecture
These concepts are foundational for the course.

### Understanding of Client-Server Architecture
This serves as a crucial base for the course content.

---

## Core Concepts

### 1. Service-Level Indicators (SLIs)

#### Defining Reliability
- **Service-Level Indicator (SLI):** A metric used to measure the level of service provided, typically expressed as a ratio of good events to total events.
- **Understanding User Needs:** Identify user satisfaction metrics, including who the users are, their interactions, and critical tasks.

#### Selecting and Measuring SLIs
- **Examples:** Ratio of successful HTTP requests to all HTTP requests.
- **Implementation Options:** Application server logs, load balancer logs, black-box monitoring, and client-side instrumentation.

#### Common Measurements
- **Request-Driven Systems:** Focus on availability, latency, and throughput.
- **Big Data Systems:** Emphasize throughput and end-to-end latency.
- **Storage Systems:** Key metrics include latency, availability, and durability.

#### Measurement and Calculation
- **User-Centric SLIs:** Metrics reflecting user happiness.
- **Standardization:** Use templates for consistent measurements.

#### Real-World Use Cases
- Monitoring website uptime and page load times.
- Measuring API request success rates for cloud services.

---

### 2. Service-Level Objectives (SLOs)

#### Objectives vs. Indicators
- **Service-Level Objectives (SLOs):** SLIs with thresholds and time windows.
- **Effectiveness:** Relevant to user happiness, defensible, and actionable.

#### Making Measurements Meaningful
- **Example:** 90% of API requests taking fewer than 500ms over 30 days.
- **Time Windows:** Rolling vs. calendar-based windows.

#### Stakeholder Agreement
- **Threshold Agreement:** Collaboration among stakeholders to set realistic thresholds.

#### Documenting SLOs
- **Centralized Documentation:** Include authors, service descriptions, SLI implementations, and reasoning.

#### Dashboards and Reporting
- **Purpose:** Point-in-time snapshots and trend analysis.

#### Continuous Improvement
- Adjust SLOs based on user satisfaction and system performance.

#### Real-World Use Cases
- Setting thresholds for acceptable response times for e-commerce platforms.
- Creating uptime goals for SaaS applications.

---

### 3. Error Budgets

#### Consequences of Poor Service Performance
- **Error Budgets:** Define acceptable levels of failure.
- **Incentives:** Balance innovation and reliability.

#### Creating an Error Budget
- **Calculation:** SLO threshold subtracted from 1.
- **Stakeholder Approval:** Collaboration among SREs, product engineers, and managers.

#### Drafting an Error Budget Policy
- **Policy Content:** Include action plans and escalation paths.

#### Real-World Use Cases
- Allocating downtime for system maintenance in cloud services.
- Restricting new feature releases when stability goals are not met.

---

### 4. Service-Level Agreements (SLAs)

#### Definition and Purpose
- **Service-Level Agreements (SLAs):** Contracts outlining performance expectations and consequences.

#### Components of Agreements
- **Key Elements:** Summary, goals, consequences, points of contact, and cancellation conditions.

#### Real-World Use Cases
- Negotiating uptime guarantees with enterprise clients.
- Providing financial refunds for missed availability targets in hosting services.

---

## Conclusion
By understanding and implementing SLIs, SLOs, error budgets, and SLAs, organizations can effectively measure and ensure the reliability of their services. Continuous improvement and stakeholder collaboration are key to success.
