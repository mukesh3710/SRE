# Key Notes on Continuous Integration with Jenkins

## 1. Introduction to CI
- **Definition**: Continuous Integration (CI) involves frequent integration of code by team members, verified by automated builds and tests.
- **Tools**: Jenkins, Travis CI, Bamboo, Buildbot.
- **Focus of Course**: Jenkins, an open-source CI tool written in Java.

## 2. Introduction to Jenkins
### Jenkins Capabilities:
- Compiles and builds code.
- Executes integration tests.
- Monitors task execution.
- Stops builds on failure and notifies users.
- Deploys to test or production environments.

### Features of Jenkins:
- Easy installation and configuration.
- Rich plugin ecosystem.
- Distributed builds for multi-system support.
- File fingerprinting and email integration.

### Impact of Jenkins:
- Automates build, test, and reporting processes, reducing manual effort.

## 3. Installing and Configuring Jenkins
### Pre-Requisites:
- Minimum: Java 8, 256MB RAM, >1GB free disk space.
- For teams: Java 8, >1GB RAM, >50GB disk space.

### Installation Steps:
- Download `jenkins.war`.
- Start via command line or deploy on a web server like Tomcat.

### Configuration:
- Configure concurrent jobs, environment variables, SMTP server, JDK location, and Maven Home.
- Access configuration under **Manage Jenkins > Configure System**.

## 4. Plugin Management
- **Purpose**: Extend Jenkins functionality via plugins.
- **Categories**: Source Control (Git, SVN), Testing (Selenium), Build Tools (Maven, Ant), Reporting, etc.
- **Management**: Install and configure plugins via **Manage Plugins**.

## 5. Creating a Jenkins Project
### Steps to Set Up a Project:
1. **SCM Integration**: Define source control location.
2. **Build Triggers**: Use polling, periodic builds, or other triggers.
3. **Build Actions**: Add build steps for commands or Maven targets.
4. **Post-Build Actions**: Archive artifacts, email notifications, etc.

### CRON Syntax for Triggers:
- `* * * * *`: Every minute.
- `* 9-17 * * *`: Every minute between 9 AM and 5 PM.

### Notifications:
- Set up email notifications under project configuration.

## 6. Executing the Build Job
- **Execution**: Trigger builds manually or automatically based on settings.
- **Progress Tracking**: View build history and console output.

### Build Status Indicators:
- **Weather Icon**: Aggregated build records.
- **Colored Ball**: Status of individual builds.
