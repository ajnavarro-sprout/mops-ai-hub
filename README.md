# 🌱 Sprout Social MOPs AI Hub

Welcome to the central command for Sprout Social's Marketing Operations AI ecosystem. The shared library for everything Sprout Social's Marketing Operations team builds with AI: agents, skills, prompts, and context.

### Why this Exists

AI work tends to end up in silos: a prompt in someone's notes, a skill on one laptop, an agent only its builder knows how to run. This hub fixes that.

* **`Nothing lives in a silo.`**: Personas, system instructions, and configurations for our autonomous MOPS assistants (e.g., Lead Routing Specialist, Campaign QA Bot, Data Hygiene Analyst).
* **`One place to download from.`**: Executable markdown files that give our agents specialized knowledge for specific Sprout Social workflows (e.g., Marketo list formatting, Salesforce campaign attribution mapping, A/B test setup).
* **`Updates are managed, not emailed.`**: A curated library of standardized, high-performing zero-shot and few-shot prompts for everyday MOPS tasks.

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

**Agents** are autonomous or semi-autonomous AI assistants configured to perform specific marketing operations functions. Instead of just answering questions like a standard Gemini or Claude prompt, agents have specific personas, can use tools (like reading spreadsheets or pinging APIs), and can string multiple steps together to complete a complex workflow.

Inside the **/agents** folder, you will find the system instructions and tool configurations that bring these assistants to life and govern how they behave.

**How they work together:** You assign a task to an **Agent**. The Agent reads the foundational **Context**, picks up the right **Skill** for the job, and executes the work.
