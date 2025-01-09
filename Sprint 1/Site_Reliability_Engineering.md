# Site Reliability Engineering

## Your Job as a DevOp

### Definition of SRE
Site Reliability Engineering (SRE) happens when a software engineer designs an operations function. It is an automation-first approach that minimizes manual work.

**Use Case:** A startup uses SRE principles to automate server provisioning, ensuring rapid scaling without manual intervention.

### Role within DevOps
SRE is one of the three fundamental practice areas of DevOps, alongside continuous delivery and infrastructure automation. It focuses on production support, including on-call support, monitoring, SLAs, and performance planning.

**Use Case:** An e-commerce platform adopts SRE to monitor application performance during peak sales periods, ensuring SLA compliance.

### Engineering Focus
SRE emphasizes automated solutions and continuous improvement processes to operations, promoting an agile team approach and systems thinking.

**Use Case:** A media company uses SRE to automate log analysis and proactively identify system bottlenecks.

## You Aren't Google or Netflix

### Tailor SRE Practices
Examine your organization’s specific needs rather than mimicking companies like Google or Netflix.

**Use Case:** A mid-sized SaaS company customizes SRE to prioritize customer-facing uptime rather than internal systems.

### Use of Maturity Models
Assess your organization's state using a maturity model and revisit it periodically to track progress.

**Use Case:** A logistics company periodically evaluates its maturity model to improve incident response times.

### Focus on Practices Over Tools
Use existing tools where possible, and only build tools that add real value. Avoid reinventing the wheel.

**Use Case:** A fintech company integrates open-source tools for monitoring instead of building proprietary solutions.

## Release Engineering

### Automate Releases
Automate releases using tools like CloudFormation, Terraform, and Rundeck to reduce errors.

**Use Case:** A healthcare platform uses Terraform to deploy HIPAA-compliant infrastructure automatically.

### Streamlined Process
Simplify reviews and approvals, often through code reviews and pull requests.

**Use Case:** A gaming company uses GitHub Actions to automate code reviews and streamline feature deployment.

### Effective Communication
Communicate changes clearly using automated reports and feature flags.

**Use Case:** A ride-sharing app uses feature flags to gradually roll out updates, reducing the risk of outages.

## Change Management

### High-Visibility Communication
Use tools like ChatOps to share changes in common chat rooms and overlay deployment changes on dashboards.

**Use Case:** A global retailer overlays deployment status on monitoring dashboards to keep teams informed.

### Phases of Change Management
1. **Stabilize the Patient:** Freeze changes outside designated windows.
2. **Catch and Release:** Inventory hand-built systems and identify high failure rates.
3. **Establish a Repeatable Build Library:** Implement automated, tested builds.
4. **Enable Continuous Improvement:** Form a feedback loop between release and incident management.

**Use Case:** A telecom provider reduces outages by implementing automated build processes for critical systems.

## Self-Service Automation

### Empowerment through Automation
Enable individuals to perform tasks independently through automation.

**Use Case:** A marketing team automates data extraction, reducing dependency on IT teams.

### Benefits
Reduces hidden queues and increases reliability by eliminating manual operations.

**Use Case:** A cybersecurity firm uses automation to streamline threat analysis and response.

### Lifecycle of Automation
1. Create scripts for personal use.
2. Operationalize them for team use.
3. Add access control and logging for wider sharing.

**Use Case:** An educational platform evolves its automation scripts to improve course deployment.

## SLAs and SLOs

### SLAs vs. SLOs
- **SLAs:** Service contracts for performance.
- **SLOs:** Goals a service strives to meet.

**Use Case:** An ISP crafts SLAs ensuring 99.9% uptime, supported by robust monitoring systems.

### Crafting Tips
Start simple, avoid absolute language, and involve the team in discussions.

**Use Case:** A financial app uses error budgets to balance innovation and reliability.

## Incident Management

### Importance of a Playbook
A playbook for handling incidents reduces downtime.

**Use Case:** An airline creates detailed playbooks for system outages to maintain booking services.

### Incident Command System
First responders act as incident commanders, explicitly transferring roles as needed.

**Use Case:** A streaming service uses PagerDuty to assign incident command during outages.

### Tools and Communication
Use tools like PagerDuty for alert routing and Statuspage for user communication.

**Use Case:** A logistics app uses OpsGenie to coordinate cross-team responses during delivery failures.

### Metrics for Incident Response
Track MTBF, MTTA, and MTTR to measure and improve response times.

**Use Case:** A cloud provider tracks MTTR to improve data recovery processes.

## Introducing Postmortems

### Importance of Postmortems
Postmortems provide a feedback loop to improve product development and operations.

**Use Case:** A fintech company’s blameless postmortems uncover API bottlenecks.

### Blameless Postmortems
Encourage learning without fear of punishment.

**Use Case:** A SaaS company improves system stability by analyzing postmortem findings.

### Postmortem Coordinator
A designated coordinator gathers stakeholders and completes documentation.

**Use Case:** A retail platform’s coordinator ensures timely resolution of Black Friday incidents.

## The Postmortem Process

1. **Description and Timeline:** Document the incident timeline.
2. **Sources and What Went Well:** Highlight safeguards.
3. **Contributing Causes and Corrective Actions:** Identify causes and actions.
4. **Metrics and Meeting:** Finalize the postmortem within days.

**Use Case:** An IoT provider’s postmortem process improves device connectivity reliability.

## Troubleshooting

### Develop a Methodology
Use structured approaches to identify and resolve issues.

**Use Case:** A mobile app team uses logs and A/B tests to resolve user complaints.

### Define the Problem
Clearly document the issue and scope.

**Use Case:** A payment gateway narrows down transaction delays to third-party API failures.

### Experiment and Validate
Validate hypotheses using pre-existing data.

**Use Case:** A gaming server team uses logs to pinpoint high-latency regions.

### Restore Service Quickly
Focus on restoring service even without identifying the root cause.

**Use Case:** An e-commerce site restores checkout services during a database outage.

### Know Your System
Understand normal system behavior and dependencies.

**Use Case:** A healthcare app uses baselines to identify anomalies in patient data access.

## Performance Engineering

### Understand the Whole System
Monitor the entire system and tie optimizations to business goals.

**Use Case:** A financial platform reduces latency in critical trading systems.

### Use the Right Tools
Employ tools like OpenTrace, Zipkin, and New Relic for analysis.

**Use Case:** A content delivery network uses Zipkin to optimize video streaming.

### Design for Performance
Optimize designs by minimizing external calls and utilizing caching.

**Use Case:** A logistics firm improves delivery times using optimized routing algorithms.

## Capacity and Scalability

### Automation is Essential
Deploy infrastructure via automation for reliability.

**Use Case:** A cloud gaming service automates scaling during peak usage.

### Scalability vs. Capacity Planning
Short-term scaling for immediate needs; long-term for cost efficiency.

**Use Case:** A social media app plans capacity for viral event spikes.

### Testing Before Upgrading
Test infrastructure in production-like environments.

**Use Case:** A streaming service validates new server configurations in staging environments.

### Scalability Patterns
Use pooling, caching, and pre-computation to enhance performance.

**Use Case:** A travel booking site precomputes search results during high-traffic periods.

## Distributed Design

### Engineering for Failure
Design systems to handle failures gracefully.

**Use Case:** A retail site builds redundancy into payment processing systems.

### Distributed Systems
Use load balancing and redundancy to maintain reliability.

**Use Case:** A messaging app ensures uptime with distributed servers.

### Observability and Controllability
Ensure logging, metrics, and control features.

**Use Case:** A ride-sharing app uses observability tools to monitor driver and rider behavior.

## Deliberate Adversity

### Chaos Engineering
Inject failures to improve fault tolerance.

**Use Case:** A video platform runs chaos experiments to test server reliability during live streams.

### Cultural Aspect
Emphasize resilience through team experimentation.

**Use Case:** A cybersecurity team adopts chaos engineering to simulate attack scenarios.

### Chaos Experiments
Test system behavior under failure conditions.

**Use Case:** A banking app runs failure scenarios to improve transaction system robustness.
