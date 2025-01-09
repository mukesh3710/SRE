# Site Reliability Engineering

## Your Job as a DevOp
### Definition of SRE
Site Reliability Engineering (SRE) is an automation-first approach where software engineers design operational functions, reducing manual tasks.
- **Use Case**: Automating routine server maintenance tasks.

### Role within DevOps
SRE bridges operations and development with a focus on production support, monitoring, SLAs, and performance planning.
- **Use Case**: Managing on-call rotations and incident escalation for seamless operations.

### Engineering Focus
SRE drives automation and continuous improvement, promoting agile teamwork and system thinking.
- **Use Case**: Implementing self-healing infrastructure to automatically resolve common errors.

---

## You Aren't Google or Netflix
### Tailor SRE Practices
Adapt SRE principles to your organization's unique needs instead of copying large-scale companies.
- **Use Case**: Creating custom alert thresholds based on your traffic patterns.

### Use of Maturity Models
Assess your SRE maturity with tools and revisit them to track progress.
- **Use Case**: Periodically reviewing and updating SLAs to align with business growth.

### Focus on Practices Over Tools
Prioritize proven practices and only develop tools that add significant value.
- **Use Case**: Leveraging open-source monitoring tools instead of building custom dashboards.

---

## Release Engineering
### Automate Releases
Use tools like Terraform or Rundeck to reduce errors and improve reliability.
- **Use Case**: Automating deployment pipelines to multiple environments.

### Streamlined Process
Simplify reviews and approvals using pull requests and clear processes.
- **Use Case**: Peer-reviewing code changes for production readiness.

### Effective Communication
Clearly communicate changes to stakeholders using feature flags and reports.
- **Use Case**: Sending automated notifications for deployed features.

---

## Change Management
### High-Visibility Communication
Keep teams informed via tools like ChatOps and deployment dashboards.
- **Use Case**: Notifying teams of service downtimes and updates in a shared chat channel.

### Phases of Change Management
1. **Stabilize the Patient**: Restrict changes to scheduled windows.
   - **Use Case**: Implementing a change freeze during peak hours.
2. **Catch and Release**: Inventory high-risk systems.
   - **Use Case**: Identifying legacy systems prone to failure.
3. **Repeatable Build Library**: Automate builds for critical systems.
   - **Use Case**: Using CI/CD pipelines for consistent application builds.
4. **Continuous Improvement**: Integrate feedback from incidents into processes.
   - **Use Case**: Regularly updating playbooks based on postmortems.

---

## Self-Service Automation
### Empowerment through Automation
Enable teams to execute tasks independently, reducing silos.
- **Use Case**: Allowing developers to spin up test environments on demand.

### Benefits
Lower work in progress (WIP) and improve reliability.
- **Use Case**: Replacing manual database updates with automated scripts.

### Lifecycle of Automation
Start with personal scripts, then operationalize and share with teams.
- **Use Case**: Creating shared libraries for common deployment tasks.

---

## SLAs and SLOs
### Definitions
- **SLAs**: Commitments on service performance.
  - **Use Case**: Guaranteeing 99.9% uptime to customers.
- **SLOs**: Goals to measure success internally.
  - **Use Case**: Ensuring API response times remain under 200ms.

### Metrics
Carefully choose metrics aligned with business goals.
- **Use Case**: Using error budgets to prioritize reliability improvements.

---

## Incident Management
### Importance of a Playbook
Reduce downtime with detailed incident response guides.
- **Use Case**: Using predefined steps to handle database outages.

### Incident Command System
Define clear roles for incident management.
- **Use Case**: Assigning an incident commander during major outages.

### Tools and Communication
Use PagerDuty and Statuspage for alerting and communicating.
- **Use Case**: Notifying customers about ongoing incidents through a status page.

### Metrics for Response
Measure MTBF, MTTA, and MTTR.
- **Use Case**: Analyzing mean time to resolve incidents for process improvements.

---

## Introducing Postmortems
### Importance
Learn from failures to improve future resilience.
- **Use Case**: Identifying gaps in monitoring after a major incident.

### Blameless Postmortems
Encourage open sharing without fear of punishment.
- **Use Case**: Discussing human errors to implement safeguards.

---

## The Postmortem Process
1. **Description and Timeline**: Document the incident.
   - **Use Case**: Creating a detailed timeline of events.
2. **What Went Well**: Highlight safeguards that worked.
   - **Use Case**: Recognizing the impact of automated rollbacks.
3. **Corrective Actions**: Specify improvements.
   - **Use Case**: Adding missing alerts to monitoring systems.

---

## Troubleshooting
### Develop a Methodology
Use a structured approach to problem-solving.
- **Use Case**: Analyzing logs to trace API failures.

### Restore Service Quickly
Focus on restoring service even if the root cause isn’t clear.
- **Use Case**: Restarting services to restore availability.

---

## Performance Engineering
### Understand the Whole System
Monitor and optimize for end-to-end performance.
- **Use Case**: Identifying bottlenecks in database queries.

### Use the Right Tools
Employ tools like New Relic for performance insights.
- **Use Case**: Monitoring application response times during peak hours.

---

## Capacity and Scalability
### Automation is Essential
Automate infrastructure provisioning.
- **Use Case**: Scaling resources during traffic spikes with Terraform.

### Scalability Patterns
Implement caching and pre-computed content.
- **Use Case**: Using a CDN to cache frequently accessed files.

---

## Distributed Design
### Engineering for Failure
Design systems to handle potential failures.
- **Use Case**: Implementing failover for critical services.

### Observability and Controllability
Ensure systems are observable and manageable.
- **Use Case**: Using metrics and logs for debugging distributed services.

---

## Deliberate Adversity
### Chaos Engineering
Inject failures to test system resilience.
- **Use Case**: Simulating server crashes to verify recovery mechanisms.

### Chaos Experiments
Test hypotheses about failure conditions.
- **Use Case**: Verifying redundancy by disabling primary services temporarily.
