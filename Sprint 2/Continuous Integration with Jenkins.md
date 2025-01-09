# Continuous Integration with Jenkins

## 1. Introduction to CI

- CI is a practice where team members integrate work frequently, verified by automated builds and tests.
- Tools for CI: Jenkins, Travis CI, Bamboo, Buildbot.
- Jenkins: Open-source CI tool written in Java.

## 2. Introduction to Jenkins

### What Jenkins Does:
- Compiles and builds code.
- Runs shell/command line scripts.
- Executes integration tests.
- Monitors tasks and stops builds on failure.
- Notifies users of build status.
- Deploys to test/production environments.

### Features of Jenkins:
- Easy to install and configure.
- Rich plugin ecosystem.
- Permanent links to builds.
- Extensible and supports distributed builds.
- File fingerprinting for dependency management.
- Email integration.

### Benefits of Jenkins:
- Automates build, test, and reporting processes.
- Reduces manual effort and speeds up delivery.

## 3. Installing and Configuring Jenkins

### Pre-Requisites:
- Java 8, adequate RAM, and disk space based on team size.
- Installable on multiple OS platforms.

### Installation:
- Download `jenkins.war` file.
- Start Jenkins via command line or Tomcat server.
- Access Jenkins at `http://localhost:8080/`.

### Configuration:
- Set home directory, concurrent job executions, custom environment variables.
- Configure email notifications, JDK, and Maven settings.

## 4. Plugin Management

- Plugins extend Jenkins functionality for tasks like source control, testing, builds, reporting, etc.
- Configure plugins via "Manage Plugins" in Jenkins.

## 5. Creating a Jenkins Project

### Setting up a Project:
- Plan SCM, build triggers, scripts, archiving, test results, and notifications.
- Create a project via "New Item" in the Jenkins dashboard.

### Configure Build Triggers:
- Options include polling SCM, periodic builds, or triggering remotely.
- Define schedules using CRON syntax.

### Adding Build Steps:
- Add actions like "Execute Windows Batch Command" or "Invoke top-level Maven targets".

### Post-Build Activities:
- Archive artifacts, deploy, record fingerprints, and send email notifications.

## 6. Executing the Build Job

- Build jobs display in the dashboard and trigger based on settings.
- View progress in "Build History" and check console output for details.
- Build status indicated via weather icons and colored balls.

## 7. CI Pipeline with Jenkins, Git, and Tomcat

### Steps to Set Up CI Pipeline:
1. Pre-requisites: Ensure Jenkins, Maven, Tomcat are running.
2. Plugins: Install "Deploy to Container" and "Git" plugins.
3. Create Jobs:
   - Pull job to detect Git changes.
   - Build job triggered by Pull job.
   - Deploy job to push results to Tomcat.

### Setting Up Deploy Job:
- Post-build actions: Deploy `*.war` file to a Tomcat container with credentials and URL.
- Jobs trigger automatically on Git changes.

## 8. Invoking Tests from Jenkins Project

### Unit Testing:
- Plugins: Use JUnit or similar.
- Configure Ant build steps and publish test results.

### Automated Testing:
- Install Hudson Selenium Plugin.
- Configure Selenium SuiteRun in the build steps.

## 9. Security Management in Jenkins

### Securing Jenkins:
- Enable security under "Configure Global Security".
- Use Jenkins' user database for authentication.
- Define roles and permissions with Matrix-based Security.

## 10. Distributed Build

### Master-Slave Architecture:
- Master handles scheduling, dispatching, monitoring, and reporting.
- Slaves execute build jobs on various OS platforms.

### Setting Up Slave Node:
- Add nodes under "Manage Jenkins" and configure IP, credentials, and launch method.

### Configuring Projects for Slaves:
- Restrict jobs to specific slave nodes using label expressions.

## 11. Jenkins Backup

- Backup Jenkins home directory manually or using backup plugins.
- Use ThinBackup for selective backups like jobs and configurations.

## 12. Best Practices

- Secure Jenkins.
- Use appropriate plugins.
- Schedule jobs effectively.
- Monitor disk space and archive data.
- Reuse generic jobs.
- Take regular backups of configurations and logs.
