# Continuous Integration with Jenkins

## 1. Introduction to CI

Continuous Integration is a software development practice where members of a team integrate their work frequently, usually each person integrates at least daily - leading to multiple integrations per day. Each integration is verified by an automated build (including test) to detect integration errors as quickly as possible.

You need a tool to create a CI-enabled environment. Jenkins, Travis CI, Bamboo, and Buildbot are different tools available to enable CI. In this course, you will be learning about Jenkins, a widely used open-source CI tool, written in Java.

## 2. Introduction to Jenkins

### What does Jenkins do:
- Compiles and builds the code
- Runs an internal shell or command line script
- Starts execution of the integration tests
- Monitors execution of tasks
- Stops build in case of failure
- Notifies users of the build status
- Deploys in test or production environments

### Features of Jenkins:
- **Easy to install**
- **Easy to configure various tasks**
- **Rich plugin ecosystem**: Integrates with a variety of build, test, deploy, and reporting tools
- **Permanent links**: Jenkins provides direct links to the latest or failed build, which can be used for easy communication
- **Extensibility**: Customize Jenkins to suit your needs
- **Distributed builds**: Jenkins can distribute build and test jobs to multiple computers with different operating systems
- **File fingerprinting**: Manages dependencies
- **Email integration**: Emails the build status

### Difference made by Jenkins:
- **Pre Jenkins**:
  - Source code was completely built and then tested
  - Bugs identified during testing in the source code had to be fixed and re-tested
  - Slows the software delivery, as the entire process is manual
- **Post Jenkins**:
  - Once a code change is committed, Jenkins automatically takes care of the build, test, and reporting of results

## 3. Installing and Configuring Jenkins

### Pre-Requisites to Install Jenkins:
- Recommended minimum configuration for installing Jenkins:
  - **Locally**: Java 8, 256MB RAM, and > 1 GB free disk space
  - **Small team**: Java 8, > 1 GB RAM, and > 50 GB free disk space
- Since all builds take place on the Jenkins machine, the system should have enough disk space for build storage.
- Jenkins can be installed on Windows, Ubuntu/Debian, Red Hat, Fedora/CentOS, macOS, and openSUSE.

### Installing Jenkins:
- Jenkins can be started from the command line or run on a web application server.
- Download the `jenkins.war` file from Jenkins.
- Start Jenkins directly from the command line with `java -jar jenkins.war`.
- On successful completion, Jenkins can be accessed locally from `http://localhost:8080/`.
- To run it from Tomcat server:
  - Put the `.war` file into the webapps directory.
  - Start Tomcat, and Jenkins installation will be available at `http://localhost:8080/jenkins`.

### Launching Jenkins:
- On the first launch of Jenkins, complete a few configuration steps:
  - Obtain a generated value, which is the super admin password.
  - Follow the instructions on the screen.
  - Install suggested plugins.
  - Set up an admin user when prompted or choose to continue with the generated value.
- Jenkins is now ready for use!

### Configuring Jenkins:
- Select the Jenkins home directory (prefer a location with enough free space).
- Decide the number of concurrent job executions allowed on the Jenkins machine.
- Add custom environment variables.
- Mention the SMTP server and user email suffix in the email notification section.
- Configure the location of JDK installation.
- To build Maven applications, configure the location of Maven Home.
- Perform these tasks by selecting **Configure System** under **Manage Jenkins**.

## 4. Plugin Management

### Extending Jenkins Functionality:
- Jenkins has relatively few abilities but aids software developers by providing a variety of plugins.
- Plugins are add-ons that allow Jenkins to interact with many other software tools.
- The exact plugins you install depend on the nature of your project.

### Plugins in Jenkins:
- **Source Control**: Git, SVN, Mercurial
- **Testing**: Selenium, Windmill
- **Triggers**: Jabber, Directory watchers
- **Artifact**: To copy components between projects like Amazon S3, SCP
- **Code Analysis**: Parse code with tools like CheckStyle, Findbugs, PMD
- **Build Tools**: For large projects, use a build manager such as Maven or Ant
- **Reporting**: Jenkins provides its own reports and can be extended using tools like Static Analysis Collector, which collects different analysis results and shows them in a combined trend graph
- Plugins can be configured via **Manage Plugins** under **Manage Jenkins**.

## 5. Creating a Jenkins Project

### Set up a Simple Project:
- Plan and configure the following sections:
  - **SCM**: Associate with a version control server
  - **Triggering Build**: Control when Jenkins performs builds (e.g., Polling, Periodic, or Build based on other projects)
  - Execution of scripts, Ant, and Maven targets
  - Archiving artifacts
  - Recording and publishing build and test results
  - Email notifications

### Create a New Project:
- Select **New Item** from the Jenkins dashboard.
- Type the project name.
- Select **Freestyle project** (freestyle is the most configurable and flexible option, easy to set up!).
- Click **OK**.

### Configure Job Notifications:
- Choose to prevent builds when upstream or downstream jobs are in the queue or building.
- Use plugins like Tikal to expand notification options (e.g., JSON and XML formats).

### Configure SCM:
- Define the source code location in the **Source Code Management** section.
- For Git, mention the repository URL.
- For GitHub, add the user ID and password under the **Add** button.

### Configure Build Triggers:
- Define triggers for builds:
  - Build whenever a SNAPSHOT dependency is built.
  - Trigger builds remotely.
  - Build after other projects are built.
  - Build periodically (CRON jobs).
  - Poll SCM (CRON jobs).

### Adding a Build Step:
- Select **Add Build Step** in the build section and choose the build option:
  - For Java builds, select **Execute Windows Batch Command**.
  - For Maven builds, select **Invoke top-level Maven targets** and set goals such as `clean package`.

### Configure Post-Build Activities:
- Jenkins provides various post-build options, such as:
  - Aggregate downstream test results.
  - Archive artifacts.
  - Deploy artifacts to a Maven repository.
  - Record fingerprints of files.
  - Send email notifications.

## 6. Executing the Build Job

### Running the Job:
- Build jobs are displayed on the Jenkins dashboard.
- Build starts automatically based on the trigger settings.
- To run manually, select **Build Now**.
- View progress in the **Build History** section.
- After completion, click the build number to see the **Console Output** for details.

### Build Job Status:
- **Weather Icon**: Displays build records on the home page dashboard.
- **Colored Ball**: Shows individual build status on the project page.
- Hover over the status for tooltips and explanations.
