# **Day 2 – Agent Tools & Interoperability (MCP) – Summary Notes**

![License](https://img.shields.io/badge/license-MIT-blue.svg)  ![Status](https://img.shields.io/badge/status-Active-brightgreen.svg)   ![Made With](https://img.shields.io/badge/Made%20with-Markdown-blueviolet.svg)   ![Program](https://img.shields.io/badge/Google-AI%20Agents%20Intensive%202025-orange.svg)

A structured, easy-to-digest summary of **Day 2** of the Google AI Agents Intensive 2025 — breaking down how tools extend an LLM’s abilities, why MCP exists, and how modern agent systems achieve reliable perception, action, integration, and tool orchestration in the real world.

---

## 📄 Table of Contents — Day 2

- 🔧 [Why Models Alone Aren’t Enough](#why-models-alone-arent-enough)
- 🔌 [MCP](#mcp)
- 🏗️ [What Are Tools in AI Agents?](#what-are-tools-in-ai-agents)
- 🧰 [Tool Categories](#tool-categories)
- 📘 [Best Practices for Building Tools](#best-practices-for-building-tools)
- 🖧 [MCP – Architecture](#mcp--architecture)
- 🔄 [How They Communicate](#how-they-communicate)
- 🧭 [Tools in MCP](#tools-in-mcp)
- 🚨 [Error Signaling in MCP](#error-signaling-in-mcp)
- ⚙️ [Benefits to the Agent Ecosystem](#benefits-to-the-agent-ecosystem)
- 🧠 [Key Problem: Context Window Bloating](#key-problem-context-window-bloating)
- 🛡️ [Security Considerations](#security-considerations)
- ✅ [Final Takeaways](#final-takeaways)


---


## **Topic: How to get these incredibly smart language models to actually do useful things in the real world?**

*(How models go from just thinking → to actually doing is what will be covered.)*

---

## **Why Models Alone Aren’t Enough**

* Models are fundamentally stuck inside their training data and are completely isolated from anything current or anything they can act on.
* The ability to *perceive and act* is what makes Agentic AI revolutionary.
* Tools are basically the agent’s **senses and hands**.
* Historically, connecting models ↔ tools was a nightmare (n models × m tools = exponential complexity).
* The scalable solution: **Model Context Protocol (MCP).**

---

# **MCP**

* Designed as an **open standard** to streamline integration.
* Provides a **unified plug-and-play** way to connect tools.
* Decouples the **brain (reasoner)** from the **tool/actor**.

---

# **What Are Tools in AI Agents?**

A tool is a function or program an LLM-based application uses to do what the model cannot do on its own.

**Two types of tasks tools help with:**

1. **Know something new** → e.g., fetch a real-time weather API.
2. **Do something** → e.g., send email, update a database, trigger actions.

> *Know something OR do something.*

---

# **Tool Categories**

### **1. Function Tools**

* Developer-defined (Python, docstrings, etc.)
* Docstrings define the **contract**: inputs, outputs, behavior.
* Model reads docs to know *how* to call the function.

### **2. Built-In Tools**

* Provided by model providers (e.g., Google Search in Gemini).

### **3. Agent Tools**

* One agent invokes another agent as if it was a tool.
* Primary agent remains in charge and uses the sub-agent’s output.
* Like a manager requesting a report from a specialist team.

---

# **Grouping Tools by Purpose**

1. Information retrieval
2. Getting data
3. Action execution (system/API integration)
4. Connecting to other software
5. Human-in-the-loop tools - when the agent needs to stop and ask for permission or clarification from a person.


---

# **Challenges**

Designing tools is easy; **making them reliable** is the real challenge.

---

# **Best Practices for Building Tools**

1. **Documentation is paramount.**

   * Name, parameters, description → fed into model context.
   * Doc becomes the model’s instruction manual.

2. **Describe the action, not the implementation details.**

  * Tell the LLM the tool and not how to use the tool, it's about reinforcing the separation of concerns.

3. **Each tool should do one clear, high-level task.**

   * Hide system complexity.

4. **Keep tool outputs concise.**

   * Avoid context bloat → increases latency, cost, and reduces LLM reasoning.
   * Prefer summaries or URIs over raw data.

5. **Error Handling**

   * Use schema validation on inputs/outputs.
   * Errors should be descriptive *and instructive*.
   * Example: “Rate Limit Exceeded. Wait 5 seconds before retry.”

---

# **MCP – Architecture**

<img width="996" height="588" alt="image" src="https://github.com/user-attachments/assets/d0231fae-07cc-47b7-9e43-ef5597bdb162" />
(Taken from Google whitepaper)

A standardization layer inspired by the **Language Server Protocol (LSP).**

### **Three Main Components**

#### **1. MCP Host**

* Manages user experience
* Orchestrates thinking
* Decides when tools are needed
* Enforces safety policies
* Acts as the *traffic cop*

#### **2. MCP Client**

* Embedded in the host
* Maintains connection with the server
* Sends commands
* Manages session lifecycle

#### **3. MCP Server**

* Provides capabilities/tools
* Advertises available tools
* Executes commands
* Returns results

---

# **How They Communicate**

### **Communication Layer**

* Uses **JSON RPC 2.0** (standard message format).

### **Transport Layer**
<img width="998" height="592" alt="image" src="https://github.com/user-attachments/assets/13ed035e-5633-4c0f-97e9-8f1ffb322285" />
(Taken from Google whitepaper)

1. **Local Development:**

  * Often you DDO standard input output. Efficient and fast if everything's on same machine.


2. **Distributed Systems:**

* Typically use streamable HTTP.
* It is designed for remote connections.
* It supports server send events (SSE) which means the server can stream results back which is great for tools that might take a while to run. Allows more flexible deployments across a network.


---

# **Tools in MCP**

* Uses standardized **JSON Schema**.
* Each tool must include:

  * name
  * description
  * input schema
  * (optional) output schema

### **Types of Results**

1. **Structured:**

   * JSON matching the output schema
   * Preferred for reasoning

2. **Unstructured:**

   * Raw text, images, audio, URIs

---

# **Error Signaling in MCP**

Two levels:

1. **Protocol-level errors**

   * e.g., calling a tool that doesn't exist

2. **Execution-level errors**

   * Server sends result object with an `error` flag

---

# **Benefits to the Agent Ecosystem**

1. Accelerating development and fostering a reusable ecosystem. By having a standard way for tools and agents to talk, it reduces time to integrate new capabilities.
2.	Public MCP Registries: Where people can publish standardized declarations for their servers.
3.	Enables dynamic capabilities.
4.	Significant architectural flexibility by decoupling the agents core reasoning logic from the specific tool implementations. You can build more modular systems.


---

# **Key Problem: Context Window Bloating**

With thousands of tools, each with schemas and definitions, context explodes.

### **Solution: Tool Retrieval (RAG for tools)**

1. Don’t load all tools.
2. Maintain a semantic index of tool definitions.
3. Agent retrieves only **Top-K relevant tools**.
4. Only those definitions go into the context.

This keeps the reasoning context **clean, light, and efficient.**

---

# **Security Considerations**

⦁	MCP was itself designed for decentralized innovation and interoperability and it doesn't have heavy built-in enterprise security features. It's a gap in the core protocol.
⦁	Problem: 
   * Confused deputy problem: well-known vulnerability. User hijack the prompt and access the sensitive info via MCP server -> Prompt Injection leading to privilege escalation via the tool server. 


### **Solution**

 1. You must wrap the raw MCP protocol in layers of external centralized governance and security i.e. you don't expose MCP servers directly.
 2. You put something like an enterprise grade API endpoint, etc.
 3. Security is not in MCP, it's around MCP. Protocol enables the connection, but the enterprise security layers ensure it's used safely.   


---

# **Final Takeaways**

* Foundation models = powerful brains
* Tools = eyes and hands
* MCP = universal language connecting them
* Tool design best practices + external security = production-grade agents
