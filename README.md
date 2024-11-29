# AI Assisted Development 

## **Description**

Experience the transformative power of AI with the "AI Assisted Development Demo with OpenShift AI." This demonstration leverages the robust capabilities of Red Hat OpenShift AI on the stable, secure OpenShift Container Platform. Dive into advanced AI capability and see how they can enhance the software development process.

The goal of this demo is to demonstrate how to use AI to improve software development efficiency. It deploys pre-trained Mistral LLM on Red Hat OpenShift AI which exposes a secured restful api for code generation, code merge issue suggestion and software development knowledge Chatbot. It deploys Open WebUI for Chatbot frontend, Continue IDE plugin for code generation entry. GitLab and SonarQube with plugins are also deployed for merge request code Quality scanning triggered by GitLab CI, adding SonarQube analysis report to GitLab code merge comment automatically. A python-based AI agent ai-suggestion is deployed to invoke LLM for code change suggestion and commenting to GitLab code merge thread. it deploys GitLab Reviewer a portal where you can see all GitLab merge requests of your teams in one place, vote merge and quick link to CI and MR pages.

## **Key Components of the Demo**

- Red Hat OpenShift AI for LLM
- Mistral LLM on Red Hat OpenShift AI
- GitLab and SonarQube with plugins
- GitLab Reviewer portal
- Continue IDE plugin
- Open WebUI Chatbot
- AI suggestion agent

## **Special Features**

- **AI suggestion**: Harness the power of AI agent linking traditional software developerment tools to explore advanced AI functionalities.
- **GPU-Enhanced Performance**: You can directly utilize configured NVIDIA GPUs for computationally intensive tasks in LLM model serving.

## **Provisioning and Access**

- **Environment Setup**: Configured on AWS with a combination of NVIDIA GPU and g6 worker nodes.
- **Access  Details**: Cluster admin and Red Hat OpenShift AI portal access credentials will be provided via email after the demo environment is provisioned.
- **Setup Time**: Approximately 1 hour and 30 minutes.

## **Additional Resources**

- **Reference Document**: [Tutorials](https://github.com/rh-demos/ai-assisted-development/blob/main/README.md)

 **This demo is not just a showcase but a hands-on experience that provides insights into the potential development tools work together with Generative AI on Red Hat OpenShift AI, supported by powerful GPU technology.**

## Tutorials

[0 - Environment](0-environment.md)

[1 - Deploy LLM](1-deploy-llm.md)

[2 - Configure open-webui](2-configure-open-webui.md)

[3 - Configure continue](3-configure-continue.md)

[4 - Configure Code Tools](4-0-configure-code-tools.md)

​	[4.1 - Configure Gitlab](4-1-configure-gitlab.md)

​	[4.2 - Configure SonarQube](4-2-configure-sonarqube.md)

​	[4.3 - Configure Gitlab Reviewer](4-3-configure-gitlab-reviewer.md)

​	[4.4 - Configure AI Agent](4-4-configure-ai-agent.md)

​	[4.5 - Configure Gitlab CI](4-5-configure-gitlab-ci.md)

[5 - Verify Code Intelligence](5-verify-code-intelligence.md)

[6- Note](6-note.md)

