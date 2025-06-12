# devops-er-capstone
Capstone Project for the AWS ER Training DevOps Section

## Initial Requirements

In order to run test this application, you will need to have the following:
1. A GitHub account and this repository to be cloned somewhere that you can make changes to it
2. An AWS server set up with Docker, Node, AWS CLI, eksctl/kubectl, Java, and Jenkins installed, as explained in the Activity 7 instructions for the course

Ensure that your Jenkins server has all of the necessary pipeline and GitHub plugins installed, including the following:
- Configuration as Code Plugin
- Credentials Plugin
- Credentials Binding Plugin
- Docker API Plugin
- Docker Commons Plugin
- Docker Pipeline
- Docker Plugin
- Git Plugin
- GitHub API Plugin
- GitHub Branch Source Plugin
- GitHub Integration Plugin
- Pipeline
- Pipeline Utility Steps
- Pipeline: API
- Pipeline: Basic Steps
- Pipeline: Build Step
- Pipeline: Declarative
- Pipeline: GitHub
- Pipeline: Groovy
- Pipeline: Input Step
- Pipeline: Job
- Pipeline: SCM Step
- Pipeline: Stage Step
- Pipeline: Step API
and all recommended plugins

## Initial Setup

Configuring GitHub Jenkins WebHook:
1. 

Setting up the pipeline in Jenkins:
1. Make sure initial requirements are taking care of.
2. Log into your Jenkins server as the admin user.
3. From the main Dashboard, click "New Item". Then click "Pipeline" and give the pipeline a name (e.g. full-devops-pipeline).
4. In the "General" section, give the pipeline a description and check the "GitHub Project" box. Paste `https://github.com/w-burgis/devops-er-capstone` into the "Project url" field.
5. In the "Triggers" section, select "GitHub hook trigger for GITScm polling".
6. In the "Pipeline" section, select "Pipeline script from SCM." Under "SCM" choose "Git" and then paste `https://github.com/w-burgis/devops-er-capstone` into the "Repository URL" field. For the "Branch Specifier", enter `*/main` and ensure `Jenkinsfile` is the value for "Script Path".
7. Click "Save".
