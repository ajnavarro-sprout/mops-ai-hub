# 🌱 Sprout Social MOPS AI Hub

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

# What are skills?
***
Skills are markdown files that give AI agents specialized knowledge and workflows for specific tasks. When you add these to your project, your agent can recognize when you're working on a marketing task and apply the right frameworks and best practices.
