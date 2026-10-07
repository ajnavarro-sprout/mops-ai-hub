# 🌱 Sprout Social MOPs AI Hub

Welcome to the central command for Sprout Social's Marketing Operations AI ecosystem.

This repository is our team's single source of truth for everything related to artificial intelligence and automation. Instead of siloed prompts and scattered scripts, the `mops-ai-hub` acts as a shared library where our marketing agents, specialized skills, and foundational context live together. By centralizing our AI toolset, we ensure every agent understands Sprout Social's positioning, tech stack, and data models before executing a task.

### What's Inside

* **`/agents`**: Personas, system instructions, and configurations for our autonomous MOPS assistants (e.g., Lead Routing Specialist, Campaign QA Bot, Data Hygiene Analyst).
* **`/skills`**: Executable markdown files that give our agents specialized knowledge for specific Sprout Social workflows (e.g., Marketo list formatting, Salesforce campaign attribution mapping, A/B test setup).
* **`/prompts`**: A curated library of standardized, high-performing zero-shot and few-shot prompts for everyday MOPS tasks.
* **`/context`**: The foundational master files that ground our AI in Sprout Social's specific business logic, ideal customer profiles (ICPs), and marketing tech stack architecture.

### How to Use This Hub

Every skill and agent in this repository relies on shared context. When building or using a new tool, the system is designed to check our foundational `sprout-mops-context.md` file first so it understands our go-to-market motion and operational standards.

Whether you are pulling an existing skill into your local AI coding environment (like Cursor or Claude) or contributing a new workflow that saves the team hours of manual data entry, this hub is designed to scale our operations efficiently.

# What are Skills?

Skills are markdown files that give AI agents specialized knowledge and workflows for specific tasks. When you add these to your project, your agent can recognize when you're working on a marketing task and apply the right frameworks and best practices.

# What are Agents?

If a **Skill** is the step-by-step Standard Operating Procedure (SOP), the Agent is the digital team member executing it.

Agents are autonomous or semi-autonomous AI assistants configured to perform specific marketing operations functions. Instead of just answering questions like a standard ChatGPT prompt, agents have specific personas, can use tools (like reading spreadsheets or pinging APIs), and can string multiple steps together to complete a complex workflow.

Inside the **/agents** folder, you will find the system instructions and tool configurations that bring these assistants to life and govern how they behave.

**How they work together:** You assign a task to an **Agent**. The Agent reads the foundational **Context**, picks up the right **Skill** for the job, and executes the work.
