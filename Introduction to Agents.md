# 🤖 Google AI Agents Intensive 2025 — Whitepaper Summary Notes

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Status](https://img.shields.io/badge/status-Active-success.svg)
![Made with](https://img.shields.io/badge/Made%20with-Markdown-informational)
![Program](https://img.shields.io/badge/Google-AI%20Agents%20Intensive%202025-orange)

A structured, easy-to-digest summary of the **Google AI Agents Intensive 2025** whitepapers — capturing the anatomy, taxonomy, orchestration, memory, reliability, and evolution of AI agents.

---

## 📑 Table of Contents
1. [🧩 Agent Anatomy](#-agent-anatomy)
2. [🔁 Reasoning Strategy](#-reasoning-strategy)
3. [🧭 Taxonomy — Levels of Agent Capability](#-taxonomy--levels-of-agent-capability)
4. [🏗️ Building Beyond Level 2](#️-building-beyond-level-2--making-agents-reliable-in-production)
5. [⚙️ Orchestration Layer](#️-back-to-the-orchestration-layer)
6. [🧪 Testing & Debugging — AgentOps](#-testing--debugging--agentops)
7. [🔐 Security & Scaling](#-security--scaling)
8. [📈 Learning & Evolution](#-learning--evolution)
9. [🧬 Simulation & Agent Gym](#-simulation--agent-gym)
10. [📚 Resources](#-resources)
11. [✅ End of Summary](#-end-of-summary)

---

## 🧩 Agent Anatomy  
**Agent Anatomy** can be divided into **3 parts:**  
**Model**, **Tools**, **Orchestration Layer**

---

### **1. MODEL**
- **Managing context window** → How: Information comes from **memory** and **tools**, but it’s the model that curates it.  
- It decides **what inputs** are needed for the next thought process.  
- The **model thinks**, but it needs tools to actually do anything.  
- The model **reasons** about which tool it needs at each layer.  
- The model is continuously in a loop:  
  **thinking → acting → observing → thinking again.**

---

### **2. TOOLS**
- Tools are **connections to the outside world** or even to the **internal system**.

---

### **3. ORCHESTRATION LAYER**
- The **orchestration layer** actually calls the tool.  
- The **result from the tool** is directly fed into the model’s context for the next thought process.  
- The orchestration layer is the **governor of the whole process** — it manages the **operational loop**.

---

## 🔁 Reasoning Strategy  
- **Chain of Thought**, **ReAct**

### **Loop:**
1. **Get the mission/goal**  
2. **Scan the scene** – look around, what tools are available, what’s relevant in memory  
3. **Think it through** – reasoning step  
4. **Take action** – orchestration layer calls the tool  
5. **Observe and iterate** until the overall mission is complete

---

## 🧭 Taxonomy — Levels of Agent Capability  

It is important to decide **how complex** the agent needs to be.

| Level | Capability | Description |
|-------|-------------|-------------|
| **0** | **Baseline** | Just the LLM itself |
| **1** | **Connected Problem Solver** | Tools are connected; this level connects the model to the world |
| **2** | **Strategic Problem Solver** | Handles multi-part goals; key skill is **context engineering** — crafting inputs smartly for each step |
| **3** | **Collaborative Multi-Agent System** | Agents treat other agents as tools (e.g., Project Manager agent). Difference between calling a complex function and calling an agent → agent has its own iteration process and plan. |
| **4** | **Self-Evolving System** | The system finds gaps in its own capabilities. It knows what it can’t do and takes steps to fix it. <br>**Example:** Project Manager agent realizes it needs real-time social media sentiment analysis but no tool exists → it invokes an **Agent Creator Tool** to build a new one on the fly. It adapts and expands its toolkit. |

---

## 🏗️ Building Beyond Level 2 — Making Agents Reliable in Production  

1. **Model Selection**  
   - Move beyond generic benchmark scores.  
   - Choose models with best **reasoning** and **tool-use reliability** for the task.  
   - Leads to **Model Routing**.

2. **Model Routing**  
   - Send different tasks to different models.  
   - Use **faster, cheaper models** for simpler tasks and **advanced ones** for complex reasoning.  
   - Goal: **Smart Resource Allocation.**

3. **Retrieval and Action**  
   - For **retrieval**, grounding in facts is key.  
   - Use **RAG** (with vector DB) for unstructured data search or **NL2SQL tools** for structured DB queries.  
   - **Action** → done via APIs wrapped as tools (e.g., calendar API calls).

4. **Function Calling**  
   - Models must know **how to call tools correctly**.  
   - Each tool must have **clear descriptions** (like OpenAPI spec).  
   - This defines what the tool does, what parameters it needs, and what output format it returns.

---

## ⚙️ Back to the Orchestration Layer  

1. **Defines the agent’s persona**, boundaries, and operating rules — usually through **system prompts** or a **constitution**.  
2. **Manages Memory**  
   - **Short-Term Memory** → scratchpad for current tasks, includes running history of action-observation pairs.  
   - **Long-Term Memory** → persists across sessions, remembers preferences, past interactions, or learned knowledge.  
   - Architecturally, this is often implemented as a **tool**, typically a **RAG system** connected to a **vector database** where memories are stored and retrieved.

---

## 🧪 Testing & Debugging — AgentOps  

1. **Evaluating Quality** → *Agent-as-Judge* technique  
2. **Debugging** → Use **observability tools** like **OpenTelemetry traces**.  
   - Traces provide a **step-by-step log** of the agent’s entire thought process (its trajectory).  
3. **Human Feedback** →  
   - User reports a failure → reproduce it as a **permanent test case** in the **golden dataset**.

---

## 🔐 Security & Scaling  

1. **Defense in Depth** — multi-layered security  
   - Hard-coded guardrails via **policy engines** (e.g., blocking API calls that exceed spending limits).  
2. **AI-based Guard Models** — *AI checking AI.*  
3. **Agent Identity** — each agent has a **secure, verifiable digital identity** determining what it’s allowed to access.  
4. **Agent Sprawl Management** —  
   - With many agents, governance becomes key.  
   - Use a **central control plane or gateway** as the single point of control.

---

## 📈 Learning & Evolution  

Agents **learn and evolve** from:  
- Runtime experiences  
- Logs and traces  
- User feedback  
- External signals (e.g., updated company policies)

---

## 🧬 Simulation & Agent Gym  

- A **safe, off-production environment** for simulating interactions without affecting real users.  
- Used to **train, test, and evaluate** agent behavior before deployment.

---

## 📚 Resources  

Here are the reference whitepapers and learning materials from the **Google AI Agents Intensive 2025** program:  

| Resource | Description |
|-----------|-------------|
| [📄 Whitepaper 1: Foundations of Agentic Systems](https://www.kaggle.com/whitepaper-introduction-to-agents) | Covers the anatomy, reasoning strategies, and orchestration principles. |
| [📄 Whitepaper 2: Reliable & Scalable Agents](#) | Discusses production-level reliability, Model Routing, and testing methodologies. |
| [📄 Whitepaper 3: Memory, Evolution, and Simulation](#) | Explores long-term memory, adaptive learning, and simulation-based training for agents. |
| [🌐 Google AI Agents Intensive Program Page](#) | Official program overview and resources. |

(Whitepaper 1 link is available here)
---

## ✅ End of Summary  
*Google AI Agents Intensive 2025 — Whitepaper Summary Notes*
