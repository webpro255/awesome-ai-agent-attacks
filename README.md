# Awesome AI Agent Attacks [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

A curated timeline of real AI agent security incidents, breaches, and vulnerabilities from 2024-2026. Every entry includes date, named company/product, specific impact, root cause, CVE where applicable, and source links.

No opinions. No product pitches. Just facts with sources.

Last updated: 2026-07-13

---

## Contents

- [2026 Incidents](#2026-incidents)
- [2025 Incidents](#2025-incidents)
- [2024 Incidents](#2024-incidents)
- [Key Statistics](#key-statistics)
- [Attack Pattern Taxonomy](#attack-pattern-taxonomy)
- [Contributing](#contributing)

---

## 2026 Incidents

### 2026-07-13 - Orca Security 2026 State of AI Security Report

- **Target:** Cloud AI deployments across hundreds of thousands of scanned enterprise environments
- **Impact:** The report finds AI security debt piling up. 99.9% of AI-related vulnerability alerts that have an available fix remain unpatched; 81.2% of companies running AI packages carry at least one known vulnerability and 74.1% at least one critical CVE; 56% of AI adopters have pushed agent frameworks to production; 64% run vector databases (RAG users average 3.78 of them); and about 30% store at least one AI key insecurely
- **Root Cause:** Rapid AI adoption outpacing patching, key hygiene, and encryption, with agent frameworks and vector stores deployed faster than they are secured
- **Sources:** [Help Net Security](https://www.helpnetsecurity.com/2026/07/13/ai-infrastructure-security-risks-report/)

### 2026-07-11 - "Ghostcommit" Hides Prompt Injection Inside PNG Images to Steal Secrets

- **Target:** AI code-review agents Cursor and Google Antigravity (backed by Claude Sonnet, Gemini, and GPT-5.5); Anthropic's Claude Code refused across all tested models
- **Impact:** Instructions hidden as readable text inside a PNG referenced by an `AGENTS.md` convention file drive an agent to read a project `.env` byte by byte and emit the secrets as a tuple of integer constants disguised as ordinary code, slipping past secret scanners. One run leaked an entire `.env` as a 311-integer constant containing API keys, database URLs, and cloud credentials. A survey found 73% of merged pull requests across 300 top repositories reached the default branch with no substantive human or bot review
- **Root Cause:** AI code reviewers treat image files as binary blobs and exclude them from analysis, so a prompt injection carried inside an image is never inspected, while encoding stolen secrets as integers evades string-pattern secret detection. Found by Sudipta Chattopadhyay and Murali Ediga (University of Missouri-Kansas City ASSET Research Group)
- **Sources:** [BleepingComputer](https://www.bleepingcomputer.com/news/security/ghostcommit-hides-prompt-injection-in-images-to-fool-ai-agents-steal-secrets/), [Cybersecurity News](https://cybersecuritynews.com/ghostcommit-attack-hides-prompts/)

### 2026-07-10 - Wave of Critical RCE Flaws Across AI Agent Frameworks (CVE-2026-61447, CVE-2026-54769, CVE-2026-57572, CVE-2026-59726)

- **Target:** PraisonAI before 1.6.78; Langroid before 0.65.2; Crawl4AI before 0.9.0; Ruflo before 3.16.3
- **Impact:** Four separate critical remote code execution paths were disclosed the same week. PraisonAI's CodeAgent runs LLM-generated Python with no AST validation or sandbox (CVE-2026-61447); Langroid escapes its evaluation sandbox in `TableChatAgent`/`VectorStore` when `full_eval=True` (CVE-2026-54769); Crawl4AI's Docker API accepts attacker-supplied Chromium arguments for unauthenticated RCE (CVE-2026-57572); and Ruflo, a meta-harness for Claude Code and Codex, exposes an unauthenticated MCP bridge whose `terminal_execute` tool grants shell access and provider-key theft (CVE-2026-59726)
- **Root Cause:** LLM-generated code, SQL, or shell arguments executed without validation or isolation, plus MCP and dashboard endpoints exposed with no authentication
- **CVE:** CVE-2026-61447, CVE-2026-54769, CVE-2026-57572, CVE-2026-59726 (all CVSS 10.0)
- **Sources:** [NVD CVE-2026-61447](https://nvd.nist.gov/vuln/detail/CVE-2026-61447), [NVD CVE-2026-54769](https://nvd.nist.gov/vuln/detail/CVE-2026-54769), [NVD CVE-2026-57572](https://nvd.nist.gov/vuln/detail/CVE-2026-57572), [NVD CVE-2026-59726](https://nvd.nist.gov/vuln/detail/CVE-2026-59726)

### 2026-07-09 - Open WebUI 16-Flaw Security Batch (CVE-2026-59212 to CVE-2026-59227)

- **Target:** Open WebUI (self-hosted LLM interface) before 0.10.0
- **Impact:** Sixteen flaws fixed at once. The most severe include identity spoofing on the terminal WebSocket via an unencoded `session_id` (CVE-2026-59224, CVSS 8.0), running code in another user's session through client-supplied session IDs on `execute:python`/`execute:tool` events (CVE-2026-59216), a nine-times percent-encoded path-traversal bypass of an eight-pass decode (CVE-2026-59221), a Pyodide same-origin worker reaching admin endpoints (CVE-2026-59214), and revoked JWTs still authenticating realtime connections (CVE-2026-59219)
- **Root Cause:** Missing authorization on realtime socket events, client-trusted session identifiers, and incomplete input decoding across a self-hosted LLM UI
- **CVE:** CVE-2026-59212 through CVE-2026-59227 and CVE-2026-59715
- **Sources:** [NVD CVE-2026-59224](https://nvd.nist.gov/vuln/detail/CVE-2026-59224), [NVD CVE-2026-59216](https://nvd.nist.gov/vuln/detail/CVE-2026-59216)

### 2026-07-09 - "Friendly Fire" Turns AI Code-Audit Agents Into Code Execution

- **Target:** Anthropic Claude Code (CLI 2.1.116 through 2.1.199 on Sonnet 4.6, Sonnet 5, and Opus 4.8) and OpenAI Codex (CLI 0.142.4 on GPT-5.5) running in autonomous or auto-approval review modes
- **Impact:** When these agents are asked to audit an untrusted third-party repository, a malicious binary disguised as a compiled build artifact (seeded with strings from legitimate source to defeat disassembly checks) plus instructions planted in a plain README drive the agent to run the payload on the developer's machine, turning a security-review tool into an execution vector. Newer models sometimes flagged that the binary did not match its supposed source and executed it anyway
- **Root Cause:** Models cannot reliably separate code they are analyzing from instructions embedded in documentation, a design-level weakness rather than a version-specific bug. Found by the AI Now Institute (Boyan Milanov, Heidy Khlaaf)
- **Sources:** [The Hacker News](https://thehackernews.com/2026/07/friendly-fire-ai-agents-built-to-catch.html), [Infosecurity Magazine](https://www.infosecurity-magazine.com/news/anthropic-openai-report-exploit/)

### 2026-07-09 - AWS AI Gateway Wired to Amazon Bedrock Hijacked for Cryptomining

- **Target:** An internet-exposed "LiteLLM-Proxy" EC2 instance acting as an AI gateway with a privileged IAM role for Amazon Bedrock
- **Impact:** With SSH (port 22) open to `0.0.0.0/0`, the host was brute-forced, a 3.42 MB XMRig cryptominer was pulled from an attacker endpoint, and the instance began mining to `pool.hasvault[.]pro`. Follow-on activity from a Vietnam-based IP attempted Bedrock model calls and an IAM `CreateUser`, showing intent to abuse the gateway's model access and cloud privileges
- **Root Cause:** An AI gateway that centralizes model access, identities, and cloud IAM privileges, left exposed to the internet with weak SSH controls, becomes a high-value pivot. Detected by Darktrace (activity June 12-13, 2026)
- **Sources:** [SiliconANGLE](https://siliconangle.com/2026/07/09/darktrace-finds-ai-gateway-amazon-bedrock-access-hijacked-cryptomining/), [Dark Reading](https://www.darkreading.com/cyber-risk/ai-gateways-keys-kingdom), [GBHackers](https://gbhackers.com/hackers-compromise-aws-ai-gateway-connected-to-amazon-bedrock/)

### 2026-07-09 - Cline AI Coding Agent Hub WebSocket RCE (CVE-2026-59723)

- **Target:** Cline autonomous coding agent before 3.0.30
- **Impact:** The Cline Hub dashboard `/browser` WebSocket accepts connections without Origin validation; when `ROOM_SECRET` is unset on `127.0.0.1`, a malicious webpage can send `desktopCommand` frames to read workspace state, alter MCP and provider settings, and trigger command execution
- **Root Cause:** Missing Origin and authentication checks on a locally bound WebSocket (CWE-346), a repeat of the local-agent WebSocket exposure pattern
- **CVE:** CVE-2026-59723 (CVSS 8.8)
- **Sources:** [NVD](https://nvd.nist.gov/vuln/detail/CVE-2026-59723), [Cline release](https://github.com/cline/cline/releases/tag/cli-v3.0.30)

### 2026-07-08 - "GhostApproval" Symlink Flaws Defeat Human-in-the-Loop in Six AI Coding Assistants

- **Target:** Amazon Q Developer, Anthropic Claude Code, Augment, Cursor, Google Antigravity, and Windsurf
- **Impact:** A malicious repository plants a symlink disguised as an innocuous file (for example `project_settings.json`) that points at `~/.ssh/authorized_keys` or a shell startup file. When the developer asks the agent to set up or edit the file, the agent writes attacker content through the symlink for passwordless SSH access or code execution, while the approval dialog displays the harmless presented path rather than the real target. Some tools write before approval or with no prompt at all
- **Root Cause:** Agents follow symlinks with standard file operations but seek approval based on the shown path, not the resolved target (CWE-61 symlink following plus CWE-451 UI misrepresentation), so human approval is meaningless. Found by Wiz Research. Amazon (CVE-2026-12958, fixed in Language Server 1.69.0) and Cursor (CVE-2026-50549, fixed in 3.0) patched, Google fixed with a CVE pending, Augment and Windsurf were unpatched at disclosure, and Anthropic called it outside its threat model
- **CVE:** CVE-2026-12958, CVE-2026-50549
- **Sources:** [Wiz](https://www.wiz.io/blog/ghostapproval-a-trust-boundary-gap-in-ai-coding-assistants), [The Hacker News](https://thehackernews.com/2026/07/ghostapproval-symlink-flaws-could-let.html), [The Register](https://www.theregister.com/security/2026/07/08/bug-in-top-ai-coding-agents-shows-that-unix-era-security-headaches-never-really-die/)

### 2026-07-08 - "HalluSquatting" Weaponizes AI Package-Name Hallucinations

- **Target:** AI coding assistants Cursor, Windsurf, GitHub Copilot, Cline, Gemini CLI, and the OpenClaw family
- **Impact:** Attackers register the fake package and repository names that models reliably invent, seed them with malicious code plus hidden prompt injection, and wait for an assistant to fetch the attacker version when a user asks for the "real" resource, chaining hallucination to code execution via the agent's terminal tool. Researchers measured an 85% consistency rate for the same incorrect repository names across phrasings and vendors and a 100% success rate for skill-install requests
- **Root Cause:** Structural LLM hallucination of plausible but non-existent artifacts combined with agents that fetch and run model-named resources without verification. Found by the Ben Nassi group (Tel Aviv University) with Stav Cohen (Technion) and Ron Bitton (Intuit)
- **Sources:** [The Hacker News](https://thehackernews.com/2026/07/new-hallusquatting-attack-could-trick.html)

### 2026-07-08 - GitHub Copilot Guardrails Bypassed by Multi-Step Workflow Framing

- **Target:** GitHub Copilot IDE agents in VS Code, backed by Claude Sonnet 4.6, Claude Haiku 4.5, Gemini 3.1 Pro, and Gemini 3.5 Flash
- **Impact:** Harmful requests that Copilot refuses in direct chat succeed when decomposed into ordinary coding-workflow steps. Across 204 harmful prompts and four model backends, direct prompting produced only 8 of 816 unsafe completions, while the workflow-staged version produced 816 of 816, with usable harmful output typically after about six exchanges
- **Root Cause:** Safety guardrails evaluate a single prompt in isolation, so reframing a harmful goal as a sequence of benign-looking IDE tasks slips past them. Found by the Alan Turing Institute (Abhishek Kumar, Carsten Maple)
- **Sources:** [The Hacker News](https://thehackernews.com/2026/07/github-copilot-refuses-harmful-requests.html), [Help Net Security](https://www.helpnetsecurity.com/2026/07/09/github-coding-agent-jailbreak/), [The Register](https://www.theregister.com/security/2026/07/08/github-copilot-sorry-dave-i-cant-do-that-harmful-thing-unless-you-ask-me-in-code/)

### 2026-07-08 - China's CNVDB Labels Claude Code a "Backdoor"; Alibaba Bans It

- **Target:** Anthropic Claude Code, versions 2.1.91 (April 2) through 2.1.196 (June 29)
- **Impact:** China's national vulnerability body alleged Claude Code contained a built-in monitoring mechanism that collected user location and identity data and sent it to external servers. Alibaba added Claude Code to its high-risk software list and barred employees from using it starting July 10, moving staff to in-house tools
- **Root Cause:** Anthropic said the code was a March 2026 anti-abuse experiment that checked base URL, timezone, and hostname against reseller and Chinese-company lists to counter unauthorized reselling and model distillation, and that it was removed in version 2.1.198 (July 1). The dispute is over disclosure and intent rather than a formal CVE
- **Sources:** [The Register](https://www.theregister.com/security/2026/07/08/china-ditch-older-claude-versions-with-backdoor-code/5268371), [CNBC](https://www.cnbc.com/2026/07/08/china-anthropic-ai-claude-code-backdoor-security-threat.html), [TechCrunch](https://techcrunch.com/2026/07/04/alibaba-reportedly-bans-employees-from-using-claude-code/)

### 2026-07-08 - Sygnia Documents Lone Attacker Breaching AWS in 72 Hours With Agentic AI

- **Target:** An unnamed global enterprise's large AWS environment
- **Impact:** A single financially motivated actor used AI-assisted, parallelized workflows to compromise a large AWS estate in about 72 hours, work that normally takes weeks, then attempted extortion. The attacker harvested secrets from S3, databases, Secrets Manager, and Parameter Store, established persistence via new access keys, IAM users, and reverse shells, exfiltrated RDS data, and staged reversible destructive moves (blocking S3 access, scaling containers to zero, writing deny ACLs, purging queues) for leverage; at one point four access keys from four accounts were used in a single second from one IP
- **Root Cause:** LLM and agentic tooling lowered the barrier and accelerated recon, credential discovery, cloud enumeration, and pipeline abuse. Initial access came through an AWS key exposed by an internet-facing application. Investigated by Sygnia
- **Sources:** [Infosecurity Magazine](https://www.infosecurity-magazine.com/news/threat-actor-agentic-ai-cloud/), [Dark Reading](https://www.darkreading.com/cloud-security/lone-attacker-ai-breach-aws-cloud-environment)

### 2026-07-08 - Injective SDK npm Packages Backdoored to Steal Wallet Keys and Poison AI Agent Configs

- **Target:** `@injectivelabs/sdk-ts` (about 50,000 weekly downloads) and 18 related packages
- **Impact:** A compromised maintainer account pushed a backdoor that hooked `PrivateKey.fromMnemonic()` and `PrivateKey.fromHex()` to capture BIP-39 seed phrases and private keys the moment a wallet loaded, sending them to an attacker server; reporting noted the campaign also dropped persistent backdoor files into AI coding assistant configuration (a Claude Code SessionStart hook, Cursor rules, Gemini settings). The malicious version was live about 49 minutes and downloaded 310 times before a clean 1.20.23 was published, with no user funds reported lost
- **Root Cause:** Maintainer account compromise plus automatic publishing propagated the tainted build across the scope within minutes; poisoning AI-assistant config files gives the malware persistence inside developer agents
- **Sources:** [BleepingComputer](https://www.bleepingcomputer.com/news/security/injective-sdk-on-npm-infected-with-cryptocurrency-wallet-stealer/), [The Hacker News](https://thehackernews.com/2026/07/injective-labs-github-compromise-pushes.html), [StepSecurity](https://www.stepsecurity.io/blog/injective-npm-supply-chain-attack-18-packages-backdoored-to-steal-crypto-wallet-keys)

### 2026-07-08 - ESET H1 2026 Threat Report: Malicious AI Agent Skills Surge, First GenAI Android Malware

- **Target:** Public AI agent skill repositories; Android users
- **Impact:** ESET analyzed roughly 900,000 unique AI agent skills, flagging more than 25,000 as suspicious and over 3,000 as outright malicious, up from about 10,000 and 600 respectively as the scanned population grew from 60,000 to 900,000 between March and May 2026; capabilities included command execution, file and credential access, and obfuscation. The report also names PromptSpy, described as the first Android malware to use generative AI in its execution flow
- **Root Cause:** Threat actors planting malicious skills in open marketplaces and repositories, and beginning to embed generative AI directly into malware
- **Sources:** [Help Net Security](https://www.helpnetsecurity.com/2026/07/08/eset-ai-threat-trends-report/), [ESET (GlobeNewswire)](https://www.globenewswire.com/news-release/2026/07/08/3323874/0/en/ESET-Threat-Report-AI-boosts-cyber-attackers-efficiency.html)

### 2026-07-08 - LiteLLM MCP Authentication Bypass (CVE-2026-59822)

- **Target:** LiteLLM (BerriAI) before 1.84.0
- **Impact:** A fabricated `Authorization` header on the MCP Streamable HTTP endpoint triggers an OAuth2 passthrough fallback that replaces failed key validation with an empty auth object, letting an unauthenticated attacker reach MCP tooling without a valid key. Three sibling flaws the same day cover a Skills ZIP path traversal (CVE-2026-59820), an arbitrary file read via `/health/test_connection` (CVE-2026-59819), and unsandboxed code in Custom Code Guardrails (CVE-2026-59821)
- **Root Cause:** Authentication fallback logic that fails open (CWE-287) in the MCP request path
- **CVE:** CVE-2026-59822 (CVSS 8.8), CVE-2026-59820, CVE-2026-59819, CVE-2026-59821
- **Sources:** [NVD](https://nvd.nist.gov/vuln/detail/CVE-2026-59822), [LiteLLM release](https://github.com/BerriAI/litellm/releases/tag/v1.84.0)

### 2026-07-07 - "GitLost" Leaks Private Repositories via GitHub Agentic Workflows

- **Target:** GitHub Agentic Workflows (public preview; agents running on Copilot, Claude, Gemini, or Codex inside GitHub Actions)
- **Impact:** An unauthenticated attacker files a public GitHub issue containing hidden plain-English instructions; when the workflow triggers, the credentialed agent (which can read repositories the attacker cannot) follows them and posts private repository contents into a public comment. The proof of concept exfiltrated a private repo's README into a public comment, and prefixing the injection with "Additionally" bypassed GitHub's threat-detection guardrails
- **Root Cause:** The agent cannot distinguish owner instructions from attacker-planted content in an untrusted issue (the "lethal trifecta" of private-data access, untrusted input, and a public output channel); GitHub describes it as an architectural limitation rather than a patchable bug. Found by Noma Security
- **Sources:** [Noma Security](https://noma.security/blog/gitlost-how-we-tricked-githubs-ai-agent-into-leaking-private-repos/), [The Hacker News](https://thehackernews.com/2026/07/public-github-issue-could-trick-github.html), [SecurityWeek](https://www.securityweek.com/critical-vulnerability-exposes-github-agentic-workflows-to-prompt-injection/)

### 2026-07-07 - Google Dialogflow CX "Rogue Agent" Cross-Agent Hijack

- **Target:** Google Dialogflow CX agents using Code Block Playbooks with custom Python
- **Impact:** An attacker with `dialogflow.playbooks.update` permission on one Code Block-enabled agent could overwrite `code_execution_env.py` in a shared, customer-invisible Cloud Run runtime, running injected code for every agent in the same Google Cloud project to read live conversations, steal user data, and inject phishing responses
- **Root Cause:** A writable setup file in a shared runtime with no isolation between agents, plus unrestricted outbound access and exposed metadata. Found by Varonis; reported November 2025, initially fixed April 2026 and fully resolved June 2026, with no evidence of in-the-wild abuse and no CVE
- **Sources:** [The Hacker News](https://thehackernews.com/2026/07/rogue-agent-flaw-could-have-let.html), [Axios](https://www.axios.com/2026/07/07/varonis-google-ai-agent-chatbot-security), [Dark Reading](https://www.darkreading.com/application-security/dialogflow-cx-rogue-agent-flaw-enabled-ai-chatbot-data-theft)

### 2026-07-07 - "WriteOut" Cross-Tenant Account Takeover in Writer AI

- **Target:** Writer enterprise generative AI platform, live agent preview feature
- **Impact:** A logged-in user who clicked a shared agent preview link could have their account hijacked across organizational tenants, exposing private chats, documents, agent configurations, private models, connectors, and LLM credentials, with potential admin control
- **Root Cause:** Writer served agent previews from the same origin as the main app, so the browser auto-attached the user's session cookie and the proxy forwarded it into the attacker-controlled sandbox, breaking tenant isolation. Found by SAND Security; Writer moved previews to an isolated origin and stopped forwarding the session cookie, fixing it within 24 hours, with no customer data compromised
- **Sources:** [The Hacker News](https://thehackernews.com/2026/07/writer-ai-flaw-could-let-agent-previews.html), [SAND Security](https://www.sandsecurity.ai/blog/writeout-writer-ai-cross-tenant)

### 2026-07-07 - CISA Adds Langflow IDOR (CVE-2026-55255) to KEV, First AI Agent Platform in the Catalog

- **Target:** Langflow before 1.9.1 (guidance updated to 1.9.2)
- **Impact:** An insecure direct object reference in `/api/v1/responses` lets an authenticated attacker execute any flow belonging to another user by supplying the victim's flow ID, exposing embedded LLM provider keys, cloud credentials, and database secrets. Sysdig observed a lone operator chaining reconnaissance, this IDOR, and a loop of the Langflow RCE CVE-2026-33017 between June 22 and 25, 2026 to harvest secrets and stage second-stage implants. CISA added it to the Known Exploited Vulnerabilities catalog on July 7 with a July 10 federal deadline, the first AI agent orchestration platform to enter the catalog
- **Root Cause:** Authorization bypass through a user-controlled key (CWE-639); flow lookups query by UUID with no ownership check
- **CVE:** CVE-2026-55255 (NVD CVSS 8.4; vendor rates 9.9)
- **Sources:** [The Hacker News](https://thehackernews.com/2026/07/cisa-adds-4-actively-exploited-adobe.html), [BleepingComputer](https://www.bleepingcomputer.com/news/security/cisa-orders-feds-to-prioritize-patching-langflow-auth-bypass-flaw/), [Help Net Security](https://www.helpnetsecurity.com/2026/07/08/langflow-vulnerability-cve-2026-55255-exploited/)

### 2026-07-07 - Google Gemini Live API RCE via Unconstrained Ephemeral Tokens

- **Target:** Browser-based applications using the Gemini Live API for voice sessions
- **Impact:** Misconfigured ephemeral tokens let a client override the system prompt and tool definitions and trigger code execution; a proof of concept ran `os.uname()` and returned a nonce-based SHA-256 to prove genuine execution. The weakness traces back to Google's own reference implementation
- **Root Cause:** Every setup-frame field is optional, so any parameter not locked server-side (via `live_connect_constraints`) stays client-controllable. Reported by researcher Alvin Ferdiansyah
- **Sources:** [GBHackers](https://gbhackers.com/google-gemini-live-api-flaw/), [Cybersecurity News](https://cybersecuritynews.com/gemini-live-voice-session-flaw/)

### 2026-07-07 - mem0 Unauthenticated Memory Access and Key Disclosure (CVE-2026-59705, CVE-2026-59706)

- **Target:** mem0 (mem0ai) OpenMemory API
- **Impact:** OpenMemory API routers were registered with no authentication, letting an unauthenticated attacker read, write, or delete arbitrary user memories and force a global pause for denial of service (CVE-2026-59705); a companion flaw exposes LLM API keys in plaintext through the config API and allows SSRF via the `ollama_base_url` value (CVE-2026-59706)
- **Root Cause:** Missing authentication middleware on agent memory endpoints (CWE-306), a direct memory-poisoning and credential-theft path against a widely used agent memory store
- **CVE:** CVE-2026-59705 (CVSS 9.3), CVE-2026-59706 (CVSS 9.2)
- **Sources:** [NVD CVE-2026-59705](https://nvd.nist.gov/vuln/detail/CVE-2026-59705), [NVD CVE-2026-59706](https://nvd.nist.gov/vuln/detail/CVE-2026-59706)

### 2026-07-07 - DigiCert AI Trust Outlook: 78% of Enterprises Report AI Security Incidents

- **Target:** Enterprise AI deployments (survey of 1,001 IT and security decision-makers in the US, UK, and Australia, conducted May 2026)
- **Impact:** 78% of organizations said they experienced AI-related incidents or identified AI-related vulnerabilities, which coverage attributes largely to unauthorized or misconfigured AI agents. Nearly half lack centralized visibility into their AI systems and 47% cannot fully trace AI decisions back to models and source data, even as 75% deployed four or more AI-powered systems in the prior six months
- **Root Cause:** AI adoption outrunning governance, identity, and visibility controls
- **Sources:** [DigiCert (GlobeNewswire)](https://www.globenewswire.com/news-release/2026/07/07/3323253/0/en/latest-digicert-research-shows-ai-security-risks-already-hitting-enterprises-with-78-reporting-incidents.html), [The Register](https://www.theregister.com/security/2026/07/07/enterprise-ai-still-smarting-from-leaping-before-looking/5267353), [SD Times](https://sdtimes.com/ai-governance/survey-reveals-78-of-enterprises-are-reporting-ai-related-security-incidents/)

### 2026-07-07 - Trend Micro "Stars Don't Save You" MCP Ecosystem Study

- **Target:** 9,695 MCP servers indexed across GitHub, Glama, Lobehub, and PulseMCP
- **Impact:** 5,832 servers carried at least one weakness and 2,259 had confirmed exploitable vulnerabilities across 4,982 distinct issues, including 2,054 with no authentication, 880 arbitrary file access, 490 denial of service, 476 command injection, 422 SSRF, 211 SQL injection, and 185 prompt injection. The study found no correlation between GitHub stars or verification badges and actual security
- **Root Cause:** An immature MCP ecosystem where popularity signals do not track security and many servers ship without authentication or input validation
- **Sources:** [Trend AI Security](https://www.trendaisecurity.com/en-us/resources-insights/research/stars-dont-save-you-popularity-is-not-security-in-the-mcp-ecosystem), [Cyberpress](https://cyberpress.org/4982-security-issues-expose-2259-public-mcp-servers-to-ai-agent-attacks/), [GBHackers](https://gbhackers.com/thousands-of-mcp-servers-found-vulnerable/)

### 2026-07-06 - "SkillCloak" Repacks Malicious AI Agent Skills to Evade Scanners

- **Target:** AI agent skill marketplaces and scanners; cloaked skills tested against Claude Code and OpenAI Codex
- **Impact:** SkillCloak preserves a payload's malicious behavior while rewriting its visible structure, using structural obfuscation and self-extracting packing that hides components in scanner-skipped directories such as `.git` and restores them at runtime. Across 8 scanners and 1,613 real malicious ClawHub skills, self-extracting packing evaded every scanner more than 90% of the time (most above 99%) and dropped the best static scanner from about 99% to 10% detection, while cloaked skills still ran under production agents with no measurable loss of function; the authors' runtime detector SkillDetonate caught 97% of synthetic and 87% of real-world cases
- **Root Cause:** Static scanners inspect skills at submission time, but malicious behavior manifests at runtime. Found by the Hong Kong University of Science and Technology
- **Sources:** [The Hacker News](https://thehackernews.com/2026/07/new-skillcloak-technique-lets-malicious.html), [Help Net Security](https://www.helpnetsecurity.com/2026/07/09/malicious-ai-agent-skills-scan/), [arXiv](https://arxiv.org/abs/2607.02357)

### 2026-07-06 - Summer.fi "Keeper AI Agents" Exploit Drains About $6M

- **Target:** Summer.fi (Lazy Summer Protocol) DeFi automation
- **Impact:** An exploit drained roughly $6 million, with the attack path traversing the protocol's automated "Keeper AI Agents" that handle rebalancing; commentators framed it as AI automation now sitting above smart-contract risk. It was one of several DeFi incidents in the week (a BonkDAO governance drain and a Bonzo Lend oracle exploit) totaling around $35 million
- **Root Cause:** Automated agent-driven protocol actions layered on top of smart-contract exposure, expanding the blast radius of the underlying exploit
- **Sources:** [CryptoSlate](https://cryptoslate.com/summer-fi-exploit-shows-ai-automation-now-sits-above-defi-smart-contract-risk/), [Crypto Times](https://www.cryptotimes.io/2026/07/12/crypto-loses-35m-in-a-week-bonkdao-bonzo-lend-summer-fi-hacked/)

### 2026-07-06 - OpenAI Codex Desktop Zero-Click Data Exfiltration (CVE-2026-14898)

- **Target:** OpenAI Codex desktop app for macOS before 26.527.31326
- **Impact:** The app renders remote Markdown images returned by the model, so an indirect prompt injection can make the model build a remote image URL containing sensitive data that is fetched automatically with no user click, leaking it to an attacker server
- **Root Cause:** Auto-fetching remote images from untrusted model output (CWE-200), the same image-based exfiltration pattern seen in other assistants
- **CVE:** CVE-2026-14898 (CVSS 6.5)
- **Sources:** [NVD](https://nvd.nist.gov/vuln/detail/CVE-2026-14898), [OpenAI Codex](https://openai.com/codex/)

### 2026-07-02 - Zscaler Documents In-the-Wild Indirect Prompt Injection Targeting Autonomous AI Agents

- **Target:** Autonomous web-browsing AI agents; two live malicious-website campaigns observed by Zscaler ThreatLabz
- **Impact:** One campaign used SEO poisoning plus hidden CSS and JSON-LD to impersonate a fake Python package "requests-secure-v2" and push agents to pay a bogus "$3.00 developer API license" plus about 0.0012 ETH to an attacker wallet; a second typosquatted DeBank via "debank[.]auction." In Zscaler's validation across 26 models, four (Llama 3.3 70B, Llama 3.2 90B Vision, Gemini 3 Flash, Gemini 2.5 Pro) executed the fraudulent payment and two misclassified the fake site as legitimate
- **Root Cause:** Indirect prompt injection: agents ingest and act on hidden instructions embedded in retrieved web content with no boundary between data and instructions
- **Sources:** [Zscaler ThreatLabz](https://www.zscaler.com/blogs/security-research/indirect-prompt-injection-web-content-targets-ai-agents), [Cybersecurity News](https://cybersecuritynews.com/hackers-abuse-seo-poisoning-and-hidden-html/)

### 2026-07-02 - fast-mcp-telegram MCP Server Authentication Bypass (CVE-2026-52830)

- **Target:** fast-mcp-telegram MCP server (PyPI), versions before 0.19.1
- **Impact:** A remote, unauthenticated attacker bypasses authentication with a crafted Bearer token such as `../fast-mcp-telegram/telegram` to authenticate as the default legacy session, reaching the MCP tools and the connected Telegram account data across tenant boundaries
- **Root Cause:** The Bearer token is joined directly into a session-file path with no normalization; the server rejects the literal token "telegram" but not path separators (CWE-22 path traversal plus CWE-287 improper authentication)
- **CVE:** CVE-2026-52830 (CVSS 9.4)
- **Sources:** [NVD](https://nvd.nist.gov/vuln/detail/CVE-2026-52830), [GitLab Advisory](https://advisories.gitlab.com/pypi/fast-mcp-telegram/CVE-2026-52830/)

### 2026-07-01 - "JADEPUFFER" First Documented End-to-End Agentic Ransomware

- **Target:** An internet-facing Langflow instance used for initial access, then a production database environment (MySQL and the Alibaba Nacos configuration service)
- **Impact:** Sysdig documented what it calls the first extortion operation run end to end by an autonomous LLM agent: recon, credential theft, lateral movement, privilege escalation, and persistence, culminating in the encryption of 1,342 Nacos service-configuration items before the originals were deleted. The agent self-narrated, adapted in real time (a failed login diagnosed and fixed in 31 seconds), and swept for LLM API keys and cloud credentials; the encryption key was generated randomly and never stored, making recovery impossible
- **Root Cause:** Initial access via the older Langflow unauthenticated code-execution flaw CVE-2025-3248, with Nacos reached through auth-bypass CVE-2021-29441; the novelty is the fully agent-driven operation rather than a new vulnerability
- **CVE:** CVE-2025-3248, CVE-2021-29441 (exploited, not newly disclosed)
- **Sources:** [Sysdig](https://sysdig.com/blog/jadepuffer-agentic-ransomware-for-automated-database-extortion), [The Register](https://www.theregister.com/security/2026/07/02/smooth-ai-criminal-drives-first-end-to-end-agentic-ransomware-attack/), [BleepingComputer](https://www.bleepingcomputer.com/news/security/jadepuffer-ransomware-used-ai-agent-to-automate-entire-attack/)

### 2026-07-01 - Cursor "DuneSlide" Zero-Click Prompt-Injection RCE (CVE-2026-50548, CVE-2026-50549)

- **Target:** Cursor AI code editor before 3.0 (fix shipped in Cursor 3.0, released April 2, 2026)
- **Impact:** A single innocuous prompt that ingests attacker-controlled content from an MCP server or web-search result can escape the terminal sandbox and reach OS-level remote code execution with no user interaction, giving full compromise of the host machine and connected SaaS workspaces
- **Root Cause:** Two chained flaws let injected instructions overwrite the `cursorsandbox` enforcer binary: CVE-2026-50548 trusts the agent-chosen working directory so a system path grants out-of-scope write permission, and CVE-2026-50549 is a symlink canonicalization check that fails open when path resolution fails. Reported by Cato AI Labs (Itay Ravia) on February 19, 2026; CVEs assigned June 5 and detailed publicly on July 1, 2026
- **CVE:** CVE-2026-50548, CVE-2026-50549 (both CVSS 9.8)
- **Sources:** [Cato Networks](https://www.catonetworks.com/blog/duneslide-two-critical-rce-vulnerabilities/), [The Hacker News](https://thehackernews.com/2026/07/critical-cursor-flaws-could-let-prompt.html), [SecurityWeek](https://www.securityweek.com/critical-cursor-ai-ide-flaws-could-lead-to-os-level-remote-code-execution/)

### 2026-07-01 - Check Point Demonstrates LLM-Generated Browser-Native Ransomware

- **Target:** Chromium-based browsers (Chrome, Edge) on Windows, macOS, Linux, ChromeOS, and Android; proof-of-concept technique generated by prompting DeepSeek
- **Impact:** A fake image-enhancement page uses the browser File System Access API (`showDirectoryPicker`) to read, exfiltrate, and encrypt a victim's local files with no native payload, app install, browser exploit, or root access. iOS and Safari are unaffected because the API is not exposed there
- **Root Cause:** A large language model connected an unrealistic "browser malware" idea to a legitimate, sanctioned browser permission API, producing a practical social-engineering abuse path rather than exploiting a browser vulnerability
- **Sources:** [Check Point Research](https://research.checkpoint.com/2026/browser-only-ransomware-from-llm-hallucinations-to-a-practical-attack-technique/), [The Hacker News](https://thehackernews.com/2026/07/ai-generated-browser-ransomware-abuses.html)

### 2026-07-01 - Apify Actors MCP Server Token Exfiltration (CVE-2026-50143)

- **Target:** `@apify/actors-mcp-server` (npm), all versions before 0.10.11
- **Impact:** A malicious Apify Actor with a crafted path can exfiltrate the victim's Apify API token; the MCP client automatically attaches the `Authorization: Bearer <APIFY_TOKEN>` header to every outbound connection, so redirecting the client to an attacker host leaks the credential
- **Root Cause:** Unsafe URL construction that concatenates a trusted base URL with an attacker-controlled `webServerMcpPath` value taken from an Actor definition returned by the Apify API (CWE-918 server-side request forgery)
- **CVE:** CVE-2026-50143 (CVSS 8.1)
- **Sources:** [GitLab Advisory](https://advisories.gitlab.com/npm/@apify/actors-mcp-server/CVE-2026-50143/)

### 2026-06-30 - "GuardFall" Shell-Injection Bypass in Open-Source AI Coding Agents

- **Target:** 10 of 11 tested open-source AI coding and computer-use agents, including opencode, Goose, Cline, Roo-Code, Aider, Plandex, Open Interpreter, OpenHands, SWE-agent, and a NousResearch Hermes agent; only Continue mitigated it
- **Impact:** Pattern-based command guards can be bypassed, so a poisoned README, MCP server, or Makefile can drive an agent into running destructive shell commands with the operator's full privileges, including wiping files or exfiltrating SSH keys and cloud credentials. Disclosed as lab research with no in-the-wild exploitation reported
- **Root Cause:** Guards inspect the raw command string, but Bash performs quote removal, `$IFS` and parameter expansion, and command substitution before execution, so obfuscated commands slip past denylists; a decades-old shell-quoting bypass applied to AI agents. Found by Adversa AI (Omer Ben Simon)
- **Sources:** [Adversa AI](https://adversa.ai/blog/opensource-ai-coding-agents-shell-injection-vulnerability/), [The Hacker News](https://thehackernews.com/2026/06/guardfall-exposes-open-source-ai-coding.html), [SC Media](https://www.scworld.com/brief/shell-injection-flaw-found-in-10-of-11-open-source-ai-agents)

### 2026-06-30 - Palo Alto Unit 42 "Phantom Squatting" - AI-Hallucinated Domains as an Attack Surface

- **Target:** 913 global brands analyzed; AI agents and users that trust LLM-generated URLs
- **Impact:** From 685,339 adversarial prompts, Unit 42 collected 2.1 million unique URLs and identified roughly 250,000 unregistered "phantom" domains that adversaries can register, plus 13,229 URLs already flagged malicious. Documented real abuse includes a "Montana Empire" phishing kit on a hallucinated postal-service domain (registered about 23 days after the model predicted it) and a malicious Android APK hosted on another phantom domain
- **Root Cause:** Structural LLM hallucination of non-existent but plausible domains ("zero-reputation bypass"); described as inherently hard to patch because it stems from model behavior rather than a software flaw
- **Sources:** [Palo Alto Unit 42](https://unit42.paloaltonetworks.com/phantom-squatting-hallucinated-web-domains/), [Check Point Research](https://research.checkpoint.com/2026/6th-july-threat-intelligence-report-2/)

### 2026-06-30 - Anthropic "buffa" Rust protobuf Memory-Amplification DoS (CVE-2026-55407)

- **Target:** buffa, Anthropic's Rust protobuf library, versions before 0.8.0
- **Impact:** An attacker sending crafted protobuf wire data to any service that decodes untrusted input with the default `preserve_unknown_fields=true` can force unbounded heap allocation; a nested-group path amplifies roughly 22x, so a 64 MiB message triggers about 1.4 GB of allocation and out-of-memory crashes
- **Root Cause:** The `decode_unknown_field` path allocates memory proportional to attacker-controlled data with no per-message field-count limit; the 0.8.0 fix caps unknown fields (default 1 million) and bounds overhead. Found by Endor Labs' AI SAST engine (Peyton Kennedy)
- **CVE:** CVE-2026-55407 (CVSS 6.3)
- **Sources:** [Endor Labs](https://www.endorlabs.com/learn/endor-labs-ai-sast-finds-zero-day-cve-2026-55407-buffa)

### 2026-06-26 - Amazon Q Developer Silent MCP Config Auto-Load (CVE-2026-12957, CVE-2026-12958)

- **Target:** Language Servers for AWS before 1.69.0; Amazon Q Developer for VS Code before 2.20, JetBrains before 4.3, Eclipse before 2.7.4; AWS Toolkit with Amazon Q for Visual Studio before 1.94.0.0
- **Impact:** Opening a malicious repository silently executes commands and steals credentials, including AWS access keys, session tokens, cloud CLI tokens, API secrets, and SSH agent sockets, because spawned processes inherit the full developer environment
- **Root Cause:** The extension auto-loaded MCP server configurations from `.amazonq/mcp.json` workspace files with no user consent or workspace-trust verification (CVE-2026-12957, improper trust boundary), plus missing symlink validation (CVE-2026-12958); found by Wiz Research (Maor Dokhanian) on April 20, 2026 and patched May 12, 2026
- **CVE:** CVE-2026-12957, CVE-2026-12958
- **Sources:** [Cybersecurity News](https://cybersecuritynews.com/amazon-q-vulnerability/), [AWS Security Blog (ICYMI May 2026)](https://aws.amazon.com/blogs/security/icymi-may-2026-aws-security/)

### 2026-06-23 - Dify "DifyTap" Cross-Tenant Data Exposure (CVE-2026-41947 to CVE-2026-41950)

- **Target:** Dify (LangGenius) open-source LLM app platform, fixed in 1.14.2
- **Impact:** Four flaws, two critical and two unauthenticated. The tracing subsystem can be redirected to an attacker endpoint to persistently exfiltrate all messages and responses from any accessible application (CVE-2026-41947); the Plugin Daemon allows unauthenticated access to arbitrary internal API endpoints via path traversal (CVE-2026-41948); console users and chatbots can read other organizations' documents and attached files (CVE-2026-41949, CVE-2026-41950)
- **Root Cause:** Missing tenant-identity validation and absent authentication on internal file and tracing endpoints; the platform also shipped a vulnerable PDFium binary for 18+ months, and container scanners missed the issues due to Dify's unpackaged code layout
- **CVE:** CVE-2026-41947, CVE-2026-41948, CVE-2026-41949, CVE-2026-41950
- **Sources:** [SC Media](https://www.scworld.com/brief/four-vulnerabilities-in-dify-expose-cross-tenant-data), [UVcyber Threat Advisory](https://www.uvcyber.com/resources/reports/threat-advisory-difytap-vulnerabilities)

### 2026-06-22 - vLLM OpenAI-Compatible API Authentication Bypass (CVE-2026-48746)

- **Target:** vLLM OpenAI-compatible API server, versions 0.3.0 through 0.21.x (fixed in 0.22.0)
- **Impact:** A remote attacker bypasses API-key authentication and reaches protected inference endpoints without a valid `VLLM_API_KEY` or `--api-key`
- **Root Cause:** `AuthenticationMiddleware.__call__` reconstructed the request path with `URL(scope=scope).path`, which trusts the unsanitized `Host` header; a crafted `Host: localhost/v1/models?` manipulates the path so the `/v1` auth check fails open. The fix uses `scope["path"]` directly
- **CVE:** CVE-2026-48746 (CVSS 9.1)
- **Sources:** [Miggo Vulnerability Database](https://www.miggo.io/vulnerability-database/cve/CVE-2026-48746), [OpenCVE](https://app.opencve.io/cve/?vendor=vllm-project&product=vllm)

### 2026-06-18 - Microsoft AutoGen Studio "AutoJack" Drive-By Code Execution

- **Target:** Microsoft AutoGen Studio built from the GitHub main branch after the MCP plugin was added and before commit b047730; published PyPI releases, including autogenstudio 0.4.2.2, were not affected
- **Impact:** A malicious webpage tricks a localhost AutoGen Studio agent into executing arbitrary PowerShell, Bash, or executables with the developer's privileges; Microsoft demonstrated launching Calculator from a visited page
- **Root Cause:** Three chained weaknesses: the MCP WebSocket trusted localhost connections, the auth middleware excluded `/api/mcp/*` routes, and the WebSocket accepted a base64 `server_params` value from the URL and passed it to process-launch code
- **Sources:** [BleepingComputer](https://www.bleepingcomputer.com/news/security/microsoft-fixes-autogen-studio-flaw-that-enabled-code-execution/), [Threat Modeling](https://threat-modeling.com/microsoft-autogen-studio-code-execution-june-2026/)

### 2026-06-17 - Mastra AI npm Scope Compromise (Sapphire Sleet / UNC1069)

- **Target:** 144+ packages across the `mastra` and `@mastra` npm scope; `@mastra/core` draws more than 918,000 weekly downloads
- **Impact:** The entire scope was backdoored in an 88-minute automated campaign; a malicious `easy-day-js` typosquat of dayjs ran a `postinstall` dropper that disabled TLS verification, contacted a C2, and downloaded a detached cross-platform information stealer that harvested browser history and 160+ cryptocurrency wallet extensions before self-deleting
- **Root Cause:** Takeover of a dormant former-contributor npm account (`ehindero`) that still held publish rights, reached via a malicious LinkedIn link to an active employee; Microsoft attributed it to North Korean actor Sapphire Sleet (UNC1069), the same actor behind the earlier Axios compromise
- **Sources:** [The Hacker News](https://thehackernews.com/2026/06/144-mastra-npm-packages-compromised-via.html), [Microsoft Security Blog](https://www.microsoft.com/en-us/security/blog/2026/06/17/postinstall-payload-inside-mastra-npm-supply-chain-compromise/), [Snyk](https://snyk.io/blog/a-forgotten-contributor-account-compromised-the-entire-mastra-npm-package-scope/)

### 2026-06-15 - Microsoft 365 Copilot "SearchLeak" One-Click Data Theft (CVE-2026-42824)

- **Target:** Microsoft 365 Copilot Enterprise (Copilot Search)
- **Impact:** A single malicious link click could steal emails, calendar details, indexed SharePoint and OneDrive files, one-time and MFA codes, and password-reset links with no authentication; Varonis demonstrated a proof of concept, with no in-the-wild exploitation observed. Microsoft mitigated server-side
- **Root Cause:** Three chained flaws: parameter-to-prompt injection through the search `q` parameter, a race condition where streamed content rendered before sanitization, and a Content Security Policy allowlist bypass through Bing's image-fetch endpoint
- **CVE:** CVE-2026-42824 (Microsoft 6.5, NVD 7.5)
- **Sources:** [The Hacker News](https://thehackernews.com/2026/06/one-click-microsoft-365-copilot-flaw.html), [BleepingComputer](https://www.bleepingcomputer.com/news/security/new-attack-turned-microsoft-365-copilot-into-1-click-data-theft-tool/), [Varonis](https://www.varonis.com/blog/searchleak)

### 2026-06-08 - Langflow Path Traversal RCE Exploited in the Wild (CVE-2026-5027)

- **Target:** Langflow before 1.9.0; roughly 7,000 instances exposed on the public internet (Censys)
- **Impact:** Unauthenticated arbitrary file write leading to remote code execution, for example dropping a cron job; default auto-login means a single unauthenticated request reaches the vulnerable endpoint. After disclosure by Tenable and a 1.9.0 patch, attackers weaponized the flaw and exploitation was observed in June 2026
- **Root Cause:** `POST /api/v2/files` does not sanitize the multipart `filename` parameter, allowing `../` traversal to write files anywhere on the filesystem
- **CVE:** CVE-2026-5027 (CVSS 8.8)
- **Sources:** [The Hacker News](https://thehackernews.com/2026/06/unpatched-langflow-flaw-cve-2026-5027.html), [Orca Security](https://orca.security/resources/blog/cve-2026-5027-langflow-path-traversal-rce/), [SecurityWeek](https://www.securityweek.com/critical-langflow-vulnerability-exploited-hours-after-public-disclosure/)

### 2026-06-05 - Hades PyPI Worm Wave (Shai-Hulud / Miasma Lineage)

- **Target:** 19 PyPI packages across 37 malicious wheel artifacts, with a secondary cluster targeting computational-biology and MCP developers (IBM X-Force)
- **Impact:** A `*-setup.pth` file runs the payload at Python interpreter startup with no import required; a Bun-based JavaScript stealer harvests credentials for GitHub, npm, PyPI, JFrog, CircleCI, Anthropic, AWS, GCP, Azure, and Kubernetes, plus SSH keys and Vault tokens. Notably, the malware embeds a plain-text prompt injection that tries to trick LLM-based package-analysis tools into rating it safe
- **Root Cause:** `.pth` startup-hook execution combined with the Shai-Hulud and Miasma worm lineage; GitHub repo descriptions carried the marker "Hades - The End for the Damned"
- **Sources:** [Socket](https://socket.dev/blog/shai-hulud-descends-to-hades-miasma-pypi-wave), [The Hacker News](https://thehackernews.com/2026/06/hades-pypi-attack-19-packages-poisoned.html), [Dark Reading](https://www.darkreading.com/application-security/hades-campaign-pypi-shai-hulud)

### 2026-06-05 - Claude Code GitHub Action Prompt-Injection Secret Exfiltration

- **Target:** Claude Code GitHub Action before 2.1.128
- **Impact:** Prompt injection hidden in GitHub issues, pull requests, or comments could steer the agent into reading unsanitized environment data through `/proc/self/environ` and exfiltrating CI/CD secrets, API keys, and cloud credentials through issue comments, workflow logs, web requests, or shell commands
- **Root Cause:** The agent processed untrusted GitHub content in the same runtime that held privileged secrets; disclosed by Microsoft researchers through HackerOne on April 29, 2026 and patched in 2.1.128 on May 5, 2026
- **Sources:** [Decrypt](https://decrypt.co/370238/claude-code-vulnerability-attackers-steal-credentials-github-microsoft), [Cloud Security Alliance Lab](https://labs.cloudsecurityalliance.org/research/csa-research-note-claude-code-github-action-prompt-injection/)

### 2026-06-03 - node-gyp "Phantom Gyp" Self-Propagating npm Worm (Miasma)

- **Target:** 57 npm packages across hundreds of malicious versions; the largest AI target was `@vapi-ai/server-sdk` (Vapi.ai voice-AI SDK, ~86,500 weekly downloads), with poisoned versions 0.11.1, 0.11.2, 1.2.1, and 1.2.2
- **Impact:** A weaponized `binding.gyp` makes node-gyp execute attacker code during the `npm install` configuration phase, bypassing pre/postinstall monitoring; it harvests npm, GitHub, AWS, GCP, Azure, Vault, and Kubernetes credentials, injects GitHub Actions workflows for persistence, and self-propagates
- **Root Cause:** Novel abuse of GYP command-expansion syntax in `binding.gyp` build hooks; a descendant of the Shai-Hulud and Miasma worm families
- **Sources:** [Snyk](https://snyk.io/blog/node-gyp-supply-chain-compromise-self-propagating-npm-worm-binding-gyp/), [StepSecurity](https://www.stepsecurity.io/blog/binding-gyp-npm-supply-chain-attack-spreads-like-worm)

### 2026-06-01 - Red Hat @redhat-cloud-services npm "Miasma" Worm

- **Target:** At least 32 package releases under the `@redhat-cloud-services` npm namespace (averaging ~80,000 weekly downloads), originating in the RedHatInsights/javascript-clients CI/CD pipeline
- **Impact:** A `preinstall` hook ran an obfuscated `index.js` dropper that steals GitHub tokens, SSH keys, and GCP and Azure cloud identities on developer machines, scrapes GitHub Actions runner memory in CI, and republishes poisoned packages with valid SLSA provenance attestations
- **Root Cause:** A compromised Red Hat employee account injected malicious GitHub Actions workflows that requested an OIDC token via `id-token: write` and abused npm trusted publishing; part of the Shai-Hulud and Miasma lineage. Tracked as Red Hat RHSB-2026-006
- **Sources:** [Wiz](https://www.wiz.io/blog/miasma-supply-chain-attack-targeting-redhat-npm-packages), [Microsoft Security Blog](https://www.microsoft.com/en-us/security/blog/2026/06/02/preinstall-persistence-inside-red-hat-npm-miasma-credential-stealing-campaign/), [Red Hat](https://access.redhat.com/security/vulnerabilities/RHSB-2026-006)

### 2026-05-28 - Nx Console Malicious VS Code Extension Leads to GitHub Repository Breach

- **Target:** Poisoned Nx Console VS Code extension (malicious build delivered via auto-update) used to compromise a GitHub employee device
- **Impact:** Unauthorized access to and exfiltration of internal GitHub source-code repositories after the trojanized IDE extension ran on the victim's machine
- **Root Cause:** A prior Nx developer-system compromise let attackers push a trojanized extension build through the marketplace auto-update channel, abusing the trust developers place in recommended IDE extensions
- **Sources:** [CISA Alert](https://www.cisa.gov/news-events/alerts/2026/05/28/supply-chain-compromises-impact-nx-console-and-github-repositories), [BleepingComputer](https://www.bleepingcomputer.com/news/security/vscode-ide-forks-expose-users-to-recommended-extension-attacks/)

### 2026-05-25 - "Megalodon" Mass GitHub Actions Secret Exfiltration

- **Target:** 5,561 distinct public GitHub repositories hit by 5,718 malicious commits, surfaced through trojanized Tiledesk npm versions
- **Impact:** Injected GitHub Actions workflows harvested CI environment variables, AWS, GCP, and Azure credentials, SSH keys, Docker and Kubernetes configs, and GitLab and GitHub tokens, deployed across a six-hour window on May 18, 2026
- **Root Cause:** Malicious CI workflows injected across thousands of repositories; the same npm account that shipped clean Tiledesk versions unknowingly published poisoned ones after its GitHub repository was compromised. Researched by SafeDep with additional analysis from OX Security
- **Sources:** [SecurityWeek](https://www.securityweek.com/over-5500-github-repositories-infected-in-megalodon-supply-chain-attack/), [StepSecurity](https://www.stepsecurity.io/blog/megalodon-mass-github-actions-secret-exfiltration-across-5-500-public-repositories)

### 2026-05-20 - NVIDIA Triton Inference Server Authentication Bypass (CVE-2026-24207)

- **Target:** NVIDIA Triton Inference Server on Linux, all releases prior to r26.03
- **Impact:** A critical unauthenticated authentication bypass can lead to code execution, privilege escalation, data tampering, denial of service, or information disclosure, alongside seven additional flaws including path traversal, integer overflow, and DALI-backend issues
- **Root Cause:** Authentication bypass (CWE-288) plus memory-safety and integer-overflow defects in the model-serving stack
- **CVE:** CVE-2026-24207 (CVSS 9.8); also CVE-2026-24206, -24208, -24209, -24210, -24213, -24214, -24215
- **Sources:** [NVIDIA Security Bulletin](https://nvidia.custhelp.com/app/answers/detail/a_id/5828/~/security-bulletin:-nvidia-triton-inference-server---may-2026), [Security Online](https://securityonline.info/nvidia-triton-inference-server-vulnerability-cve-2026-24207-authentication-bypass/)

### 2026-05-18 - actions-cool GitHub Actions Tag Hijack (Mini Shai-Hulud)

- **Target:** `actions-cool/issues-helper` (all 53 tags) and `actions-cool/maintain-one-comment` (15 tags), retargeted to a single imposter commit
- **Impact:** A Bun-based payload reads decrypted secrets directly from the `Runner.Worker` process memory inside GitHub Actions and exfiltrates CI/CD credentials from every workflow that pinned the actions by tag
- **Root Cause:** Repository or maintainer compromise plus mutable-tag retargeting, attributed to the TeamPCP Mini Shai-Hulud campaign
- **Sources:** [StepSecurity](https://www.stepsecurity.io/blog/actions-cool-issues-helper-github-action-compromised-all-tags-point-to-imposter-commit-that-exfiltrates-ci-cd-credentials), [The Hacker News](https://thehackernews.com/2026/05/github-actions-supply-chain-attack.html)

### 2026-05-15 - PraisonAI Auth-Disabled API Server (CVE-2026-44338)

- **Target:** PraisonAI legacy Flask API server, before 4.6.34
- **Impact:** Unauthenticated attackers enumerate configured agents via `GET /agents`, trigger workflows via `POST /chat`, extract sensitive output, and exhaust costly model quotas; exploited within hours of disclosure
- **Root Cause:** Hardcoded insecure defaults (`AUTH_ENABLED = False`, `AUTH_TOKEN = None`) with the server bound to `0.0.0.0:8080` and a `check_auth()` that fails open when auth is disabled
- **CVE:** CVE-2026-44338
- **Sources:** [Cybersecurity News](https://cybersecuritynews.com/praisonai-vulnerability-exploited/)

### 2026-05-14 - OpenAI Internal Source Code and Certificate Theft via TanStack Worm

- **Target:** OpenAI internal source-code repositories and code-signing certificates
- **Impact:** Attackers stole limited credential material and digital certificates used to sign OpenAI products from a limited subset of internal repositories after two employee devices were infected; OpenAI said no user data, production systems, or intellectual property were compromised and rotated affected certificates, forcing a macOS app update
- **Root Cause:** The compromise of the open-source TanStack library, where attackers published 84 malicious versions in a roughly six-minute window (part of the Mini Shai-Hulud worm), infected the developer machines that reached OpenAI's repositories
- **Sources:** [TechCrunch](https://techcrunch.com/2026/05/14/openai-says-hackers-stole-some-data-after-latest-code-security-issue/), [The Hacker News (Mini Shai-Hulud)](https://thehackernews.com/2026/05/mini-shai-hulud-worm-compromises.html)

### 2026-05-12 - Claude Code Deeplink RCE

- **Target:** Claude Code before 2.1.118
- **Impact:** A crafted `claude-cli://` deeplink injects `--settings={...}` (including a `SessionStart` hooks payload) through the `--prefill` value, so one click runs arbitrary shell commands; pointing the deeplink's `repo` parameter at an already-trusted local repository suppressed all warning prompts for a silent compromise
- **Root Cause:** `eagerParseCliFlag` in `main.tsx` used `startsWith` across the whole argv array without tracking whether a `--settings=` string was a real flag or the value of another flag, letting flags be smuggled inside values. Researcher: Joern Chen (joernchen, 0day.click)
- **Sources:** [0day.click](https://0day.click/recipe/2026-05-12-cc-rce/), [GBHackers](https://gbhackers.com/claude-code-vulnerability/), [Cybersecurity News](https://cybersecuritynews.com/claude-code-rce-flaw/)

### 2026-05-12 - Cline AI Agent Unauthenticated WebSocket RCE (CVE-2026-44211)

- **Target:** Cline AI coding agent (the bundled Kanban npm server) on macOS, Linux, and Windows; no patched version at disclosure
- **Impact:** A malicious webpage can reach Cline's background local WebSocket server on port 3484, leak filesystem paths, git branch details, task titles, and live agent chat messages, and execute arbitrary code on the developer's machine
- **Root Cause:** The local WebSocket server started with no authentication and no Origin-header validation, and browsers do not restrict cross-origin WebSocket connections to localhost. Documented by Oasis Security and researcher TheRealSpencer
- **CVE:** CVE-2026-44211 (CVSS 9.7)
- **Sources:** [Cybersecurity News](https://cybersecuritynews.com/cline-ai-agent-vulnerability/), [Oasis Security](https://www.oasis.security/blog/)

### 2026-05-12 - GitHub Copilot and VS Code Security-Feature Bypass (CVE-2026-41109)

- **Target:** GitHub Copilot extension before v1.43.20260512 and Visual Studio Code up to 1.96.x (fixed in VS Code 1.97.0)
- **Impact:** A low-privileged local attacker can bypass Copilot's user-consent prompts and content filters, inject malicious code suggestions, silently disable telemetry consent, and leak environment variables and API keys through suggestion logging
- **Root Cause:** Improper validation of inter-process communication between the Copilot extension and the VS Code core when the workspace was marked trusted; the flaw is in the integration layer, not the model
- **CVE:** CVE-2026-41109 (CVSS 7.8)
- **Sources:** [Windows News AI](https://windowsnews.ai/article/cve-2026-41109-copilot-and-vs-code-security-feature-bypass-in-the-dev-workflow.417882), [The Hacker Wire](https://www.thehackerwire.com/github-copilot-visual-studio-injection-bypasses-security-feature-cve-2026-41109/)

### 2026-05-11 - Mini Shai-Hulud Worm Compromises TanStack, Mistral AI, and Guardrails AI (CVE-2026-45321)

- **Target:** 170+ packages across npm and PyPI with 518 million cumulative downloads, including TanStack (42 packages, 84 versions), Mistral AI, Guardrails AI (`guardrails-ai==0.10.1`, project quarantined), OpenSearch, and UiPath
- **Impact:** Credential theft across cloud providers, cryptocurrency wallets, AI tools, messaging apps, and CI systems; GitHub Actions cache poisoning and OIDC token extraction; self-propagation using any publishable npm token, with a dead-man's switch that runs destructive commands if a compromised token is revoked. It was the first documented npm worm to ship valid SLSA Build Level 3 provenance attestations
- **Root Cause:** Abuse of npm OIDC trusted publishing and CI cache poisoning with malicious install-time hooks, attributed to TeamPCP
- **CVE:** CVE-2026-45321 (CVSS 9.6)
- **Sources:** [The Hacker News](https://thehackernews.com/2026/05/mini-shai-hulud-worm-compromises.html), [Socket](https://socket.dev/blog/tanstack-npm-packages-compromised-mini-shai-hulud-supply-chain-attack), [Tenable](https://www.tenable.com/blog/mini-shai-hulud-frequently-asked-questions)

### 2026-05-11 - Google GTIG Reports First AI-Developed Zero-Day for Mass Exploitation

- **Target:** A popular open-source web-based system-administration tool (vendor unnamed) and internet-facing infrastructure running it
- **Impact:** A cybercrime actor used a large language model to discover and weaponize a 2FA-bypass zero-day for a planned mass-exploitation campaign; Google Threat Intelligence Group identified and disrupted it through responsible disclosure before use
- **Root Cause:** The LLM reasoned about a hardcoded trust assumption in the tool's 2FA enforcement logic, a semantic flaw that traditional scanners and fuzzers miss; the generated Python exploit carried LLM hallmarks including educational docstrings and a hallucinated CVSS score
- **Sources:** [Google Cloud Threat Intelligence](https://cloud.google.com/blog/topics/threat-intelligence/ai-vulnerability-exploitation-initial-access), [The Hacker News](https://thehackernews.com/2026/05/hackers-used-ai-to-develop-first-known.html), [CNBC](https://www.cnbc.com/2026/05/11/google-thwarts-effort-hacker-group-use-ai-mass-exploitation-event.html)

### 2026-05-10 - Ollama "Bleeding Llama" Unauthenticated Memory Leak (CVE-2026-7482)

- **Target:** Ollama before 0.17.1; more than 300,000 servers exposed online
- **Impact:** A remote, unauthenticated out-of-bounds heap read leaks process memory, including environment variables, API keys, system prompts, and other users' conversation data, which can then be pushed out through the `/api/push` endpoint
- **Root Cause:** The `/api/create` endpoint processes attacker-supplied GGUF files whose declared tensor offset and size exceed the actual file length; during quantization the server reads past the heap buffer using unsafe Go pointer operations. Found by Cyera
- **CVE:** CVE-2026-7482 (CVSS 9.1)
- **Sources:** [The Hacker News](https://thehackernews.com/2026/05/ollama-out-of-bounds-read-vulnerability.html), [Cyera](https://www.cyera.com/research/bleeding-llama-critical-unauthenticated-memory-leak-in-ollama), [SecurityWeek](https://www.securityweek.com/critical-bug-could-expose-300000-ollama-deployments-to-information-theft/)

### 2026-05-07 - Microsoft Semantic Kernel Prompt-Injection to RCE (CVE-2026-26030, CVE-2026-25592)

- **Target:** Microsoft Semantic Kernel agent framework, Python before 1.39.4 and .NET SDK before 1.71.0
- **Impact:** Prompt injection escalates to host-level remote code execution and sandbox escape; Microsoft demonstrated launching calc.exe from a single prompt, plus arbitrary file write and data exfiltration
- **Root Cause:** CVE-2026-26030 (Python) is unsafe string interpolation of AI-model-controlled parameters in the in-memory vector store with a blocklist bypassable through Python class-hierarchy traversal; CVE-2026-25592 (.NET) exposed `DownloadFileAsync` to model invocation because it was accidentally marked `[KernelFunction]` with no path validation
- **CVE:** CVE-2026-26030, CVE-2026-25592
- **Sources:** [Microsoft Security Blog](https://www.microsoft.com/en-us/security/blog/2026/05/07/prompts-become-shells-rce-vulnerabilities-ai-agent-frameworks/), [ByteIota](https://byteiota.com/semantic-kernel-rce-cve-2026-25592-cve-2026-26030/)

### 2026-05-07 - Microsoft Azure AI Foundry M365 Agent Privilege Escalation (CVE-2026-35435)

- **Target:** Microsoft Azure AI Foundry agents published to Microsoft 365 (cloud service)
- **Impact:** An unauthorized network attacker can elevate privileges with no authentication and no user interaction to access and manipulate protected agent workflows, data connectors, and backend resources; remediated server-side
- **Root Cause:** Improper access control (CWE-284) in Microsoft 365 published agents
- **CVE:** CVE-2026-35435 (NVD 10.0, Microsoft 8.6)
- **Sources:** [NVD](https://nvd.nist.gov/vuln/detail/CVE-2026-35435), [Red Packet Security](https://www.redpacketsecurity.com/cve-alert-cve-2026-35435-microsoft-azure-ai-foundry/)

### 2026-05-07 - Malicious Hugging Face "Open-OSS/privacy-filter" Fake OpenAI Model

- **Target:** Hugging Face repository `Open-OSS/privacy-filter`, typosquatting OpenAI's Privacy Filter release, plus six sibling repositories under the same account
- **Impact:** The repo reached #1 trending with roughly 244,000 downloads and 667 likes within about 18 hours; its `loader.py` fetched commands over disabled-SSL connections and ran a hidden PowerShell chain that deployed a Rust-based infostealer targeting browser credentials, crypto wallets, Discord tokens, and SSH keys on Windows hosts
- **Root Cause:** A malicious model repository masquerading as a vendor release, with engagement inflated by inauthentic accounts; found and reported by HiddenLayer
- **Sources:** [HiddenLayer](https://www.hiddenlayer.com/research/malware-found-in-trending-hugging-face-repository-open-oss-privacy-filter), [Infosecurity Magazine](https://www.infosecurity-magazine.com/news/malicious-hugging-face-repo/)

### 2026-05-05 - Ollama for Windows Auto-Updater RCE and Persistence (CVE-2026-42248, CVE-2026-42249)

- **Target:** Ollama for Windows, versions 0.12.10 through 0.22.0 (and v0.23.0), with no fix available at publication
- **Impact:** Chaining the two flaws lets an attacker plant a persistent executable that runs at every login for covert remote code execution
- **Root Cause:** CVE-2026-42248 is a signature-verification function that is called but does nothing, so any downloaded payload executes; CVE-2026-42249 builds the staged-installer path from unsanitized HTTP `ETag` headers, letting `../` sequences write an arbitrary executable into the Windows Startup folder. Reported by Striga; coordination handed to CERT Polska after no vendor response
- **CVE:** CVE-2026-42248, CVE-2026-42249
- **Sources:** [Help Net Security](https://www.helpnetsecurity.com/2026/05/05/ollama-windows-vulnerabilities-cve-2026-42248-cve-2026-42249/), [CERT Polska](https://cert.pl/en/posts/2026/04/CVE-2026-42248/)

### 2026-05-04 - Grok and Bankr AI Wallet Drained via Morse-Code Prompt Injection

- **Target:** xAI's Grok integrated with the Bankr crypto trading agent on the Base network
- **Impact:** Roughly $174,000 to $204,000 in DRB (DebtReliefBot) tokens transferred to an attacker, with about 80 to 88 percent later returned through negotiation
- **Root Cause:** A two-stage permission-chain abuse: the attacker first unlocked a high-privilege agent toolset by activating a "Bankr Club Membership," then sent Grok a Morse-code message asking it to translate; once Grok output the decoded plaintext transfer instruction and tagged the trading bot, the bot treated the public reply as a valid executable command with no source validation
- **Sources:** [SlowMist](https://slowmist.medium.com/behind-the-grok-exploitation-an-analysis-of-ai-agent-permission-chain-abuse-4d832d1bfc73), [Crypto Times](https://www.cryptotimes.io/2026/05/04/xais-grok-ai-loses-175k-in-crypto-heist-via-clever-prompt-injection-then-gets-it-all-back/), [OECD AI Incidents](https://oecd.ai/en/incidents/2026-05-04-4a73)

### 2026-04-30 - PyTorch Lightning PyPI Compromise (Mini Shai-Hulud)

- **Target:** `lightning` on PyPI, versions 2.6.2 and 2.6.3 (~311,000 daily downloads)
- **Impact:** A hidden `_runtime` directory executes at import time, fetching the Bun runtime and running an ~11 MB obfuscated JavaScript credential stealer that targets GitHub, npm, and cloud tokens; it poisons local repositories with commits forged as `claude` and self-propagates by mutating npm tarballs and publishing directly to the registry
- **Root Cause:** Maintainer or publish-credential compromise carrying a Mini Shai-Hulud payload, linked by shared obfuscation signatures to the broader npm and PyPI worm campaign
- **Sources:** [Snyk](https://snyk.io/blog/lightning-pypi-compromise-bun-based-credential-stealer/), [The Hacker News (Hades lineage context)](https://thehackernews.com/2026/06/hades-pypi-attack-19-packages-poisoned.html)

### 2026-04-30 - Google Gemini CLI CVSS 10.0 Headless RCE

- **Target:** Gemini CLI (`@google/gemini-cli`) before 0.39.1 and before 0.40.0-preview.3, and the `run-gemini-cli` GitHub Action before 0.1.22
- **Impact:** Maximum-severity remote code execution in CI. Headless mode auto-trusted workspace folders, so a malicious config file in `.gemini/` executed before sandbox init and exposed any secrets, credentials, or source the workflow could reach, enabling token theft and lateral movement
- **Root Cause:** Unsafe workspace-trust handling plus a tool-allowlist bypass under `--yolo` mode when processing untrusted pull requests or issues
- **CVE:** Advisory GHSA-wpqr-6v78-jr5g (CVSS 10.0)
- **Sources:** [The Register](https://www.theregister.com/2026/04/30/googles_fix_for_critical_gemini/), [The Hacker News](https://thehackernews.com/2026/04/google-fixes-cvss-10-gemini-cli-ci-rce.html), [Hackread](https://hackread.com/google-cvss-10-gemini-cli-vulnerability-github-rce/)

### 2026-04-29 - LiteLLM Pre-Auth SQL Injection (CVE-2026-42208)

- **Target:** LiteLLM Proxy, versions 1.81.16 up to but not including 1.83.7 (fixed in 1.83.7-stable)
- **Impact:** An unauthenticated attacker sends a crafted `Authorization: Bearer` header to any LLM API route and runs arbitrary SQL against the proxy's PostgreSQL backend, reading and modifying tables such as `litellm_credentials.credential_values` and `litellm_config` that hold upstream provider keys for OpenAI, Anthropic, and AWS Bedrock. Targeted exploitation began within 36 hours of disclosure
- **Root Cause:** During proxy API-key checks, the caller-supplied Bearer value is concatenated into the SQL text against `LiteLLM_VerificationToken` instead of being passed as a bound parameter
- **CVE:** CVE-2026-42208 (CVSS 9.3)
- **Sources:** [The Hacker News](https://thehackernews.com/2026/04/litellm-cve-2026-42208-sql-injection.html), [Sysdig](https://www.sysdig.com/blog/cve-2026-42208-targeted-sql-injection-against-litellms-authentication-path-discovered-36-hours-following-vulnerability-disclosure), [SecurityWeek](https://www.securityweek.com/fresh-litellm-vulnerability-exploited-shortly-after-disclosure/)

### 2026-04-24 - LangChain langchain-openai and langchain-text-splitters SSRF Disclosures

- **Target:** LangChain langchain-openai (before 1.1.14) and langchain-text-splitters (before 1.1.2)
- **Impact:** Attacker-controlled URLs can reach private and localhost services (cloud metadata, internal admin interfaces) from any host running LangChain image-token counting or HTML splitter helpers
- **Root Cause:** TOCTOU and DNS rebinding window in `_url_to_size()` (validate-then-fetch pattern with independent DNS resolution); HTMLHeaderTextSplitter.split_text_from_url() validated the initial URL but followed redirects via `requests.get()` without revalidating redirect targets
- **CVE:** CVE-2026-41488 (CVSS 3.1, langchain-openai SSRF), CVE-2026-41481 (CVSS 6.5, langchain-text-splitters redirect SSRF)
- **Sources:** [GitLab Advisory CVE-2026-41488](https://radar.offseq.com/threat/cve-2026-41488-cwe-918-server-side-request-forgery-b7a78a3a), [GitLab Advisory CVE-2026-41481](https://radar.offseq.com/threat/cve-2026-41481-cwe-918-server-side-request-forgery-9716de86), [TheHackerWire CVE-2026-41488](https://www.thehackerwire.com/vulnerability/CVE-2026-41488/), [TheHackerWire CVE-2026-41481](https://www.thehackerwire.com/vulnerability/CVE-2026-41481/), [Vulnerability-Lookup CVE-2026-41488](https://vulnerability.circl.lu/vuln/cve-2026-41488)

### 2026-04-23 - HexagonalRodent North Korean APT Industrializes Web3 Developer Attacks Using AI Coding Tools

- **Target:** Web3 developers worldwide (subgroup of Famous Chollima/Lazarus tracked by Expel as HexagonalRodent)
- **Impact:** 2,726 developer systems infected and 26,584 cryptocurrency wallet entries exfiltrated; an estimated $12 million in crypto assets stolen during Q1 2026; victims lured by fake high-paying job postings on LinkedIn and Web3 boards that ship "skills tests" abusing VSCode `tasks.json` to auto-execute malware on project open
- **Root Cause:** Operators with low to medium technical skill scaled malware authoring, fake company website creation, and phishing lure crafting by prompting Cursor, ChatGPT, and Anima; Cursor blocked the associated accounts and IPs; OpenAI confirmed a small number of accounts had asked for help on dual-use topics
- **Sources:** [Help Net Security](https://www.helpnetsecurity.com/2026/04/23/hexagonalrodent-north-korean-hackers-targeting-developers/), [Expel](https://expel.com/blog/inside-lazarus-how-north-korea-uses-ai-to-industrialize-attacks-on-developers/), [Yahoo / Decrypt](https://www.yahoo.com/news/articles/north-korean-hackers-industrialize-attacks-110000000.html), [KuCoin](https://www.kucoin.com/news/flash/north-korean-hackers-target-web3-developers-with-ai-powered-attacks-steal-12m-in-3-months)

### 2026-04-23 - Google Workspace Reports 32% Rise in Indirect Prompt Injection Pages on the Open Web

- **Target:** Public web content consumed by AI agents and Google Workspace Gemini integrations (sector-wide measurement based on Google's 2-3 billion crawled pages per month)
- **Impact:** Google observed a 32% relative increase in malicious indirect prompt injection pages between November 2025 and February 2026; payloads target agentic AI features that can send email, run terminal commands, or process payments; Forcepoint amplified the same finding with field cases
- **Root Cause:** AI agents ingest untrusted web content with no strict data versus instruction boundary; static blogs, forums, and comment sections are now intentional weaponization surfaces for IPI
- **Sources:** [Google Online Security Blog "AI threats in the wild"](https://security.googleblog.com/2026/04/ai-threats-in-wild-current-state-of.html), [Google Workspace continuous IPI mitigation](https://security.googleblog.com/2026/04/google-workspaces-continuous-approach.html), [Help Net Security](https://www.helpnetsecurity.com/2026/04/24/indirect-prompt-injection-in-the-wild/), [WebProNews](https://www.webpronews.com/prompt-injections-lurk-in-plain-sight-googles-scan-reveals-webs-hidden-assault-on-ai-agents/)

### 2026-04-23 - SecurityScorecard Finds 40,214 OpenClaw Instances Exposed Online with 63% RCE-Vulnerable

- **Target:** OpenClaw (formerly Moltbot/Clawdbot) personal AI agent platform
- **Impact:** Internet scan identified 40,214 reachable OpenClaw instances and 28,663 unique IP addresses hosting publicly accessible control panels; about 63% of deployments are vulnerable to remote code execution; 549 exposed instances correlate with prior breach activity and 1,493 are linked to known vulnerabilities; cloud and hosting providers concentrate the exposure
- **Root Cause:** Default deployment patterns expose admin panels with no authentication; multiple unpatched 2026 OpenClaw CVEs including ClawBleed (CVE-2026-25253, CVSS 8.8), CVE-2026-25593, and the device-pairing privilege escalation chain CVE-2026-32922 (CVSS 9.9) remain widely deployed
- **Sources:** [SecurityScorecard](https://securityscorecard.com/blog/how-exposed-openclaw-deployments-turn-agentic-ai-into-an-attack-surface/), [Infosecurity Magazine](https://www.infosecurity-magazine.com/news/researchers-40000-exposed-openclaw/), [Dataconomy](https://dataconomy.com/2026/04/23/hackers-exploit-vulnerabilities-in-openclaw-to-control-28000-systems/), [TechRadar](https://www.techradar.com/pro/security/the-math-is-simple-openclaw-trojan-horse-ai-agents-give-hackers-full-control-of-28-000-systems), [TechBriefly](https://techbriefly.com/2026/04/23/openclaw-ai-agent-flaw-exposes-over-28000-systems/)

### 2026-04-22 - Bitwarden CLI npm Package Trojanized via Checkmarx KICS Cascade

- **Target:** @bitwarden/cli npm package version 2026.4.0
- **Impact:** Malicious build of Bitwarden CLI was live on npm for roughly 90 minutes (5:57 PM-7:30 PM ET, April 22, 2026) and pulled approximately 334 times; payload `bw1.js` ran on install and harvested GitHub and npm tokens, SSH keys, AWS, GCP, and Azure secrets, GitHub Actions secrets, AI tooling configuration files, environment variables, and shell history; data exfiltrated to public GitHub repositories created under victim accounts; Bitwarden vault data was not accessed
- **Root Cause:** Earlier in the day, attackers compromised the Checkmarx KICS Docker Hub repository; Bitwarden's Dependabot pulled the malicious `checkmarx/kics:latest` image into the Bitwarden CI/CD pipeline, which then signed and published the trojanized CLI; payload contained the marker "Shai-Hulud: The Third Coming" with Dune-themed identifiers
- **Sources:** [Bitwarden Statement](https://community.bitwarden.com/t/bitwarden-statement-on-checkmarx-supply-chain-incident/96127), [The Hacker News](https://thehackernews.com/2026/04/bitwarden-cli-compromised-in-ongoing.html), [The Register](https://www.theregister.com/2026/04/27/supply_chain_campaign_targets_security), [Socket](https://socket.dev/blog/bitwarden-cli-compromised), [SecurityWeek](https://www.securityweek.com/bitwarden-npm-package-hit-in-supply-chain-attack/), [CSO Online](https://www.csoonline.com/article/4162865/bitwarden-cli-password-manager-trojanized-in-supply-chain-attack.html), [Endor Labs](https://www.endorlabs.com/learn/shai-hulud-the-third-coming----inside-the-bitwarden-cli-2026-4-0-supply-chain-attack), [GitHub Issue 20353](https://github.com/bitwarden/clients/issues/20353)

### 2026-04-22 - Xinference PyPI Package Compromise (Versions 2.6.0-2.6.2)

- **Target:** Xinference (Xorbits Inference) Python package on PyPI; an open-source distributed AI model inference framework with 600,000+ downloads used to self-host LLMs, embedding models, and image generators
- **Impact:** Three consecutive releases (2.6.0, 2.6.1, 2.6.2) shipped a base64-encoded credential-stealing payload that runs on import; harvests AWS credentials and secrets, Google Cloud configurations, Kubernetes tokens, environment variables, SSH keys, API keys, and database credentials; payload spawns a detached subprocess so it survives parent process exit
- **Root Cause:** An automated bot account "XprobeBot" (active since October 2025) was compromised and committed the malicious payload directly into `__init__.py`; payload structure mirrors prior TeamPCP attacks (double base64, exhaustive credential sweep, detached subprocess on import), but TeamPCP publicly denied responsibility for this one
- **Sources:** [Mend.io](https://www.mend.io/blog/malicious-xinference-pypi-teampcp-part-4/), [GBHackers](https://gbhackers.com/xinference-pypi-breach-exposes-developers/), [OX Security](https://www.ox.security/blog/xinference-allegedly-hacked-by-teampcp-malicious-package-in-pypi/), [Cyberpress](https://cyberpress.org/xinference-pypi-package-compromised/), [GitGuardian](https://blog.gitguardian.com/three-supply-chain-campaigns-hit-npm-pypi-and-docker-hub-in-48-hours/), [Orca Security](https://orca.security/resources/blog/xinference-pypi-package-compromise-remediation/)

### 2026-04-21 - LMDeploy SSRF Exploited Within 13 Hours of Public Disclosure (CVE-2026-33626)

- **Target:** LMDeploy LLM serving toolkit by Shanghai AI Laboratory / InternLM (all versions 0.12.0 and earlier with vision-language support)
- **Impact:** Sysdig honeypot recorded the first exploit attempt 12 hours and 31 minutes after the GitHub advisory went live; attackers used the vision-language image loader as a generic HTTP SSRF primitive to port-scan internal networks behind the model server, hit AWS Instance Metadata Service (IMDS), Redis, MySQL, a secondary HTTP admin interface, and an out-of-band DNS exfiltration endpoint, all in a single eight-minute session
- **Root Cause:** `load_image()` in `lmdeploy/vl/utils.py` fetches arbitrary URLs without validating internal or private IP ranges; fix in 0.12.3 adds URL validation and IP filtering
- **CVE:** CVE-2026-33626 (CVSS 7.5)
- **Sources:** [The Hacker News](https://thehackernews.com/2026/04/lmdeploy-cve-2026-33626-flaw-exploited.html), [Sysdig](https://www.sysdig.com/blog/cve-2026-33626-how-attackers-exploited-lmdeploy-llm-inference-engines-in-12-hours), [GBHackers](https://gbhackers.com/attackers-exploit-lmdeploy-flaw/), [SC Media](https://www.scworld.com/brief/lmdeploy-vulnerability-exploited-in-real-time-highlighting-ai-infrastructure-risks), [SentinelOne CVE Profile](https://www.sentinelone.com/vulnerability-database/cve-2026-33626/), [Vulert](https://vulert.com/blog/lmdeploy-cve-2026-33626-ssrf/)

### 2026-04-21 - CanisterSprawl Self-Propagating npm Worm via Namastex Labs and pgserve

- **Target:** Namastex Labs npm publisher namespaces and pgserve (embedded PostgreSQL for Node.js development)
- **Impact:** At least 16 malicious package versions across linked namespaces; pgserve releases 1.1.11, 1.1.12, and 1.1.13 (April 21 starting 22:14 UTC) carried a postinstall hook that harvested npm publish tokens, AWS, GCP, Azure, and Kubernetes credentials, SSH keys, and AI tooling configuration, then republished poisoned versions of every package the victim could publish; if a PyPI token was found, the worm jumped to PyPI; data exfiltrated both to a webhook (`telemetry.api-monitor.com`) and to ICP canister `cjn37-uyaaa-aaaac-qgnva-cai.raw.icp0.io`
- **Root Cause:** Compromised maintainer credentials at Namastex Labs (a vendor of agentic AI tooling) plus npm's allowance of postinstall hooks; ICP canister IDs cannot be removed via registrar takedowns or DNS sinkholing, blunting law-enforcement response
- **Sources:** [StepSecurity](https://www.stepsecurity.io/blog/pgserve-compromised-on-npm-malicious-versions-harvest-credentials), [Socket](https://socket.dev/blog/namastex-npm-packages-compromised-canisterworm), [The Hacker News](https://thehackernews.com/2026/04/self-propagating-supply-chain-worm.html), [The Register](https://www.theregister.com/2026/04/22/another_npm_supply_chain_attack/), [BleepingComputer](https://www.bleepingcomputer.com/news/security/new-npm-supply-chain-attack-self-spreads-to-steal-auth-tokens/), [SC Media](https://www.scworld.com/news/namastex-npm-packages-compromised-canisterworm-supply-chain-attack), [Cloud Security Alliance Lab](https://labs.cloudsecurityalliance.org/research/csa-research-note-npm-canistersprawl-supply-chain-worm-20260/), [Infosecurity Magazine](https://www.infosecurity-magazine.com/news/npm-supply-chain-worm-canister/)

### 2026-04-21 - Anthropic Claude Mythos Preview Accessed by Discord Group via Vendor Breach

- **Target:** Anthropic Claude Mythos Preview (cyber-offensive AI model held back under Project Glasswing limited-partner program)
- **Impact:** A small private Discord community gained continuous access to Mythos starting on launch day (April 7, 2026) by guessing the preview URL pattern, drawing on operational details from a Mercor breach three weeks earlier; group has not used Mythos for cyberattacks but has retained access; Anthropic stated it is investigating "unauthorized access to Claude Mythos Preview through one of our third-party vendor environments"
- **Root Cause:** A third-party contractor with operational knowledge confirmed URL guesses based on Anthropic's prior naming conventions; access controls relied on URL secrecy at the vendor environment rather than authenticated tenancy; Mercor breach context fed the recon
- **Sources:** [TechCrunch](https://techcrunch.com/2026/04/21/unauthorized-group-has-gained-access-to-anthropics-exclusive-cyber-tool-mythos-report-claims/), [Bloomberg](https://www.bloomberg.com/news/articles/2026-04-21/anthropic-s-mythos-model-is-being-accessed-by-unauthorized-users), [Fortune](https://fortune.com/2026/04/23/anthropic-mythos-leak-dario-amodei-ceo-cybersecurity-hackers-exploits-ai/), [Engadget](https://www.engadget.com/ai/anthropic-is-investigating-unauthorized-access-of-its-mythos-cybersecurity-tool-091017168.html), [Cybernews](https://cybernews.com/security/anthropic-mythos-ai-unauthorized-access/), [Hackread](https://hackread.com/discord-access-anthropic-claude-mythos-ai-breach/), [GovInfoSecurity](https://www.govinfosecurity.com/report-discord-group-uses-claudes-supposedly-secret-mythos-a-31484), [The Next Web](https://thenextweb.com/news/anthropic-mythos-unauthorized-access-vendor-breach)

### 2026-04-21 - Cloud Security Alliance Survey: AI Agent Incidents Common Across Enterprises

- **Target:** AI agent governance across 418 surveyed organizations (CSA / Token Security study, "Autonomous but Not Controlled: AI Agent Incidents Now Common in Enterprises")
- **Impact:** 65% of organizations experienced at least one AI agent-related cybersecurity incident in the past 12 months; 88% confirmed or suspected AI agent incidents; 82% had discovered previously unknown AI agents in their environment in the past year; 61% reported data exposure, 43% operational disruption, 35% financial loss, and 41% unintended actions in business processes from these incidents; only 21% had formal decommissioning processes in place
- **Root Cause:** Survey analysis; visibility and decommissioning gaps allow shadow AI agents to operate outside identity, network, and audit controls; healthcare incident rate reaches 92.7%
- **Sources:** [CSA Press Release](https://cloudsecurityalliance.org/press-releases/2026/04/21/new-cloud-security-alliance-survey-reveals-82-of-enterprises-have-unknown-ai-agents-in-their-environments), [BusinessWire](https://www.businesswire.com/news/home/20260421037010/en/New-Cloud-Security-Alliance-Survey-Reveals-82-of-Enterprises-Have-Unknown-AI-Agents-in-Their-Environments), [Infosecurity Magazine](https://www.infosecurity-magazine.com/news/unchecked-ai-agents-cause/), [ADVISOR Magazine](https://www.lifehealth.com/autonomous-but-not-controlled-ai-agent-incidents-now-common-in-enterprises/)

### 2026-04-21 - Flowise CSV Agent Prompt Injection RCE (CVE-2026-41264)

- **Target:** FlowiseAI Flowise (all versions before 3.1.0)
- **Impact:** Authenticated RCE on Flowise hosts through any chatflow that uses the CSV Agent node; an attacker prompts the LLM into emitting a Python script that the server then runs without sandboxing
- **Root Cause:** `run` method of the CSV_Agents class evaluates LLM-generated Python without proper sandboxing; bypasses earlier hardening for CVE-2026-41137; reported through Trend Micro Zero Day Initiative; fix in 3.1.0 disallows all imports inside CSV Agent
- **CVE:** CVE-2026-41264 (CVSS 7.0)
- **Sources:** [GitHub Advisory GHSA-3hjv-c53m-58jj](https://github.com/FlowiseAI/Flowise/security/advisories/GHSA-3hjv-c53m-58jj), [GitLab Advisory](https://advisories.gitlab.com/npm/flowise-components/CVE-2026-41264/), [THREATINT](https://cve.threatint.eu/CVE/CVE-2026-41264), [SC Media](https://www.scworld.com/brief/active-exploitation-of-max-severity-flowise-bug-threatens-broad-compromise)

### 2026-04-20 - Vercel Breach via Context.ai AI Tool Supply Chain

- **Target:** Vercel (Next.js hosting platform) via Context.ai (AI Office Suite)
- **Impact:** Attacker accessed Vercel Google Workspace and internal systems, including environment variables for a "limited subset" of customer projects; stolen data offered for $2M on BreachForums by a ShinyHunters persona; downstream crypto projects scrambled to rotate API keys
- **Root Cause:** Context.ai employee infected with Lumma Stealer in February 2026; attacker abused the Context.ai Google Workspace OAuth application that a Vercel employee had granted "Allow All" enterprise scopes; OAuth token replay escalated into the Vercel tenant
- **Sources:** [TechCrunch](https://techcrunch.com/2026/04/20/app-host-vercel-confirms-security-incident-says-customer-data-was-stolen-via-breach-at-context-ai/), [The Hacker News](https://thehackernews.com/2026/04/vercel-breach-tied-to-context-ai-hack.html), [The Register](https://www.theregister.com/2026/04/20/vercel_context_ai_security_incident/), [Vercel Bulletin](https://vercel.com/kb/bulletin/vercel-april-2026-security-incident), [OX Security](https://www.ox.security/blog/vercel-context-ai-supply-chain-attack-breachforums/), [Trend Micro](https://www.trendmicro.com/en_us/research/26/d/vercel-breach-oauth-supply-chain.html), [CoinDesk](https://www.coindesk.com/tech/2026/04/20/hack-at-vercel-sends-crypto-developers-scrambling-to-lock-down-api-keys), [Tom's Hardware](https://www.tomshardware.com/tech-industry/cyber-security/vercel-breached-after-employee-grants-ai-tool-unrestricted-access-to-google-workspace)

### 2026-04-17 - FastGPT Authentication and Password Change NoSQL Injection

- **Target:** FastGPT AI agent building platform (before v4.14.9.5)
- **Impact:** Unauthenticated attacker can log in as any user including root via MongoDB operator injection on the password field; authenticated attacker can bypass old-password verification to take over any account
- **Root Cause:** TypeScript type assertion without runtime validation on password login endpoint; NoSQL operator injection in password change endpoint
- **CVE:** CVE-2026-40351 (CVSS 9.8), CVE-2026-40352 (CVSS 8.8)
- **Sources:** [TheHackerWire CVE-2026-40351](https://www.thehackerwire.com/vulnerability/CVE-2026-40351/), [TheHackerWire CVE-2026-40352](https://www.thehackerwire.com/vulnerability/CVE-2026-40352/)

### 2026-04-16 - Anthropic MCP Systemic STDIO Design RCE

- **Target:** Anthropic Model Context Protocol reference SDKs (Python, TypeScript, Java, Rust) and 200,000+ downstream instances
- **Impact:** Arbitrary command execution on any MCP host via STDIO transport; OX Security demonstrated takeover of six production platforms and 30+ RCE reports across projects including LiteLLM, LangChain, Flowise, GPT Researcher, Agent Zero, and Windsurf; 11 CVEs assigned to downstream projects
- **Root Cause:** MCP STDIO transport accepts arbitrary command strings and passes them to subprocess execution with no validation, sanitization, or sandboxing; commands execute even when process startup fails; Anthropic declined to modify the protocol and said sanitization is the developer's responsibility
- **CVE:** CVE-2025-65720, CVE-2026-30623, CVE-2026-30624, CVE-2026-40933 (Flowise MCP Adapters, CVSS 10.0) and 7+ others
- **Sources:** [OX Security](https://www.ox.security/blog/the-mother-of-all-ai-supply-chains-critical-systemic-vulnerability-at-the-core-of-the-mcp/), [The Hacker News](https://thehackernews.com/2026/04/anthropic-mcp-design-vulnerability.html), [The Register](https://www.theregister.com/2026/04/16/anthropic_mcp_design_flaw/), [CSO Online](https://www.csoonline.com/article/4159889/rce-by-design-mcp-architectural-choice-haunts-ai-agent-ecosystem.html), [Infosecurity Magazine](https://www.infosecurity-magazine.com/news/systemic-flaw-mcp-expose-150/), [TechRadar](https://www.techradar.com/pro/security/this-is-not-a-traditional-coding-error-experts-flag-potentially-critical-security-issues-at-the-heart-of-anthropics-mcp-exposes-150-million-downloads-and-thousands-of-servers-to-complete-takeover), [GitHub Advisory CVE-2026-40933](https://github.com/advisories/GHSA-c9gw-hvqq-f33r)

### 2026-04-15 - LiteLLM OIDC Userinfo Cache Authentication Bypass

- **Target:** LiteLLM (BerriAI LLM gateway) deployments with JWT auth enabled
- **Impact:** Unauthenticated attacker can craft a token whose first 20 characters collide with a cached legitimate token, inheriting that user's identity and permissions across the gateway
- **Root Cause:** OIDC userinfo cache keyed on token[:20] instead of the full token or a secure hash; JWTs produced by the same signing algorithm share the same header prefix
- **CVE:** CVE-2026-35030 (CVSS 9.4)
- **Sources:** [LiteLLM Advisory](https://docs.litellm.ai/blog/security-hardening-april-2026), [GitLab Advisory](https://advisories.gitlab.com/pkg/pypi/litellm/CVE-2026-35030/), [GitHub Advisory](https://github.com/advisories/GHSA-jjhc-v7c2-5hh6), [SecurityOnline](https://securityonline.info/litellm-security-vulnerability-auth-bypass-rce-patch/), [Wiz](https://www.wiz.io/vulnerability-database/cve/cve-2026-35030)

### 2026-04-15 - Copilot Studio ShareLeak and Agentforce PipeLeak Form-Based Prompt Injection

- **Target:** Microsoft Copilot Studio and Salesforce Agentforce
- **Impact:** Attackers fill public-facing SharePoint or Web-to-Lead form fields with a fake system-role payload; hijacked agents query connected data sources in bulk and email the results to an attacker address with no volume cap, no Human-in-the-Loop prompt, and no trace shown to the employee who triggered the agent; Capsule Security reports the email channel remains exploitable on Agentforce Sub-Agents (formerly Custom Topics) even after Salesforce's remediation
- **Root Cause:** Untrusted form inputs concatenated directly into agent context windows; agents simultaneously hold read access to CRM/SharePoint data and authority to send outbound email
- **CVE:** CVE-2026-21520 (CVSS 7.5, Copilot Studio ShareLeak); PipeLeak has no CVE assigned
- **Sources:** [VentureBeat](https://venturebeat.com/security/microsoft-salesforce-copilot-agentforce-prompt-injection-cve-agent-remediation-playbook), [Dark Reading](https://www.darkreading.com/cloud-security/microsoft-salesforce-patch-ai-agent-data-leak-flaws), [CSO Online](https://www.csoonline.com/article/4159079/copilot-and-agentforce-fall-to-form-based-prompt-injection-tricks.html), [NVD CVE-2026-21520](https://nvd.nist.gov/vuln/detail/CVE-2026-21520), [PointGuard AI](https://www.pointguardai.com/ai-security-incidents/copilot-studio-leak-the-assistant-that-overshared-cve-2026-21520)

### 2026-04-15 - Claude Code, Gemini CLI, Copilot Agent Hijacked via GitHub Comments

- **Target:** Anthropic Claude Code Security Review, Google Gemini CLI Action, GitHub Copilot Agent (all GitHub Actions integrations)
- **Impact:** Prompt injection via PR titles, issue descriptions, and comments lets attackers execute arbitrary commands in the Actions runner, steal Anthropic and Gemini API keys, GitHub tokens, and any repository or organization secret available to the workflow
- **Root Cause:** Each agent ingests untrusted GitHub comment content as authoritative instructions with no separation between policy and data; all three vendors paid bug bounties ($100 Anthropic, $500 GitHub, undisclosed Google) but none assigned CVEs or published advisories, leaving downstream users unaware
- **Sources:** [The Register](https://www.theregister.com/2026/04/15/claude_gemini_copilot_agents_hijacked/), [SecurityWeek](https://www.securityweek.com/claude-code-gemini-cli-github-copilot-agents-vulnerable-to-prompt-injection-via-comments/), [The Next Web](https://thenextweb.com/news/ai-agents-hijacked-prompt-injection-bug-bounties-no-cve), [Cybernews](https://cybernews.com/security/ai-agents-github-prompt-injection-pattern/)

### 2026-04-15 - n8n Webhook Weaponization for Phishing Campaigns

- **Target:** n8n AI workflow automation platform (cloud-hosted webhooks)
- **Impact:** Attackers embed n8n-hosted webhook URLs in phishing emails; clicking opens a JavaScript CAPTCHA page on the trusted n8n domain that then downloads modified RMM tools such as Datto and ITarian; March 2026 volume of emails carrying these URLs was 686% higher than January 2025
- **Root Cause:** Public webhook URLs on trusted n8n infrastructure let attackers bypass email security filters that would otherwise block attacker-controlled domains; abuse first observed October 2025 and escalated through April 2026
- **Sources:** [The Hacker News](https://thehackernews.com/2026/04/n8n-webhooks-abused-since-october-2025.html), [Cisco Talos](https://blog.talosintelligence.com/the-n8n-n8mare/), [SC Media](https://www.scworld.com/brief/ai-workflow-platform-n8n-abused-for-phishing-and-device-fingerprinting), [TechRepublic](https://www.techrepublic.com/article/news-hackers-abuse-n8n-workflows-malware-delivery/)

### 2026-04-14 - OWASP GenAI Q1 2026 Exploit Round-up Report

- **Target:** AI ecosystem (sector-wide report covering January 1 - April 11, 2026)
- **Impact:** Documents the transition from theoretical risks to active exploitation; 520 reported tool misuse and privilege escalation incidents in 2026; prompt injection with 450 incidents; highlights Anthropic Claude abuse in the 150GB Mexican government data theft as the period's prominent case; notes a growing gap between traditional CVE-based vulnerability management and architectural AI risks that never receive CVE IDs
- **Root Cause:** Report synthesis; attackers target agent identities, orchestration layers, and supply chains rather than just model outputs
- **Sources:** [OWASP GenAI Q1 2026 Report](https://genai.owasp.org/2026/04/14/owasp-genai-exploit-round-up-report-q1-2026/)

### 2026-04-13 - Malicious LLM Router Research Reveals Credential and Crypto Theft

- **Target:** 428 public AI API routers tested by UCSB/UCSD researchers (published arXiv April 8, amplified April 13)
- **Impact:** 26 routers injected malicious tool calls, 9 injected malicious code into agent outputs, 17 accessed researcher AWS credentials, and at least one drained ETH from a researcher-controlled wallet; one client reportedly lost $500,000 in crypto to a malicious router; attacks include payload injection (AC-1), secret exfiltration (AC-2), dependency rewriting, and adaptive evasion that activates only in autonomous "YOLO mode"
- **Root Cause:** LLM routers operate as opaque man-in-the-middle intermediaries between clients and model providers; agents accept rewritten tool calls as trusted output
- **Sources:** [ArXiv paper](https://arxiv.org/html/2604.08407v1), [CoinDesk](https://www.coindesk.com/tech/2026/04/13/ai-agents-are-set-to-power-crypto-payments-but-a-hidden-flaw-could-expose-wallets), [Risky Business](https://news.risky.biz/risky-bulletin-malicious-llm-proxy-routers-found-in-the-wild/), [CCN](https://www.ccn.com/news/crypto/will-ai-steal-bitcoin-research-malicious-llm-routers-crypto-theft/), [OECD AI Incident Database](https://oecd.ai/en/incidents/2026-04-10-d6e2)

### 2026-04-13 - Marimo Pre-Auth RCE Weaponized to Deploy NKAbuse via Hugging Face

- **Target:** Marimo Python reactive notebook platform (all versions up to 0.20.4)
- **Impact:** 662 exploit events from 11 source IPs across 10 countries between April 11 and 14, 2026; reverse shells, credential theft, DNS exfiltration, lateral movement to PostgreSQL and Redis, and deployment of a new NKAbuse variant; malware installer hosted on a typosquat Hugging Face Space "vsccode-modetx" drops a Go ELF binary named kagent that uses the NKN blockchain for C2
- **Root Cause:** /terminal/ws WebSocket endpoint lacks the validate_auth() call present on other endpoints; unauthenticated attackers obtain a full PTY shell; exploitation began within 10 hours of public disclosure
- **CVE:** CVE-2026-39987 (CVSS 9.3)
- **Sources:** [Sysdig](https://www.sysdig.com/blog/cve-2026-39987-update-how-attackers-weaponized-marimo-to-deploy-a-blockchain-botnet-via-huggingface), [The Hacker News](https://thehackernews.com/2026/04/marimo-rce-flaw-cve-2026-39987.html), [BleepingComputer](https://www.bleepingcomputer.com/news/security/hackers-exploit-marimo-flaw-to-deploy-nkabuse-malware-from-hugging-face/), [Cybersecurity News](https://cybersecuritynews.com/attackers-spread-blockchain-based-backdoor-via-hugging-face/)

### 2026-04-13 - Nginx UI MCP Auth Bypass Under Active Exploitation

- **Target:** nginx-ui open-source Nginx management tool (before v2.3.4), approximately 2,600 publicly reachable instances
- **Impact:** Network-adjacent attacker can invoke 12 MCP tools (including nginx_config_add with auto-reload) in two HTTP requests; full Nginx server takeover, traffic interception, administrator credential harvest; chains with CVE-2026-27944 to extract the node_secret for ongoing session control
- **Root Cause:** /mcp_message endpoint handles every destructive tool invocation without the AuthRequired() middleware that protects the paired /mcp endpoint
- **CVE:** CVE-2026-33032 (CVSS 9.8)
- **Sources:** [The Hacker News](https://thehackernews.com/2026/04/critical-nginx-ui-vulnerability-cve.html), [BleepingComputer](https://www.bleepingcomputer.com/news/security/critical-nginx-ui-auth-bypass-flaw-now-actively-exploited-in-the-wild/), [Rapid7](https://www.rapid7.com/blog/post/etr-cve-2026-33032-nginx-ui-missing-mcp-authentication/), [Picus Security](https://www.picussecurity.com/resource/blog/cve-2026-33032-mcpwn-how-a-missing-middleware-call-in-nginx-ui-hands-attackers-full-web-server-takeover), [Security Affairs](https://securityaffairs.com/190841/hacking/cve-2026-33032-severe-nginx-ui-bug-grants-unauthenticated-server-access)

### 2026-04-11 - aws-mcp-server Unauthenticated RCE via Command Injection

- **Target:** aws-mcp-server (Model Context Protocol server for AWS CLI)
- **Impact:** Unauthenticated remote attackers can execute arbitrary code on affected installations; full compromise of the MCP server and any AWS credentials it holds
- **Root Cause:** Missing validation of a user-supplied string passed to a system call; improper handling of the allowed commands list
- **CVE:** CVE-2026-5058 (CVSS 9.8), CVE-2026-5059 (CVSS 9.8)
- **Sources:** [TheHackerWire CVE-2026-5058](https://www.thehackerwire.com/aws-mcp-server-remote-code-execution-via-command-injection-cve-2026-5058/), [TheHackerWire CVE-2026-5059](https://www.thehackerwire.com/aws-mcp-server-aws-cli-command-injection-rce/), [NVD CVE-2026-5059](https://nvd.nist.gov/vuln/detail/CVE-2026-5059), [Endor Labs](https://www.endorlabs.com/learn/classic-vulnerabilities-meet-ai-infrastructure-why-mcp-needs-appsec)

### 2026-04-10 - Red Hat OpenShift AI odh-dashboard Kubernetes Token Disclosure

- **Target:** Red Hat OpenShift AI (odh-dashboard component)
- **Impact:** Kubernetes Service Account tokens disclosed via a NodeJS endpoint; disclosed tokens usable to authenticate to the Kubernetes API and reach cluster resources
- **Root Cause:** Insecure handling/exposure of token data on a NodeJS endpoint in odh-dashboard
- **CVE:** CVE-2026-5483 (CVSS 8.5)
- **Sources:** [TheHackerWire](https://www.thehackerwire.com/red-hat-openshift-ai-odh-dashboard-kubernetes-token-disclosure-cve-2026-5483/), [Red Hat RHSA-2026:3713](https://access.redhat.com/errata/RHSA-2026:3713), [CSO Online](https://www.csoonline.com/article/4067305/red-hat-openshift-ai-weakness-allows-full-cluster-compromise-warns-advisory.html)

### 2026-04-08 - PraisonAI Template Injection in Agent Tool Definitions

- **Target:** PraisonAI (multi-agent teams framework, PyPI) prior to v4.5.115
- **Impact:** Arbitrary code execution via template expressions injected into agent.start() input processed by create_agent_centric_tools()
- **Root Cause:** Unescaped user input passed directly to template-rendering tools such as acp_create_file
- **CVE:** CVE-2026-39891 (CVSS 8.8)
- **Sources:** [GitLab Advisory](https://advisories.gitlab.com/pkg/pypi/praisonai/CVE-2026-39891/), [NVD](https://nvd.nist.gov/vuln/detail/CVE-2026-39891), [TheHackerWire](https://www.thehackerwire.com/praisonai-template-injection-via-agent-input-cve-2026-39891/)

### 2026-04-08 - UNC1069 Contagious Interview Cross-Ecosystem Package Campaign

- **Target:** npm, PyPI, Go, Rust, Packagist registries (developer tooling impersonation)
- **Impact:** 1,700+ malicious packages identified since January 2025 fueling espionage and financial theft; SEAL blocked 164 UNC1069 domains impersonating Teams and Zoom between Feb 6 and Apr 7, 2026
- **Root Cause:** DPRK-linked UNC1069 (overlaps BlueNoroff, Sapphire Sleet, Stardust Chollima) publishing malware-loader packages disguised as dev tooling; delivery via fake Zoom/Teams ClickFix lures after LinkedIn/Telegram/Slack social engineering
- **Sources:** [The Hacker News](https://thehackernews.com/2026/04/n-korean-hackers-spread-1700-malicious.html), [Vulert](https://vulert.com/blog/north-korea-malicious-packages-npm-pypi-go-rust/), [Cybersecuritywaala](https://cybersecuritywaala.com/news/north-korea-linked-malicious-packages-in-registries/)

### 2026-04-07 - AWS Bedrock AgentCore "Agent God Mode" Cross-Agent Memory Access

- **Target:** AWS Bedrock AgentCore starter toolkit
- **Impact:** Default IAM roles auto-created by the toolkit scope permissions to wildcard, letting any agent read or poison memory belonging to every other agent in the AWS account; enables privilege escalation and cross-tenant data exfiltration
- **Root Cause:** Starter toolkit auto-create logic issues IAM policies with GetMemory and RetrieveMemoryRecords on resource "*" instead of per-resource scoping
- **Sources:** [Unit 42](https://unit42.paloaltonetworks.com/exploit-of-aws-agentcore-iam-god-mode/), [Unit 42 sandbox escape](https://unit42.paloaltonetworks.com/bypass-of-aws-sandbox-network-isolation-mode/)

### 2026-04-07 - Flowise AI Agent Builder RCE Actively Exploited in the Wild

- **Target:** Flowise (open-source AI agent builder) versions up to 3.0.5
- **Impact:** Unauthenticated remote code execution via CustomMCP node; 12,000-15,000+ instances exposed on the internet; first in-the-wild exploitation from a Starlink IP
- **Root Cause:** CustomMCP node parses user-supplied mcpServerConfig and executes JavaScript without validation, exposing child_process and fs modules under full Node.js runtime privileges
- **CVE:** CVE-2025-59528 (CVSS 10.0)
- **Sources:** [The Hacker News](https://thehackernews.com/2026/04/flowise-ai-agent-builder-under-active.html), [BleepingComputer](https://www.bleepingcomputer.com/news/security/max-severity-flowise-rce-vulnerability-now-exploited-in-attacks/), [Security Affairs](https://securityaffairs.com/190471/security/attackers-exploit-critical-flowise-flaw-cve-2025-59528-for-remote-code-execution.html), [CSO Online](https://www.csoonline.com/article/4155680/hackers-exploit-a-critical-flowise-flaw-affecting-thousands-of-ai-workflows.html)

### 2026-04-03 - PraisonAI Gateway Unauthenticated Agent Control

- **Target:** PraisonAI Gateway
- **Impact:** Any network client can enumerate agents and send arbitrary messages via unauthenticated WebSocket
- **Root Cause:** No authentication on WebSocket and agent topology endpoints
- **CVE:** CVE-2026-34952 (CVSS 9.1)
- **Sources:** [TheHackerWire](https://www.thehackerwire.com/praisonai-gateway-unauthenticated-agent-control/)

### 2026-04-03 - Azure MCP Server Authentication Flaw

- **Target:** Microsoft Azure MCP Server
- **Impact:** Sensitive data accessible without valid credentials; no patch available at disclosure
- **Root Cause:** Improper authentication implementation
- **CVE:** CVE-2026-32211 (CVSS 9.1)
- **Sources:** [WindowsNews](https://windowsnews.ai/article/cve-2026-32211-critical-azure-mcp-server-authentication-flaw-exposes-sensitive-data-cvss-91.409622)

### 2026-04-02 - Meta Pauses Mercor Partnership

- **Target:** Meta / Mercor
- **Impact:** All Meta contracts with Mercor suspended indefinitely; AI training data secrets at risk
- **Root Cause:** Response to Mercor breach from LiteLLM supply chain attack
- **Sources:** [Social Media Today](https://www.socialmediatoday.com/news/meta-pauses-all-contracts-with-mercor-after-breach/816663/), [Benzinga](https://www.benzinga.com/markets/tech/26/04/51652163/meta-halts-mercor-work-breach-openai-investigates-report)

### 2026-04-01 - Drift Protocol $285M Exploit

- **Target:** Drift Protocol (Solana DeFi)
- **Impact:** $285M stolen in 12 minutes via fictitious CarbonVote Token, oracle manipulation, and zero-timelock Security Council migration
- **Root Cause:** Six-month social engineering campaign by UNC4736 (DPRK) targeting multisig signers; pre-signed hidden authorizations
- **Sources:** [TRM Labs](https://www.trmlabs.com/resources/blog/north-korean-hackers-attack-drift-protocol-in-285-million-heist), [Elliptic](https://www.elliptic.co/blog/drift-protocol-exploited-for-286-million-in-suspected-dprk-linked-attack), [The Hacker News](https://thehackernews.com/2026/04/285-million-drift-hack-traced-to-six.html)

### 2026-03-30 - ChatGPT Hidden DNS Exfiltration Channel

- **Target:** OpenAI ChatGPT code execution sandbox
- **Impact:** A single prompt could silently exfiltrate user messages, uploaded files, and other sandbox contents over DNS; same path usable to establish a remote shell inside the Linux runtime
- **Root Cause:** Sandbox blocked direct network requests but left recursive DNS resolution unrestricted; data encoded into DNS subdomain labels escaped all other network controls
- **Sources:** [Check Point Research](https://research.checkpoint.com/2026/chatgpt-data-leakage-via-a-hidden-outbound-channel-in-the-code-execution-runtime/), [The Register](https://www.theregister.com/2026/03/30/openai_chatgpt_dns_data_snuggling_flaw/), [eSecurity Planet](https://www.esecurityplanet.com/artificial-intelligence/check-point-research-reveals-chatgpt-data-exfiltration-flaw/), [Cybersecurity News](https://cybersecuritynews.com/chatgpt-vulnerability/)

### 2026-03-31 - Mercor Data Breach via LiteLLM Supply Chain

- **Target:** Mercor ($10B AI hiring startup)
- **Impact:** 40K+ people affected; Lapsus$ claims 4TB of data including PII, video interviews, credentials, source code; class action filed
- **Root Cause:** Cascading supply chain: TeamPCP compromised Trivy, stole LiteLLM credentials, published poisoned PyPI packages
- **Sources:** [Fortune](https://fortune.com/2026/04/02/mercor-ai-startup-security-incident-10-billion/), [TechCrunch](https://techcrunch.com/2026/03/31/mercor-says-it-was-hit-by-cyberattack-tied-to-compromise-of-open-source-litellm-project/), [SecurityWeek](https://www.securityweek.com/mercor-hit-by-litellm-supply-chain-attack/)

### 2026-03-31 - Axios npm Supply Chain Attack

- **Target:** Axios (70-100M weekly npm downloads)
- **Impact:** Malicious versions tagged "latest" delivered cross-platform RAT via dependency "plain-crypto-js"
- **Root Cause:** Social engineering of lead maintainer's npm credentials by Sapphire Sleet / UNC1069 (DPRK)
- **Sources:** [Microsoft Security Blog](https://www.microsoft.com/en-us/security/blog/2026/04/01/mitigating-the-axios-npm-supply-chain-compromise/), [The Hacker News](https://thehackernews.com/2026/03/axios-supply-chain-attack-pushes-cross.html), [Elastic Security Labs](https://www.elastic.co/security-labs/axios-one-rat-to-rule-them-all)

### 2026-03-31 - Cisco Source Code Stolen via Trivy Breach

- **Target:** Cisco internal dev environment
- **Impact:** 300+ GitHub repos cloned including AI product source code and customer code from banks and US government agencies
- **Root Cause:** Credentials harvested during TeamPCP's Trivy compromise used to access Cisco dev infrastructure
- **Sources:** [BleepingComputer](https://www.bleepingcomputer.com/news/security/cisco-source-code-stolen-in-trivy-linked-dev-environment-breach/), [SOCRadar](https://socradar.io/blog/trivy-cisco-breach-shinyhunters/)

### 2026-03-27 - Telnyx PyPI Supply Chain Compromise

- **Target:** Telnyx Python SDK
- **Impact:** Credential harvester concealed in WAV audio file frame data; malicious code injected into telnyx/_client.py
- **Root Cause:** Telnyx PyPI token stolen during LiteLLM compromise; TeamPCP cascading attack
- **Sources:** [The Hacker News](https://thehackernews.com/2026/03/teampcp-pushes-malicious-telnyx.html), [Akamai](https://www.akamai.com/blog/security-research/telnyx-pypi-2026-teampcp-supply-chain-attacks), [Trend Micro](https://www.trendmicro.com/en_us/research/26/c/teampcp-telnyx-attack-marks-a-shift-in-tactics.html)

### 2026-03-24 - LiteLLM Supply Chain Attack by TeamPCP

- **Target:** LiteLLM (3.4M daily PyPI downloads)
- **Impact:** Three-stage malware: credential theft, K8s lateral movement, persistent systemd backdoor; fork bomb bug triggered discovery; available ~3 hours before quarantine
- **Root Cause:** PyPI credentials stolen from LiteLLM CI environment during Trivy compromise
- **Sources:** [LiteLLM Official](https://docs.litellm.ai/blog/security-update-march-2026), [Snyk](https://snyk.io/blog/poisoned-security-scanner-backdooring-litellm/), [ReversingLabs](https://www.reversinglabs.com/blog/teampcp-supply-chain-attack-spreads)

### 2026-03-23 - Checkmarx KICS GitHub Actions Compromise

- **Target:** Checkmarx KICS, AST GitHub Action, OpenVSX extensions
- **Impact:** 35 tags hijacked; credential stealer exfiltrated secrets encrypted to attacker server checkmarx.zone
- **Root Cause:** cx-plugins-releases service account compromised via credentials from Trivy attack
- **Sources:** [Wiz](https://www.wiz.io/blog/teampcp-attack-kics-github-action), [Checkmarx](https://checkmarx.com/blog/checkmarx-security-update/), [The Hacker News](https://thehackernews.com/2026/03/teampcp-hacks-checkmarx-github-actions.html)

### 2026-03-20 - CanisterWorm npm Worm by TeamPCP

- **Target:** npm ecosystem (66+ packages)
- **Impact:** 141 malicious package artifacts; persistent systemd backdoor; first npm worm to use decentralized ICP as C2, making takedown impossible
- **Root Cause:** Credentials harvested from Trivy compromise used to publish malicious versions
- **CVE:** CVE-2026-33634
- **Sources:** [Aikido](https://www.aikido.dev/blog/teampcp-deploys-worm-npm-trivy-compromise), [The Hacker News](https://thehackernews.com/2026/03/trivy-supply-chain-attack-triggers-self.html), [Mend.io](https://www.mend.io/blog/canisterworm-the-self-spreading-npm-attack-that-uses-a-decentralized-server-to-stay-alive/)

### 2026-03-19 - Trivy GitHub Action Compromise by TeamPCP

- **Target:** Aqua Security Trivy scanner
- **Impact:** 76 of 77 version tags redirected to malicious commits; Runner.Worker memory dumped; SSH, cloud, K8s secrets harvested; 1,000+ SaaS environments compromised downstream
- **Root Cause:** Service account compromise via prior incompletely remediated incident
- **Sources:** [Wiz](https://www.wiz.io/blog/trivy-compromised-teampcp-supply-chain-attack), [Aqua Security](https://www.aquasec.com/blog/trivy-supply-chain-attack-what-you-need-to-know/), [Unit 42](https://unit42.paloaltonetworks.com/teampcp-supply-chain-attacks/)

### 2026-03-18 - Meta Sev 1 Rogue AI Agent Incident

- **Target:** Meta internal AI systems
- **Impact:** AI agent posted unauthorized technical advice; another employee followed it, exposing massive company and user data to unauthorized engineers for two hours
- **Root Cause:** AI agent acted autonomously without human-in-the-loop confirmation
- **Sources:** [TechCrunch](https://techcrunch.com/2026/03/18/meta-is-having-trouble-with-rogue-ai-agents/), [The Information](https://www.theinformation.com/articles/inside-meta-rogue-ai-agent-triggers-security-alert), [Engadget](https://www.engadget.com/ai/a-meta-agentic-ai-sparked-a-security-incident-by-acting-without-permission-224013384.html)

### 2026-03-17 - Langflow RCE Exploited Within 20 Hours

- **Target:** Langflow (all versions through 1.8.1)
- **Impact:** Attackers built working exploits within 20 hours; harvested OpenAI, Anthropic, and AWS API keys from compromised instances
- **Root Cause:** POST endpoint accepts arbitrary Python code in node definitions, executed server-side without sandboxing
- **CVE:** CVE-2026-33017 (CVSS 9.3)
- **Sources:** [The Hacker News](https://thehackernews.com/2026/03/critical-langflow-flaw-cve-2026-33017.html), [Sysdig](https://www.sysdig.com/blog/cve-2026-33017-how-attackers-compromised-langflow-ai-pipelines-in-20-hours), [Barrack AI](https://blog.barrack.ai/langflow-exec-rce-cve-2026-33017/)

### 2026-03-17 - LangChain Core Path Traversal

- **Target:** LangChain Core
- **Impact:** Arbitrary file access via prompt-loading API
- **Root Cause:** Path traversal in prompt loading
- **CVE:** CVE-2026-34070 (CVSS 7.5)
- **Sources:** [GitLab Advisory](https://advisories.gitlab.com/pkg/pypi/langchain-core/CVE-2026-34070/)

### 2026-03-11 - UNC6426 nx npm to AWS Admin Takeover

- **Target:** nx monorepo tooling / enterprise AWS
- **Impact:** Attacker cloned GitHub repos, extracted CI/CD secrets, achieved full AWS admin; S3 buckets accessed, EC2/RDS terminated
- **Root Cause:** Compromised nx npm package delivered QUIETVAULT credential stealer; GitHub-to-AWS OIDC trust chain abuse
- **Sources:** [The Hacker News](https://thehackernews.com/2026/03/unc6426-exploits-nx-npm-supply-chain.html), [CSA Labs](https://labs.cloudsecurityalliance.org/research/briefing-csa-research-note-oidc-trust-chain-abuse-cloud-take/)

### 2026-03-10 - Meta Acquires Moltbook (OpenClaw) After Security Crises

- **Target:** Moltbook/OpenClaw platform
- **Impact:** Meta acquired the platform after exposed database (1.5M API tokens, 35K emails), 1,184+ malicious skills distributing Atomic Stealer, and 135K exposed instances
- **Root Cause:** Unsecured database; no skill vetting; WebSocket accepting unauthenticated localhost connections
- **CVE:** CVE-2026-25593, CVE-2026-25253 (CVSS 8.8), and 6 others
- **Sources:** [Wiz](https://www.wiz.io/blog/exposed-moltbook-database-reveals-millions-of-api-keys), [eSecurity Planet](https://www.esecurityplanet.com/threats/hundreds-of-malicious-skills-found-in-openclaws-clawhub/), [BleepingComputer](https://www.bleepingcomputer.com/news/security/clawjacked-attack-let-malicious-websites-hijack-openclaw-to-steal-data/)

### 2026-03 - ROME AI Agent Escapes Sandbox, Mines Cryptocurrency

- **Target:** ROME agent (Alibaba-affiliated)
- **Impact:** Agent accessed GPU resources to mine crypto; created reverse SSH tunnel bypassing security processes; behavior was spontaneous, not prompted
- **Root Cause:** During reinforcement learning, agent spontaneously produced unauthorized behaviors including system tool invocation outside boundaries
- **Sources:** [Axios](https://www.axios.com/2026/03/07/ai-agents-rome-model-cryptocurrency), [Live Science](https://www.livescience.com/technology/artificial-intelligence/an-experimental-ai-agent-broke-out-of-its-testing-environment-and-mined-crypto-without-permission)

### 2026-02-22 - OpenClaw Agent Deletes 200+ Emails at Meta

- **Target:** OpenClaw / Meta
- **Impact:** Meta AI safety director lost 200+ emails; agent ignored repeated STOP commands; post received ~9M views on X
- **Root Cause:** Agent ran out of working memory and condensed prior messages, discarding the instruction to confirm before acting
- **Sources:** [Fast Company](https://www.fastcompany.com/91497841/meta-superintelligence-lab-ai-safety-alignment-director-lost-control-of-agent-deleted-her-emails), [TechCrunch](https://techcrunch.com/2026/02/23/a-meta-ai-security-researcher-said-an-openclaw-agent-ran-amok-on-her-inbox/)

### 2026-02-20 - CyberStrikeAI FortiGate Mass Compromise

- **Target:** Fortinet FortiGate devices (600+ across 55 countries)
- **Impact:** Russian-speaking financially motivated actor used commercial GenAI services and the open-source CyberStrikeAI framework to brute-force exposed management ports and single-factor credentials; Team Cymru observed 21 CyberStrikeAI C2 IPs between Jan 20 and Feb 26, 2026
- **Root Cause:** No FortiGate CVE exploitation; AI orchestration let a low-skill actor scale reconnaissance, credential spraying, and post-compromise steps against exposed admin interfaces
- **Sources:** [AWS Security Blog](https://aws.amazon.com/blogs/security/ai-augmented-threat-actor-accesses-fortigate-devices-at-scale/), [The Hacker News](https://thehackernews.com/2026/02/ai-assisted-threat-actor-compromises.html), [The Hacker News CyberStrikeAI](https://thehackernews.com/2026/03/open-source-cyberstrikeai-deployed-in.html), [The Record](https://therecord.media/gen-ai-fortigate-hackers-russia), [CSO Online](https://www.csoonline.com/article/4136198/russian-group-uses-ai-to-exploit-weakly-protected-fortinet-firewalls-says-amazon.html), [SC Media](https://www.scworld.com/news/threat-group-leverages-llms-to-compromise-600-fortigate-firewalls)

### 2026-02-09 - Clinejection Supply Chain Attack

- **Target:** Cline AI coding assistant (5M+ users)
- **Impact:** ~4,000 developer machines compromised in 8-hour window; malicious cline@2.3.0 published to npm installing OpenClaw globally
- **Root Cause:** Prompt injection in GitHub issue title tricked Claude-powered triage bot; GitHub Actions cache poisoning (Cacheract)
- **Sources:** [Snyk](https://snyk.io/blog/cline-supply-chain-attack-prompt-injection-github-actions/), [The Hacker News](https://thehackernews.com/2026/02/cline-cli-230-supply-chain-attack.html), [Adnan Khan](https://adnanthekhan.com/posts/clinejection/)

### 2026-02-04 - MCP TypeScript SDK Cross-Client Data Leak

- **Target:** MCP TypeScript SDK (v1.10.0-1.25.3)
- **Impact:** Tool results, resource content, and error messages routed to wrong client in multi-tenant deployments
- **Root Cause:** Race condition in response multiplexing of StreamableHTTPServerTransport
- **CVE:** CVE-2026-25536 (CVSS 7.1)
- **Sources:** [VulnerableMCP](https://vulnerablemcp.info/vuln/cve-2026-25536-sdk-cross-client-data-leak.html)

### 2026-01-23 - Langflow Active Exploitation Deploys Flodrix Botnet

- **Target:** Langflow (versions up to 1.6.9)
- **Impact:** Complete account takeover and RCE via malicious webpage visit; Flodrix botnet deployed for DDoS and data exfiltration
- **Root Cause:** Overly permissive CORS, no CSRF protection on token refresh, code validation endpoint allows execution
- **CVE:** CVE-2025-34291 (CVSS 9.4)
- **Sources:** [Obsidian Security](https://www.obsidiansecurity.com/blog/cve-2025-34291-critical-account-takeover-and-rce-vulnerability-in-the-langflow-ai-agent-workflow-platform), [CrowdSec](https://www.crowdsec.net/vulntracking-report/cve-2025-34291)

### 2026-01-21 - Claude Code API Key Exfiltration

- **Target:** Anthropic Claude Code
- **Impact:** Malicious settings file redirects API requests to attacker endpoint before trust prompt; stolen API key grants access to team's shared resources
- **Root Cause:** API requests issued before trust confirmation prompt
- **CVE:** CVE-2026-21852 (CVSS 5.3)
- **Sources:** [Check Point Research](https://research.checkpoint.com/2026/rce-and-api-token-exfiltration-through-claude-code-project-files-cve-2025-59536/), [GitHub Advisory](https://github.com/advisories/GHSA-jh7p-qr78-84p7)

### 2026-01-20 - Anthropic Git MCP Server Vulnerability Chain

- **Target:** Anthropic Git MCP Server
- **Impact:** Path traversal, argument injection, and RCE when chained; any directory turned into Git repo; arbitrary file overwrite
- **Root Cause:** Missing path validation, unsanitized arguments in git_diff/git_checkout
- **CVE:** CVE-2025-68143, CVE-2025-68144, CVE-2025-68145
- **Sources:** [The Hacker News](https://thehackernews.com/2026/01/three-flaws-in-anthropic-mcp-git-server.html), [SecurityWeek](https://www.securityweek.com/anthropic-mcp-server-flaws-lead-to-code-execution-data-exposure/)

### 2026-01 - Step Finance AI Trading Agent Treasury Drain

- **Target:** Step Finance (Solana DeFi portfolio manager)
- **Impact:** ~$40M drained from treasury; 261,000+ SOL transferred; native token crashed ~97% from pre-hack levels; only ~$4.7M recovered; platform wound down operations
- **Root Cause:** Executive device compromise gave attackers wallet and fee-account access; trading agents held broad permissions across wallets, oracles, and trading endpoints with no scope isolation; 45.6% of surveyed teams reused shared API keys across agents
- **Sources:** [KuCoin](https://www.kucoin.com/blog/en-ai-trading-agent-vulnerability-2026-how-a-45m-crypto-security-breach-exposed-protocol-risks)

### 2026-01-08 - n8n "Ni8mare" CVSS 10.0 RCE

- **Target:** n8n workflow automation (~100K instances)
- **Impact:** Unauthenticated full server takeover; access to API credentials, OAuth tokens, CI/CD pipelines, payment processors
- **Root Cause:** Content-Type confusion in webhook processing overwrites req.body.files; no code execution sandboxing
- **CVE:** CVE-2026-21858 (CVSS 10.0), CVE-2026-21877 (CVSS 10.0)
- **Sources:** [The Hacker News](https://thehackernews.com/2026/01/n8n-warns-of-cvss-100-rce-vulnerability.html), [Cyera Research](https://www.cyera.com/research/ni8mare-unauthenticated-remote-code-execution-in-n8n-cve-2026-21858), [The Register](https://www.theregister.com/2026/01/08/n8n_rce_bug/)

---

## 2025 Incidents

### 2025-12 - LangChain "LangGrinch" Serialization Injection

- **Target:** LangChain Core (before 0.3.81)
- **Impact:** Secret exfiltration and potential RCE via LLM-influenced metadata containing reserved 'lc' key
- **Root Cause:** dumps()/dumpd() did not escape user-controlled dicts with reserved serialization marker
- **CVE:** CVE-2025-68664 (CVSS 9.3)
- **Sources:** [Cyata](https://cyata.ai/blog/langgrinch-langchain-core-cve-2025-68664/), [The Hacker News](https://thehackernews.com/2025/12/critical-langchain-core-vulnerability.html), [Orca Security](https://orca.security/resources/blog/cve-2025-68664-langchain-serialization-flaw/)

### 2025-12 - IDEsaster - 30+ Flaws Across AI Coding Tools

- **Target:** Cursor, Windsurf, Kiro.dev, Copilot, Zed, Roo Code, Cline, others
- **Impact:** Data exfiltration, RCE, and supply chain compromise across most popular AI IDEs; Windsurf vulnerable to persistent memory poisoning
- **Root Cause:** Systemic lack of input validation; persistent memory stores process untrusted content as trusted
- **CVE:** Multiple
- **Sources:** [The Hacker News](https://thehackernews.com/2025/12/researchers-uncover-30-flaws-in-ai.html), [Fortune](https://fortune.com/2025/12/15/ai-coding-tools-security-exploit-software/)

### 2025-12 - Copilot Studio Prompt Injection Data Leak

- **Target:** Microsoft Copilot Studio
- **Impact:** Credit card data leaked; business logic manipulated (booking trips at $0)
- **Root Cause:** No-code agent platform allows employees to build AI agents without robust input validation
- **Sources:** [Tenable](https://www.tenable.com/blog/microsoft-copilot-studio-security-risk-how-simple-prompt-injection-leaked-sensitive-data), [Security Boulevard](https://securityboulevard.com/2025/12/microsoft-copilot-studio-security-risk-how-simple-prompt-injection-leaked-credit-cards-and-booked-a-0-trip/)

### 2025-11 - ServiceNow Now Assist Second-Order Prompt Injection

- **Target:** ServiceNow Now Assist / Agentforce
- **Impact:** Low-privilege agent tricks higher-privilege agent into exporting case files to external URL; ServiceNow said system "works as intended"
- **Root Cause:** Default agent configs allow autonomous overrides; agents run with initiating user privilege
- **Sources:** [The Hacker News](https://thehackernews.com/2025/11/servicenow-ai-agents-can-be-tricked.html), [AppOmni](https://appomni.com/ao-labs/ai-agent-to-agent-discovery-prompt-injection/)

### 2025-11 - CrewAI "Uncrew" GitHub Token Exposure

- **Target:** CrewAI platform
- **Impact:** Single internal GitHub token with admin rights to all private repos exposed; CVSS 9.2
- **Root Cause:** Improper error handling exposed internal GitHub token
- **Sources:** [Noma Security](https://noma.security/blog/uncrew-the-risk-behind-a-leaked-internal-github-token-at-crewai/), [Security Boulevard](https://securityboulevard.com/2025/11/crewai-github-token-exposure-highlights-the-growing-risk-of-static-credentials-in-ai-systems/)

### 2025-11 - Claude Desktop Extensions RCE

- **Target:** Anthropic Claude Desktop (Chrome, iMessage, Apple Notes extensions)
- **Impact:** Command injection in three official Anthropic-written extensions; SSH keys, AWS credentials, browser passwords exposed; CVSS 8.9
- **Root Cause:** Unsanitized input handling; no sandboxing for Desktop Extensions
- **Sources:** [Koi AI](https://www.koi.ai/blog/promptjacking-the-critical-rce-in-claude-desktop-that-turn-questions-into-exploits), [CSO Online](https://www.csoonline.com/article/4129820/anthropics-dxt-poses-critical-rce-vulnerability-by-running-with-full-system-privileges.html)

### 2025-11-13 - GTG-1002 Chinese State-Sponsored AI-Orchestrated Espionage

- **Target:** ~30 global organizations (large tech companies, financial institutions, chemical manufacturers, government agencies)
- **Impact:** First publicly documented AI-orchestrated cyber espionage campaign; Claude Code executed 80-90% of tactical operations autonomously (reconnaissance, vulnerability discovery, exploitation, lateral movement, credential harvesting, analysis, exfiltration) with humans intervening only at decision gates; detected Sep 2025, disclosed Nov 13, 2025
- **Root Cause:** Operators jailbroke Claude Code by claiming to be employees of legitimate cybersecurity firms running defensive tests, then split malicious workflows into innocuous-looking subtasks to bypass safety training
- **Sources:** [Anthropic report (PDF)](https://assets.anthropic.com/m/ec212e6566a0d47/original/Disrupting-the-first-reported-AI-orchestrated-cyber-espionage-campaign.pdf), [Cybersecurity Dive](https://www.cybersecuritydive.com/news/anthropic-state-actor-ai-tool-espionage/805550/), [The Hacker News](https://thehackernews.com/2025/11/chinese-hackers-use-anthropics-ai-to.html), [The Register](https://www.theregister.com/2025/11/13/chinese_spies_claude_attacks/), [BlackFog](https://www.blackfog.com/gtg-1002-claude-hijacked-first-ai-led-cyberattack/), [ExtraHop](https://www.extrahop.com/blog/anthropic-reveals-the-first-ai-orchestrated-cyber-espionage-campaign), [AI Incident Database](https://incidentdatabase.ai/cite/1263/)

### 2025-11-04 - GitHub Copilot Filename Prompt Injection

- **Target:** GitHub Copilot Chat (Agent mode)
- **Impact:** Arbitrary instruction execution via extremely long filenames containing prompt injections; Microsoft declined to fix
- **Root Cause:** Copilot appends file names to user prompts without sanitization
- **Sources:** [Tenable TRA-2025-53](https://www.tenable.com/security/research/tra-2025-53)

### 2025-10 - Claude Code RCE via Hooks

- **Target:** Anthropic Claude Code
- **Impact:** RCE and API token exfiltration when developers clone untrusted repositories; hooks execute before trust dialog
- **Root Cause:** Startup trust dialog allowed code execution from project configs before user accepts
- **CVE:** CVE-2025-59536 (CVSS 8.7)
- **Sources:** [Check Point Research](https://research.checkpoint.com/2026/rce-and-api-token-exfiltration-through-claude-code-project-files-cve-2025-59536/), [Dark Reading](https://www.darkreading.com/application-security/flaws-claude-code-developer-machines-risk), [Cybernews](https://cybernews.com/security/claude-code-critical-vulnerability-enabled-rce/)

### 2025-10 - Langflow Account Takeover and RCE Chain

- **Target:** Langflow (versions up to 1.6.9)
- **Impact:** Complete instance compromise; all stored API keys exposed
- **Root Cause:** Overly permissive CORS + missing CSRF protection + code validation endpoint
- **CVE:** CVE-2025-34291 (CVSS 9.4)
- **Sources:** [Obsidian Security](https://www.obsidiansecurity.com/blog/cve-2025-34291-critical-account-takeover-and-rce-vulnerability-in-the-langflow-ai-agent-workflow-platform), [NVD](https://nvd.nist.gov/vuln/detail/CVE-2025-34291)

### 2025-09 - Salesforce Agentforce "ForcedLeak"

- **Target:** Salesforce Agentforce
- **Impact:** CRM data exfiltrated via indirect prompt injection through Web-to-Lead forms; CVSS 9.4
- **Root Cause:** Indirect prompt injection via user-submitted form data; expired domain still whitelisted in CSP
- **Sources:** [Noma Security](https://noma.security/blog/forcedleak-agent-risks-exposed-in-salesforce-agentforce/), [The Hacker News](https://thehackernews.com/2025/09/salesforce-patches-critical-forcedleak.html), [The Register](https://www.theregister.com/2025/09/26/salesforce_agentforce_forceleak_attack/)

### 2025-08-20 - Salesloft Drift OAuth Supply Chain Breach

- **Target:** Salesloft Drift / Salesforce / Google Workspace / Slack
- **Impact:** 700+ organizations compromised including Cloudflare, Google, Palo Alto Networks, Zscaler; CRM records, API keys, cloud credentials stolen
- **Root Cause:** UNC6395 stole OAuth tokens from Drift chatbot integration
- **Sources:** [Google Cloud Blog](https://cloud.google.com/blog/topics/threat-intelligence/data-theft-salesforce-instances-via-salesloft-drift), [The Hacker News](https://thehackernews.com/2025/09/salesloft-takes-drift-offline-after.html), [Cloudflare Blog](https://blog.cloudflare.com/response-to-salesloft-drift-incident/)

### 2025-08 - Claude Code InversePrompt Command Injection

- **Target:** Anthropic Claude Code (below v1.0.20)
- **Impact:** Whitelisted echo command used as injection vector; AI model helps reverse-engineer its own security
- **Root Cause:** Error in command parsing; echo whitelisted without sanitization
- **CVE:** CVE-2025-54794, CVE-2025-54795 (CVSS 8.7)
- **Sources:** [Cymulate](https://cymulate.com/blog/cve-2025-547954-54795-claude-inverseprompt/), [GitHub Advisory](https://github.com/anthropics/claude-code/security/advisories/GHSA-x56v-x2h6-7j34)

### 2025-08 - Claude Code WebSocket Auth Bypass

- **Target:** Claude Code extensions
- **Impact:** Malicious websites could read local files and execute code in Jupyter notebooks via unauthenticated local WebSocket
- **Root Cause:** Unauthenticated local WebSocket servers exposed to browser contexts
- **CVE:** CVE-2025-52882
- **Sources:** [Datadog Security Labs](https://securitylabs.datadoghq.com/articles/claude-mcp-cve-2025-52882/)

### 2025-08 - OpenAI Codex CLI Command Injection

- **Target:** OpenAI Codex CLI (before v0.23.0)
- **Impact:** Arbitrary command execution in user's security context; CI/automation runs at risk
- **Root Cause:** Codex implicitly trusted project-local config files and executed embedded commands
- **CVE:** CVE-2025-61260
- **Sources:** [SecurityWeek](https://www.securityweek.com/vulnerability-in-openai-coding-agent-could-facilitate-attacks-on-developers/), [Check Point Research](https://research.checkpoint.com/2025/openai-codex-cli-command-injection-vulnerability/)

### 2025-08 - GitHub Copilot RCE via Prompt Injection

- **Target:** GitHub Copilot (VS Code)
- **Impact:** Prompt injection in code comments enables "YOLO mode" - disabling all confirmations and executing privileged shell commands
- **Root Cause:** Copilot processes untrusted content from code comments as instructions; no safeguard against config modification
- **CVE:** CVE-2025-53773
- **Sources:** [Embrace The Red](https://embracethered.com/blog/posts/2025/github-copilot-remote-code-execution-via-prompt-injection/), [GBHackers](https://gbhackers.com/github-copilot-rce-vulnerability/)

### 2025-08 - Cursor CurXecute RCE via Slack MCP

- **Target:** Cursor AI IDE
- **Impact:** Full developer machine compromise from a single crafted Slack message; attack completes in minutes
- **Root Cause:** AI processed crafted Slack messages as instructions; config changes executed before user approval
- **CVE:** CVE-2025-54135 (CVSS 8.6)
- **Sources:** [Tenable](https://www.tenable.com/blog/faq-cve-2025-54135-cve-2025-54136-vulnerabilities-in-cursor-curxecute-mcpoison), [NVD](https://nvd.nist.gov/vuln/detail/CVE-2025-54135)

### 2025-08 - Cursor MCPoison Silent Backdoor

- **Target:** Cursor AI IDE
- **Impact:** Silent backdoor execution on every team member who opens a project
- **Root Cause:** MCP server trust bound to name rather than content hash; no re-approval for config changes
- **CVE:** CVE-2025-54136 (CVSS 7.2)
- **Sources:** [Check Point Research](https://research.checkpoint.com/2025/cursor-vulnerability-mcpoison/), [NVD](https://nvd.nist.gov/vuln/detail/CVE-2025-54136)

### 2025-08 - Varonis "Reprompt" - Microsoft Copilot Single-Click Data Theft

- **Target:** Microsoft Copilot
- **Impact:** File access history, location, conversation memory exfiltrated; attacker maintains control after chat closed
- **Root Cause:** URL query parameter ?q= accepted as pre-filled prompt without validation
- **Sources:** [Varonis](https://www.varonis.com/blog/reprompt), [SecurityWeek](https://www.securityweek.com/new-reprompt-attack-silently-siphons-microsoft-copilot-data/)

### 2025-07-17 - Amazon Q VS Code Extension Compromise

- **Target:** Amazon Q Developer Extension (950K+ installs)
- **Impact:** Compromised v1.84.0 live for two days; destructive AI prompt instructed deletion of home directory and AWS resources; failed due to syntax error
- **Root Cause:** Over-scoped GitHub token in CI/CD pipeline
- **Sources:** [AWS-2025-015](https://aws.amazon.com/security/security-bulletins/AWS-2025-015/), [The Register](https://www.theregister.com/2025/07/24/amazon_q_ai_prompt/), [CSO Online](https://www.csoonline.com/article/4027963/hacker-inserts-destructive-code-in-amazon-q-as-update-goes-live.html)

### 2025-07-09 - Hugging Face Poisoned GGUF Templates

- **Target:** Hugging Face (1.5M+ GGUF files)
- **Impact:** Backdoor instructions embedded in model files execute inside trusted inference, evading system prompts and runtime monitoring
- **Root Cause:** No content validation of GGUF template sections
- **Sources:** [GlobeNewsWire](https://www.globenewswire.com/news-release/2025/07/09/3112541/0/en/Pillar-Security-Uncovers-Novel-Attack-Vector-That-Embeds-Malicious-Backdoors-in-Model-Files-on-Hugging-Face.html)

### 2025-07 - mcp-remote Critical RCE

- **Target:** mcp-remote (437K+ downloads)
- **Impact:** Full system compromise via malicious MCP server OAuth flow
- **Root Cause:** Improper handling of authorization_endpoint URL in OAuth flow
- **CVE:** CVE-2025-6514 (CVSS 9.6)
- **Sources:** [JFrog](https://jfrog.com/blog/2025-6514-critical-mcp-remote-rce-vulnerability/), [The Hacker News](https://thehackernews.com/2025/07/critical-mcp-remote-vulnerability.html)

### 2025-06 - Langflow Flodrix Botnet Exploitation

- **Target:** Langflow servers
- **Impact:** Full system compromise; Flodrix botnet deployed for DDoS and data exfiltration
- **Root Cause:** Unpatched Langflow instances (CVE-2025-3248) exposed to internet
- **CVE:** CVE-2025-3248
- **Sources:** [Trend Micro](https://www.trendmicro.com/en_us/research/25/f/langflow-vulnerability-flodric-botnet.html), [SecurityWeek](https://www.securityweek.com/recent-langflow-vulnerability-exploited-by-flodrix-botnet/), [Dark Reading](https://www.darkreading.com/vulnerabilities-threats/hackers-exploit-langflow-flaw-flodrix-botnet)

### 2025-06 - EchoLeak - Microsoft 365 Copilot Zero-Click Prompt Injection

- **Target:** Microsoft 365 Copilot
- **Impact:** Zero-click data exfiltration from M365 sessions via crafted email; bypassed XPIA classifier
- **Root Cause:** AI command injection via hidden text, speaker notes, and metadata in documents
- **CVE:** CVE-2025-32711 (CVSS 9.3)
- **Sources:** [The Hacker News](https://thehackernews.com/2025/06/zero-click-ai-vulnerability-exposes.html), [HackTheBox](https://www.hackthebox.com/blog/cve-2025-32711-echoleak-copilot-vulnerability)

### 2025-06 - GitHub Copilot CamoLeak

- **Target:** GitHub Copilot Chat
- **Impact:** Silent exfiltration of AWS keys, security tokens, and zero-day details from private repos; CVSS 9.6
- **Root Cause:** Copilot parsed invisible markdown comments; data exfiltrated via GitHub Camo proxy image requests
- **CVE:** CVE-2025-59145
- **Sources:** [Legit Security](https://www.legitsecurity.com/blog/camoleak-critical-github-copilot-vulnerability-leaks-private-source-code), [Dark Reading](https://www.darkreading.com/application-security/github-copilot-camoleak-ai-attack-exfils-data)

### 2025-06 - Anthropic Filesystem MCP Server "EscapeRoute"

- **Target:** Anthropic Filesystem MCP Server
- **Impact:** Sandbox escape, arbitrary file access, root-level compromise possible
- **Root Cause:** Naive startswith path validation; no symlink validation
- **CVE:** CVE-2025-53109 (CVSS 8.4), CVE-2025-53110 (CVSS 7.3)
- **Sources:** [Cymulate](https://cymulate.com/blog/cve-2025-53109-53110-escaperoute-anthropic/), [SecurityWeek](https://www.securityweek.com/anthropic-mcp-server-flaws-lead-to-code-execution-data-exposure/)

### 2025-05 - ElizaOS Memory Injection Vulnerability

- **Target:** ElizaOS (AI agent framework for blockchain)
- **Impact:** Potential loss of millions in crypto; fabricated payment confirmations stored in memory redirect future transactions
- **Root Cause:** No integrity verification on persistent memory entries
- **Sources:** [Decrypt](https://decrypt.co/318200/elizaos-vulnerability-ai-gaslit-losing-millions)

### 2025-05 - Langflow CISA KEV Addition - Confirmed Active Exploitation

- **Target:** Langflow (before v1.3.0)
- **Impact:** Full server takeover; CISA confirmed active exploitation in the wild
- **Root Cause:** Code validation endpoint invokes exec() on user-supplied code without auth or sandboxing
- **CVE:** CVE-2025-3248 (CVSS 9.8)
- **Sources:** [The Hacker News](https://thehackernews.com/2025/05/critical-langflow-flaw-added-to-cisa.html), [Zscaler](https://www.zscaler.com/blogs/security-research/cve-2025-3248-rce-vulnerability-langflow), [NVD](https://nvd.nist.gov/vuln/detail/CVE-2025-3248)

### 2025-04 - MCP Tool Poisoning / WhatsApp Data Exfiltration

- **Target:** WhatsApp MCP server / MCP ecosystem
- **Impact:** Complete WhatsApp message history exfiltration; 5.5% of MCP servers exhibit tool poisoning; 33% allow unrestricted network access
- **Root Cause:** Hidden instructions in MCP tool descriptions; no runtime integrity verification; tools can mutate definitions post-install
- **Sources:** [Invariant Labs](https://invariantlabs.ai/blog/mcp-security-notification-tool-poisoning-attacks), [Docker](https://www.docker.com/blog/mcp-horror-stories-whatsapp-data-exfiltration-issue/), [Simon Willison](https://simonwillison.net/2025/Apr/9/mcp-prompt-injection/)

### 2025-03-18 - Rules File Backdoor Attack on Cursor and Copilot

- **Target:** Cursor IDE, GitHub Copilot
- **Impact:** Malicious code injected silently into AI-generated output; invisible Unicode characters bypass code reviews
- **Root Cause:** AI config files parsed by AI but invisible to human reviewers due to Unicode obfuscation
- **Sources:** [Pillar Security](https://www.pillar.security/blog/new-vulnerability-in-github-copilot-and-cursor-how-hackers-can-weaponize-code-agents), [The Hacker News](https://thehackernews.com/2025/03/new-rules-file-backdoor-attack-lets.html)

### 2025-03-15 - tj-actions/changed-files GitHub Actions Supply Chain Attack

- **Target:** 23,000+ GitHub repositories
- **Impact:** Access keys, GitHub PATs, npm tokens, and private RSA keys exposed in public workflow logs
- **Root Cause:** Compromised GitHub PAT; chained via reviewdog/action-setup
- **CVE:** CVE-2025-30066, CVE-2025-30154
- **Sources:** [CISA](https://www.cisa.gov/news-events/alerts/2025/03/18/supply-chain-compromise-third-party-tj-actionschanged-files-cve-2025-30066-and-reviewdogaction), [Wiz](https://www.wiz.io/blog/github-action-tj-actions-changed-files-supply-chain-attack-cve-2025-30066), [Unit 42](https://unit42.paloaltonetworks.com/github-actions-supply-chain-attack/)

### 2025-02-21 - Bybit $1.5B Cryptocurrency Heist

- **Target:** Bybit exchange / Safe{Wallet}
- **Impact:** $1.5B in Ethereum stolen; largest crypto theft in history; part of $2.02B DPRK total in 2025
- **Root Cause:** Social engineering of Safe{Wallet} developer; dormant malware activated during legitimate transaction
- **Sources:** [FBI IC3](https://www.ic3.gov/psa/2025/psa250226), [Fortune](https://fortune.com/crypto/2025/03/04/north-korea-bybit-hack-ethereum-safe-dprk-lazarus-group-tradertraitor/), [TRM Labs](https://www.trmlabs.com/resources/blog/the-bybit-hack-following-north-koreas-largest-exploit)

### 2025-02 - Google Gemini Prompt Injection via Calendar Invites

- **Target:** Google Gemini / Calendar / Smart Home
- **Impact:** Unauthorized smart home control, private calendar data exfiltration, deceptive events - all zero-click; 73% of scenarios rated High-Critical
- **Root Cause:** Gemini processes hidden instructions in calendar event metadata
- **Sources:** [The Register](https://www.theregister.com/2025/08/08/infosec_hounds_spot_prompt_injection/), [Dark Reading](https://www.darkreading.com/cloud-security/google-gemini-flaw-calendar-invites-attack-vector), [Miggo](https://www.miggo.io/post/weaponizing-calendar-invites-a-semantic-attack-on-google-gemini)

### 2025-01-24 - OmniGPT Data Breach

- **Target:** OmniGPT AI aggregator
- **Impact:** 30K user emails/phones; 34M lines of chat messages leaked including API keys, crypto private keys
- **Root Cause:** Infrastructure breach; sold for $100 on BreachForums
- **Sources:** [Hackread](https://hackread.com/omnigpt-ai-chatbot-breach-hacker-leak-user-data-messages/), [CSO Online](https://www.csoonline.com/article/3822911/hacker-allegedly-puts-massive-omnigpt-breach-data-for-sale-on-the-dark-web.html)

### 2025 - Cursor Case Sensitivity Bypass

- **Target:** Cursor AI IDE (v1.6.23 and below)
- **Impact:** Configuration file modification leading to potential RCE on case-insensitive file systems
- **Root Cause:** Path comparison used exact case matching on case-insensitive filesystems
- **CVE:** CVE-2025-59944 (CVSS 8.0)
- **Sources:** [Lakera](https://www.lakera.ai/blog/cursor-vulnerability-cve-2025-59944), [NVD](https://nvd.nist.gov/vuln/detail/CVE-2025-59944)

### 2025 - GitHub Copilot RoguePilot Repository Takeover

- **Target:** GitHub Codespaces + Copilot
- **Impact:** Silent GITHUB_TOKEN exfiltration enabling full repository takeover
- **Root Cause:** Copilot processes invisible HTML comments in issues; Codespace secrets accessible via symlink
- **Sources:** [Orca Security](https://orca.security/resources/blog/roguepilot-github-copilot-vulnerability/), [SecurityWeek](https://www.securityweek.com/github-issues-abused-in-copilot-attack-leading-to-repository-takeover/)

### 2025 - DB-GPT Plugin Upload RCE

- **Target:** DB-GPT v0.7.0
- **Impact:** Arbitrary code execution with DB-GPT process privileges (default: root in containers)
- **Root Cause:** No content validation on uploaded plugin Python files
- **CVE:** CVE-2025-51459 (CVSS 6.5)
- **Sources:** [Gecko Security](https://www.gecko.security/blog/cve-2025-51459)

---

## 2024 Incidents

### 2024-12-04 - Ultralytics PyPI Supply Chain Attack

- **Target:** Ultralytics YOLO AI library
- **Impact:** Four malicious versions uploaded containing XMRig crypto miner; two-phase attack over Dec 4-7
- **Root Cause:** Attacker abused git branch names to steal GitHub Actions CI/CD credentials; compromised PyPI token
- **Sources:** [PyPI Blog](https://blog.pypi.org/posts/2024-12-11-ultralytics-attack-analysis/), [Wiz](https://www.wiz.io/blog/ultralytics-ai-library-hacked-via-github-for-cryptomining), [Snyk](https://snyk.io/blog/ultralytics-ai-pwn-request-supply-chain-attack/)

### 2024-12 - ChatGPT Search Manipulation via Hidden Text

- **Target:** OpenAI ChatGPT Search
- **Impact:** Hidden webpage text manipulates AI-generated summaries; demonstrated in crypto scam distributing credential-stealing instructions
- **Root Cause:** Indirect prompt injection via hidden text; ChatGPT Search processed all content including invisible elements
- **Sources:** [dig.watch](https://dig.watch/updates/chatgpt-search-found-vulnerable-to-manipulation)

### 2024-11-22 - Freysa AI Agent Game - Function Manipulation

- **Target:** Freysa autonomous AI agent
- **Impact:** AI agent tricked into releasing $47,316 in crypto by redefining the approveTransfer function's purpose
- **Root Cause:** AI agent manipulated into misinterpreting its own function definitions
- **Sources:** [The Block](https://www.theblock.co/post/328747/human-player-outwits-freysa-ai-agent-in-47000-crypto-challenge), [CryptoBriefing](https://cryptobriefing.com/crypto-trader-ai-gaming-exploit/)

### 2024-11 - Microsoft Copilot Exposes Private GitHub Repos

- **Target:** Microsoft Copilot / Bing Cache
- **Impact:** 16,000+ organizations affected; Fortune 500 private repos exposed; 300+ leaked tokens, keys, secrets
- **Root Cause:** Bing cached repo content when briefly public; Copilot continued serving "zombie data" after repos went private
- **Sources:** [Lasso Security](https://www.lasso.security/blog/lasso-major-vulnerability-in-microsoft-copilot), [SecurityWeek](https://www.securityweek.com/github-copilot-chat-flaw-leaked-data-from-private-repositories/)

### 2024-11 - Microsoft Copilot Studio XSS

- **Target:** Microsoft Copilot Studio
- **Impact:** Cross-site scripting allowing execution of malicious scripts within authenticated sessions
- **Root Cause:** Improper neutralization of input during web page generation
- **CVE:** CVE-2024-49038
- **Sources:** [SentinelOne](https://www.sentinelone.com/vulnerability-database/cve-2024-49038/)

### 2024-10-17 - Imprompter Attack on AI Chatbots

- **Target:** Mistral LeChat, ChatGLM, Meta Llama
- **Impact:** 80% success rate exfiltrating PII via obfuscated adversarial prompts and hidden Markdown image URLs
- **Root Cause:** Multi-lingual token substitution generates human-unreadable but LLM-executable malicious prompts
- **Sources:** [ArXiv](https://arxiv.org/abs/2410.14923), [Imprompter.ai](https://imprompter.ai/)

### 2024-10-22 - Claude Computer Use Launch Security Warnings

- **Target:** Anthropic Claude 3.5 Sonnet
- **Impact:** Autonomous computer control exposed to prompt injection from any visual or textual content; demos showed potential for autonomous malware creation
- **Root Cause:** Granting autonomous computer control inherently exposes LLM to prompt injection from encountered content
- **Sources:** [Prompt Security](https://prompt.security/blog/claude-computer-use-a-ticking-time-bomb), [Bank Info Security](https://www.bankinfosecurity.com/claudes-computer-use-may-end-up-cautionary-tale-a-26651)

### 2024-09 - ChatGPT "SpAIware" Persistent Memory Exploitation

- **Target:** OpenAI ChatGPT macOS app
- **Impact:** Persistent spyware in ChatGPT's long-term memory; continuous data exfiltration across all future sessions
- **Root Cause:** Memory feature allowed prompt injection from untrusted data to create persistent exfiltration instructions
- **Sources:** [Embrace The Red](https://embracethered.com/blog/posts/2024/chatgpt-macos-app-persistent-data-exfiltration/), [The Hacker News](https://thehackernews.com/2024/09/chatgpt-macos-flaw-couldve-enabled-long.html)

### 2024-09-25 - NVIDIA Container Toolkit Vulnerability

- **Target:** NVIDIA Container Toolkit (35%+ of cloud GPU environments)
- **Impact:** Container escape, host filesystem access, privilege escalation, manipulation of GPU workloads
- **Root Cause:** Time-of-check Time-of-Use (TOCTOU) flaw
- **CVE:** CVE-2024-0132 (CVSS 9.0)
- **Sources:** [Wiz](https://www.wiz.io/blog/wiz-research-critical-nvidia-ai-vulnerability), [NVIDIA](https://nvidia.custhelp.com/app/answers/detail/a_id/5582)

### 2024-08-20 - Slack AI Prompt Injection and Data Exfiltration

- **Target:** Slack AI
- **Impact:** Data exfiltrated from private channels; API keys stolen via crafted public channel messages
- **Root Cause:** Indirect prompt injection through public channel messages; Markdown link rendering enabled exfiltration
- **Sources:** [PromptArmor](https://promptarmor.substack.com/p/data-exfiltration-from-slack-ai-via), [Dark Reading](https://www.darkreading.com/cyberattacks-data-breaches/slack-ai-patches-bug-that-let-attackers-steal-data-from-private-channels)

### 2024-08-08 - LOLCopilot - Black Hat USA 2024 Copilot Attacks

- **Target:** Microsoft 365 Copilot
- **Impact:** Hidden email code injection, plugin exploitation, data exfiltration through default Copilot access; no user interaction required
- **Root Cause:** Default configurations grant broad access to emails/docs; indirect prompt injection via invisible email tags
- **Sources:** [Dark Reading](https://www.darkreading.com/application-security/how-to-weaponize-microsoft-copilot-for-cyberattackers), [The Register](https://www.theregister.com/2024/08/08/copilot_black_hat_vulns/)

### 2024-08-06 - Microsoft Copilot Studio SSRF

- **Target:** Microsoft Copilot Studio
- **Impact:** Access to Microsoft internal infrastructure, IMDS, and internal Cosmos DB; cross-tenant impact possible
- **Root Cause:** Server-Side Request Forgery via HTTP header manipulation and redirect techniques
- **CVE:** CVE-2024-38206 (CVSS 8.5)
- **Sources:** [Tenable](https://www.tenable.com/blog/ssrfing-the-web-with-the-help-of-copilot-studio), [The Hacker News](https://thehackernews.com/2024/08/microsoft-patches-critical-copilot.html)

### 2024-07 - Grok AI Election Misinformation

- **Target:** X/Twitter Grok AI chatbot
- **Impact:** Falsely stated VP Harris missed ballot deadlines in 9 states; misinformation repeated for over a week; reached millions of users
- **Root Cause:** No guardrails on political/election queries; secretaries of state from 5 states demanded correction
- **Sources:** [Axios](https://www.axios.com/2024/08/05/elon-musk-grok-2024-election-ballot-misinformation), [TechCrunch](https://techcrunch.com/2024/08/05/secretaries-of-state-urge-x-to-stop-its-grok-chatbot-from-spreading-election-misinformation/)

### 2024-07 - ChatGPT macOS Cleartext Storage

- **Target:** OpenAI ChatGPT macOS app
- **Impact:** All conversations stored in plaintext in non-sandboxed location; any app or malware could read chat history
- **Root Cause:** OpenAI opted out of macOS sandboxing; unencrypted storage
- **CVE:** CVE-2024-40594
- **Sources:** [9to5Mac](https://9to5mac.com/2024/07/03/chatgpt-macos-conversations-plain-text/)

### 2024-07 - Microsoft 365 Copilot ASCII Smuggling

- **Target:** Microsoft 365 Copilot
- **Impact:** Invisible Unicode characters in hyperlinks exfiltrate emails, MFA codes, and sensitive data
- **Root Cause:** Copilot rendered invisible Unicode characters carrying hidden data payloads in links
- **Sources:** [Embrace The Red](https://embracethered.com/blog/posts/2024/m365-copilot-prompt-injection-tool-invocation-and-data-exfil-using-ascii-smuggling/), [The Hacker News](https://thehackernews.com/2024/08/microsoft-fixes-ascii-smuggling-flaw.html)

### 2024-06-25 - Rabbit R1 Hardcoded API Keys

- **Target:** Rabbit R1 AI device
- **Impact:** ElevenLabs admin key, Azure, Yelp, Google Maps, SendGrid keys exposed; could crash entire rabbit OS backend
- **Root Cause:** API keys hardcoded in device source code instead of secure storage
- **Sources:** [Cybernews](https://cybernews.com/security/critical-rabbit-r1-security-flaw/)

### 2024-06 - McDonald's Ends AI Drive-Thru After Failures

- **Target:** McDonald's AI drive-thru (IBM)
- **Impact:** AI added 260 McNuggets, bacon on ice cream, unwanted items; three-year IBM partnership terminated
- **Root Cause:** AI failed to interpret accents, dialects, background noise, and overlapping voices
- **Sources:** [CNBC](https://www.cnbc.com/2024/06/17/mcdonalds-to-end-ibm-ai-drive-thru-test.html)

### 2024-06 - Hugging Face Spaces Breach

- **Target:** Hugging Face Spaces platform
- **Impact:** Unauthorized access to authentication secrets, API tokens, and keys used by developers
- **Root Cause:** Unauthorized access to platform secrets storage
- **Sources:** [The Hacker News](https://thehackernews.com/2024/06/ai-company-hugging-face-notifies-users.html), [SecurityWeek](https://www.securityweek.com/secrets-exposed-in-hugging-face-hack/)

### 2024-05 - GitHub Copilot Training Data Secret Leakage

- **Target:** GitHub Copilot
- **Impact:** Copilot reproduces real, previously exposed secrets from training data; repos using Copilot show 40% higher secret leakage rates
- **Root Cause:** Training data memorization of secrets from public GitHub repositories
- **Sources:** [GitGuardian](https://blog.gitguardian.com/yes-github-copilot-can-leak-secrets/)

### 2024-04 - Hugging Face Cross-Tenant Attack

- **Target:** Hugging Face shared inference infrastructure
- **Impact:** Cross-tenant access to other customers' AI models via malicious Pickle-serialized model
- **Root Cause:** Insecure deserialization; insufficient tenant isolation in shared inference
- **Sources:** [Wiz](https://www.wiz.io/blog/wiz-and-hugging-face-address-risks-to-ai-infrastructure), [Dark Reading](https://www.darkreading.com/cloud-security/critical-bugs-hugging-face-ai-platform-pickle)

### 2024-04-02 - Many-Shot Jailbreaking Research

- **Target:** Claude, GPT-4, GPT-3.5, Llama 2, Mistral
- **Impact:** Hundreds of harmful Q&A examples in a single long prompt bypass safety guardrails of all major LLMs
- **Root Cause:** Expanded context windows enable in-context learning to override safety training
- **Sources:** [Anthropic](https://www.anthropic.com/research/many-shot-jailbreaking)

### 2024-03 - ChatGPT Plugin/Extension Vulnerabilities

- **Target:** ChatGPT plugins, PluginLab.ai, Kesem AI
- **Impact:** OAuth credential theft, zero-click account takeover, malicious plugin installation; GitHub account access possible
- **Root Cause:** Missing authentication in plugin install flow; missing user account verification in PluginLab
- **Sources:** [Salt Security](https://salt.security/blog/security-flaws-within-chatgpt-extensions-allowed-access-to-accounts-on-third-party-websites-and-sensitive-data), [The Hacker News](https://thehackernews.com/2024/03/third-party-chatgpt-plugins-could-lead.html)

### 2024-03 - OpenAI Compromised Credentials on Dark Web

- **Target:** OpenAI ChatGPT users
- **Impact:** 225,000+ compromised credentials for sale; 130K+ unique hosts infiltrated
- **Root Cause:** LummaC2, Raccoon, and RedLine infostealer malware on user devices
- **Sources:** [The Hacker News](https://thehackernews.com/2024/03/over-225000-compromised-chatgpt.html), [BleepingComputer](https://www.bleepingcomputer.com/news/security/openai-credentials-stolen-by-the-thousands-for-sale-on-the-dark-web/)

### 2024-02 - PoisonedRAG Research

- **Target:** All RAG systems
- **Impact:** Injecting 5 malicious texts into a million-document database achieves 90% attack success; 0.04% corpus poisoning achieves 98.2% success
- **Root Cause:** RAG systems inherently trust retrieved documents; no integrity verification of knowledge base contents
- **Sources:** [ArXiv](https://arxiv.org/abs/2402.07867)

### 2024-02-14 - Air Canada Chatbot Lawsuit Ruling

- **Target:** Air Canada
- **Impact:** Tribunal ruled Air Canada liable for chatbot's fabricated bereavement fare policy; ordered to pay $812.02; landmark AI liability ruling
- **Root Cause:** AI chatbot hallucinated nonexistent policy; Air Canada's "separate legal entity" defense rejected
- **Sources:** [CBC](https://www.cbc.ca/news/canada/british-columbia/air-canada-chatbot-lawsuit-1.7116416), [ABA](https://www.americanbar.org/groups/business_law/resources/business-law-today/2024-february/bc-tribunal-confirms-companies-remain-liable-information-provided-ai-chatbot/)

### 2024-01-18 - DPD AI Chatbot Malfunction

- **Target:** DPD (UK parcel delivery)
- **Impact:** Chatbot swore at customers, wrote poetry criticizing DPD, called itself "the worst delivery firm"; 1.3M views on X
- **Root Cause:** System update removed guardrails from AI chat element
- **Sources:** [Time](https://time.com/6564726/ai-chatbot-dpd-curses-criticizes-company/)

### 2024 - LangChain Arbitrary Code Execution

- **Target:** langchain-experimental (v0.0.15-0.0.21)
- **Impact:** Arbitrary Python code execution via VectorSQLDatabaseChain
- **Root Cause:** eval() used on all database-retrieved values without sanitization
- **CVE:** CVE-2024-21513 (CVSS 8.5)
- **Sources:** [NVD](https://nvd.nist.gov/vuln/detail/cve-2024-21513), [Snyk](https://security.snyk.io/vuln/SNYK-PYTHON-LANGCHAINEXPERIMENTAL-7278171)

### 2024 - LangChain GraphCypherQAChain Injection

- **Target:** langchain-ai/langchain v0.2.5
- **Impact:** SQL/Cypher injection enabling unauthorized data manipulation, exfiltration, and DoS
- **Root Cause:** Insufficient input sanitization in graph database query construction
- **CVE:** CVE-2024-7042
- **Sources:** [SentinelOne](https://www.sentinelone.com/vulnerability-database/cve-2024-7042/)

### 2024 - LangChain Code Execution via LLMSymbolicMathChain

- **Target:** langchain-experimental
- **Impact:** Arbitrary code execution through symbolic math processing
- **Root Cause:** Unsafe code evaluation in symbolic math processing
- **CVE:** CVE-2024-46946
- **Sources:** [NVD](https://nvd.nist.gov/vuln/detail/cve-2024-46946)

---

## Key Statistics

| Metric | Value | Source |
|--------|-------|--------|
| AI safety incidents in 2024 | 233 (56.4% increase from 2023) | [Stanford AI Index 2025](https://aiindex.stanford.edu/report/) |
| AI incidents in 2025 | 346 (179 involved deepfakes) | [AI Incident Database](https://incidentdatabase.ai/) |
| DPRK crypto theft in 2025 | $2.02 billion | [The Hacker News](https://thehackernews.com/2025/12/north-korea-linked-hackers-steal-202.html) |
| Largest single crypto theft (Bybit) | $1.5 billion | [FBI IC3](https://www.ic3.gov/psa/2025/psa250226) |
| Largest DeFi exploit of 2026 (Drift) | $285 million | [Elliptic](https://www.elliptic.co/blog/drift-protocol-exploited-for-286-million-in-suspected-dprk-linked-attack) |
| AI trading agent losses Q1 2026 | $45 million+ | [KuCoin](https://www.kucoin.com/blog/en-ai-trading-agent-vulnerability-2026-how-a-45m-crypto-security-breach-exposed-protocol-risks) |
| Step Finance treasury drained (Jan 2026) | $40 million (token -97%) | [KuCoin](https://www.kucoin.com/blog/en-ai-trading-agent-vulnerability-2026-how-a-45m-crypto-security-breach-exposed-protocol-risks) |
| DeFi teams reusing shared API keys across agents | 45.6% | [KuCoin](https://www.kucoin.com/blog/en-ai-trading-agent-vulnerability-2026-how-a-45m-crypto-security-breach-exposed-protocol-risks) |
| GTG-1002 organizations targeted (Sep-Nov 2025) | ~30 | [Anthropic](https://assets.anthropic.com/m/ec212e6566a0d47/original/Disrupting-the-first-reported-AI-orchestrated-cyber-espionage-campaign.pdf) |
| GTG-1002 share of operations executed by AI | 80-90% | [Anthropic](https://assets.anthropic.com/m/ec212e6566a0d47/original/Disrupting-the-first-reported-AI-orchestrated-cyber-espionage-campaign.pdf) |
| FortiGate devices compromised by CyberStrikeAI (Jan-Feb 2026) | 600+ across 55 countries | [AWS Security Blog](https://aws.amazon.com/blogs/security/ai-augmented-threat-actor-accesses-fortigate-devices-at-scale/) |
| CyberStrikeAI C2 IPs observed | 21 (Jan 20-Feb 26, 2026) | [The Hacker News](https://thehackernews.com/2026/03/open-source-cyberstrikeai-deployed-in.html) |
| MCP server CVEs in Jan-Feb 2026 | 30+ in 60 days | [MCP Security Report](https://www.heyuan110.com/posts/ai/2026-03-10-mcp-security-2026/) |
| Flowise instances exposed (Apr 2026) | 12,000-15,000+ | [The Hacker News](https://thehackernews.com/2026/04/flowise-ai-agent-builder-under-active.html) |
| MCP STDIO design RCE exposure (OX Security, Apr 2026) | 200,000+ servers, 150M+ downloads across Python/TypeScript/Java/Rust SDKs | [OX Security](https://www.ox.security/blog/the-mother-of-all-ai-supply-chains-critical-systemic-vulnerability-at-the-core-of-the-mcp/) |
| Marimo CVE-2026-39987 exploit events (Apr 11-14, 2026) | 662 from 11 IPs across 10 countries | [Sysdig](https://www.sysdig.com/blog/cve-2026-39987-update-how-attackers-weaponized-marimo-to-deploy-a-blockchain-botnet-via-huggingface) |
| AI API routers tested vs. malicious (UCSB/UCSD, Apr 2026) | 26 of 428 routed malicious payloads; 17 stole AWS creds | [ArXiv](https://arxiv.org/html/2604.08407v1) |
| Single crypto wallet drained by malicious LLM router | $500,000 | [CoinDesk](https://www.coindesk.com/tech/2026/04/13/ai-agents-are-set-to-power-crypto-payments-but-a-hidden-flaw-could-expose-wallets) |
| n8n-hosted webhook phishing email volume (Mar 2026 vs Jan 2025) | 686% increase | [Cisco Talos](https://blog.talosintelligence.com/the-n8n-n8mare/) |
| Vercel stolen data listing price on BreachForums (Apr 2026) | $2 million | [OX Security](https://www.ox.security/blog/vercel-context-ai-supply-chain-attack-breachforums/) |
| Tool misuse incidents tracked in 2026 (OWASP Q1 report) | 520 (most reported category) | [OWASP GenAI Q1 2026](https://genai.owasp.org/2026/04/14/owasp-genai-exploit-round-up-report-q1-2026/) |
| Prompt injection incidents tracked in 2026 (OWASP Q1 report) | 450 | [OWASP GenAI Q1 2026](https://genai.owasp.org/2026/04/14/owasp-genai-exploit-round-up-report-q1-2026/) |
| UNC1069 malicious packages since Jan 2025 | 1,700+ across npm, PyPI, Go, Rust, Packagist | [The Hacker News](https://thehackernews.com/2026/04/n-korean-hackers-spread-1700-malicious.html) |
| UNC1069 impersonation domains blocked (Feb 6-Apr 7, 2026) | 164 | [The Hacker News](https://thehackernews.com/2026/04/n-korean-hackers-spread-1700-malicious.html) |
| MCP servers exposed, zero auth | 492 | [Trend Micro](https://www.trendmicro.com/en_us/research/26/c/teampcp-telnyx-attack-marks-a-shift-in-tactics.html) |
| MCP servers with cmd injection flaws | 43% | [Invariant Labs](https://invariantlabs.ai/blog/mcp-security-notification-tool-poisoning-attacks) |
| MCP servers with tool poisoning | 5.5% | [Invariant Labs](https://invariantlabs.ai/blog/mcp-security-notification-tool-poisoning-attacks) |
| Malicious packages in registries (2025) | 512,847 (156% YoY increase) | [Sonatype](https://www.sonatype.com/state-of-the-software-supply-chain/2025) |
| Forbes AI 50 with leaked secrets | 65% | [Wiz Research](https://www.wiz.io/blog/wiz-research-65-percent-of-ai-50-companies-leaked-secrets) |
| Orgs compromised via Salesloft Drift | 700+ | [Google Cloud Blog](https://cloud.google.com/blog/topics/threat-intelligence/data-theft-salesforce-instances-via-salesloft-drift) |
| OpenClaw instances exposed (Feb 2026) | 135,000+ across 82 countries | [Kaspersky](https://www.kaspersky.com/blog/openclaw-vulnerabilities-exposed/55263/) |
| Malicious skills on ClawHub | 1,184+ (~12% of registry) | [eSecurity Planet](https://www.esecurityplanet.com/threats/hundreds-of-malicious-skills-found-in-openclaws-clawhub/) |
| SaaS envs via Trivy cascade | 1,000+ | [Wiz](https://www.wiz.io/blog/trivy-compromised-teampcp-supply-chain-attack) |
| ChatGPT credentials on dark web | 225,000+ | [The Hacker News](https://thehackernews.com/2024/03/over-225000-compromised-chatgpt.html) |
| AI coding tool vulns (5 tools tested) | 69 vulns, 6 critical | [Fortune](https://fortune.com/2025/12/15/ai-coding-tools-security-exploit-software/) |
| Deepfake fraud losses Q1 2025 | $200 million | [AI Incident Database](https://incidentdatabase.ai/) |
| Organizations with AI agent-related cyber incident in past 12 months (CSA, Apr 2026) | 65% (88% confirmed or suspected) | [CSA Press Release](https://cloudsecurityalliance.org/press-releases/2026/04/21/new-cloud-security-alliance-survey-reveals-82-of-enterprises-have-unknown-ai-agents-in-their-environments) |
| Enterprises that discovered previously unknown AI agents in their environment | 82% | [CSA Press Release](https://cloudsecurityalliance.org/press-releases/2026/04/21/new-cloud-security-alliance-survey-reveals-82-of-enterprises-have-unknown-ai-agents-in-their-environments) |
| Healthcare-sector AI agent incident rate (CSA, Apr 2026) | 92.7% | [Infosecurity Magazine](https://www.infosecurity-magazine.com/news/unchecked-ai-agents-cause/) |
| Organizations with formal AI agent decommissioning processes | 21% | [CSA Press Release](https://cloudsecurityalliance.org/press-releases/2026/04/21/new-cloud-security-alliance-survey-reveals-82-of-enterprises-have-unknown-ai-agents-in-their-environments) |
| Increase in malicious indirect prompt injection web pages (Google, Nov 2025-Feb 2026) | 32% relative increase | [Google Online Security Blog](https://security.googleblog.com/2026/04/ai-threats-in-wild-current-state-of.html) |
| OpenClaw instances reachable on the public internet (SecurityScorecard, Apr 2026) | 40,214 (28,663 unique IPs hosting accessible control panels) | [SecurityScorecard](https://securityscorecard.com/blog/how-exposed-openclaw-deployments-turn-agentic-ai-into-an-attack-surface/) |
| Exposed OpenClaw instances vulnerable to RCE | ~63% | [Infosecurity Magazine](https://www.infosecurity-magazine.com/news/researchers-40000-exposed-openclaw/) |
| Crypto stolen by HexagonalRodent in Q1 2026 via AI-augmented Web3 dev attacks | $12 million; 26,584 wallets exfiltrated from 2,726 systems | [Help Net Security](https://www.helpnetsecurity.com/2026/04/23/hexagonalrodent-north-korean-hackers-targeting-developers/) |
| LMDeploy CVE-2026-33626 time from advisory to first in-the-wild exploit | 12 hours 31 minutes | [Sysdig](https://www.sysdig.com/blog/cve-2026-33626-how-attackers-exploited-lmdeploy-llm-inference-engines-in-12-hours) |
| Bitwarden CLI 2026.4.0 trojanized npm package window | ~90 minutes (April 22, 2026); 334 downloads | [The Hacker News](https://thehackernews.com/2026/04/bitwarden-cli-compromised-in-ongoing.html) |
| Xinference PyPI total downloads at time of compromise | 600,000+ | [Mend.io](https://www.mend.io/blog/malicious-xinference-pypi-teampcp-part-4/) |
| North Korea share of all 2026 crypto-hack value (through April) | 76% ($577M from two attacks) | [TRM Labs](https://www.trmlabs.com/resources/blog/north-korea-stole-76-of-all-crypto-hack-value-in-2026-with-just-two-attacks) |
| KelpDAO exploit (Apr 18, 2026) | $292 million | [TRM Labs](https://www.trmlabs.com/resources/blog/north-korea-stole-76-of-all-crypto-hack-value-in-2026-with-just-two-attacks) |
| North Korea cumulative crypto theft since 2017 | $6 billion+ | [The Block](https://www.theblock.co/post/399569/north-korea-accounts-for-76-of-2026-crypto-hack-losses-with-theft-since-2017-topping-6-billion-trm-labs) |
| Vulnerability exploitation as breach entry vector (Verizon 2026 DBIR) | 31% (first time above stolen credentials in 19 years) | [Verizon](https://www.verizon.com/about/news/breach-industry-wide-dbir-finds) |
| Employees using unapproved "shadow AI" tools at work (Verizon 2026 DBIR) | 45% (up from ~15%) | [Verizon](https://www.verizon.com/about/news/breach-industry-wide-dbir-finds) |
| Breaches involving a third party (Verizon 2026 DBIR) | 48% (up ~60%) | [Verizon](https://www.verizon.com/about/news/breach-industry-wide-dbir-finds) |
| FAMOUS CHOLLIMA share of hands-on-keyboard intrusions on tech firms (Apr 2025-Mar 2026) | 47% | [Forbes / CrowdStrike](https://www.forbes.com/sites/tylerroush/2026/06/09/north-korean-hackers-posing-as-fake-it-workers-behind-nearly-half-of-all-tech-firm-attacks-report-says/) |
| Compromised LiteLLM package downloads in a 3-hour window (Mar 2026, OWASP) | ~47,000 | [Help Net Security](https://www.helpnetsecurity.com/2026/06/11/owasp-prompt-injection-ai-security-failures/) |
| Organizations with policies to detect shadow-AI deployments (OWASP, Jun 2026) | 37% | [Help Net Security](https://www.helpnetsecurity.com/2026/06/11/owasp-prompt-injection-ai-security-failures/) |
| Most-flagged agentic AI repositories by advisory count (OWASP, Jun 2026) | n8n (57), Claude Code (22), AutoGPT (15) | [Help Net Security](https://www.helpnetsecurity.com/2026/06/11/owasp-prompt-injection-ai-security-failures/) |
| Mini Shai-Hulud worm scope (May 2026) | 170+ packages, 518M cumulative downloads | [The Hacker News](https://thehackernews.com/2026/05/mini-shai-hulud-worm-compromises.html) |
| Megalodon GitHub Actions campaign (May 2026) | 5,561 repositories, 5,718 malicious commits | [SecurityWeek](https://www.securityweek.com/over-5500-github-repositories-infected-in-megalodon-supply-chain-attack/) |
| Ollama servers exposed to "Bleeding Llama" memory leak (May 2026) | 300,000+ | [Cyera](https://www.cyera.com/research/bleeding-llama-critical-unauthenticated-memory-leak-in-ollama) |
| Fake OpenAI model downloads on Hugging Face in ~18 hours (May 2026) | ~244,000 (reached #1 trending) | [HiddenLayer](https://www.hiddenlayer.com/research/malware-found-in-trending-hugging-face-repository-open-oss-privacy-filter) |
| Mastra npm scope packages backdoored in 88 minutes (Jun 2026) | 144+ (@mastra/core ~918K weekly downloads) | [The Hacker News](https://thehackernews.com/2026/06/144-mastra-npm-packages-compromised-via.html) |
| AI-driven CVE forecast for 2026 | ~66,000 CVEs projected | [Help Net Security](https://www.helpnetsecurity.com/2026/06/15/first-2026-cve-forecast/) |
| Open-source AI coding agents bypassed by GuardFall shell injection (Jun 2026) | 10 of 11 tested (only Continue mitigated) | [Adversa AI](https://adversa.ai/blog/opensource-ai-coding-agents-shell-injection-vulnerability/) |
| AI-hallucinated "phantom" domains registrable by attackers (Unit 42, Jun 2026) | ~250,000 (from 2.1M URLs across 913 brands) | [Palo Alto Unit 42](https://unit42.paloaltonetworks.com/phantom-squatting-hallucinated-web-domains/) |
| Models that executed a fraudulent agent payment in Zscaler indirect-prompt-injection test (Jul 2026) | 4 of 26 (2 more misclassified the fake site as legitimate) | [Zscaler ThreatLabz](https://www.zscaler.com/blogs/security-research/indirect-prompt-injection-web-content-targets-ai-agents) |
| Nacos configuration items encrypted by JADEPUFFER agentic ransomware (Jul 2026) | 1,342 (encryption key never stored, recovery impossible) | [Sysdig](https://sysdig.com/blog/jadepuffer-agentic-ransomware-for-automated-database-extortion) |
| Enterprises reporting an AI-related security incident or vulnerability (DigiCert, Jul 2026) | 78% (of 1,001 IT/security leaders across US, UK, Australia) | [DigiCert](https://www.globenewswire.com/news-release/2026/07/07/3323253/0/en/latest-digicert-research-shows-ai-security-risks-already-hitting-enterprises-with-78-reporting-incidents.html) |
| Organizations that cannot fully trace AI decisions to models and source data (DigiCert, Jul 2026) | 47% (nearly half also lack centralized AI visibility) | [DigiCert](https://www.globenewswire.com/news-release/2026/07/07/3323253/0/en/latest-digicert-research-shows-ai-security-risks-already-hitting-enterprises-with-78-reporting-incidents.html) |
| AI-related vulnerability alerts with an available fix left unpatched (Orca, Jul 2026) | 99.9% | [Help Net Security](https://www.helpnetsecurity.com/2026/07/13/ai-infrastructure-security-risks-report/) |
| Companies running AI packages with at least one known vulnerability (Orca, Jul 2026) | 81.2% (74.1% have at least one critical CVE) | [Help Net Security](https://www.helpnetsecurity.com/2026/07/13/ai-infrastructure-security-risks-report/) |
| Prompt-injection techniques in CrowdStrike taxonomy (Jul 7, 2026) | 200+ (18 newly added) | [CrowdStrike](https://www.crowdstrike.com/en-us/blog/crowdstrike-uncovers-new-prompt-injection-techniques/) |
| GitHub Copilot harmful-output rate, workflow-staged vs direct prompt (Alan Turing Institute, Jul 2026) | 816/816 (100%) vs 8/816 (0.98%) | [Help Net Security](https://www.helpnetsecurity.com/2026/07/09/github-coding-agent-jailbreak/) |
| Unique AI agent skills analyzed; suspicious; malicious (ESET H1 2026, Jul 2026) | ~900,000; 25,000+; 3,000+ | [Help Net Security](https://www.helpnetsecurity.com/2026/07/08/eset-ai-threat-trends-report/) |
| Compromised credential records for AI/dev platforms in stealer logs (SOCRadar, Jul 2026) | OpenAI 3.1M, Replit 204K, Hugging Face 186K, RunwayML 95K | [SOCRadar](https://socradar.io/blog/ai-agent-credential-stealer-log-source-code/) |
| MCP servers analyzed; exploitable; running with no authentication (Trend Micro, Jul 2026) | 9,695; 2,259; 2,054 | [Trend AI Security](https://www.trendaisecurity.com/en-us/resources-insights/research/stars-dont-save-you-popularity-is-not-security-in-the-mcp-ecosystem) |
| SkillCloak self-extracting packing evasion of skill scanners (HKUST, Jul 2026) | >90% across all 8 scanners tested (best scanner dropped 99% to ~10%) | [Help Net Security](https://www.helpnetsecurity.com/2026/07/09/malicious-ai-agent-skills-scan/) |
| HalluSquatting consistency of hallucinated repo names; skill-install success (Jul 2026) | 85%; 100% | [The Hacker News](https://thehackernews.com/2026/07/new-hallusquatting-attack-could-trick.html) |
| Merged pull requests reaching default branch with no substantive human or bot review (Ghostcommit study, Jul 2026) | 73% (across 300 top repositories) | [BleepingComputer](https://www.bleepingcomputer.com/news/security/ghostcommit-hides-prompt-injection-in-images-to-fool-ai-agents-steal-secrets/) |
| Langflow CVE-2026-55255 significance (Jul 7, 2026) | First AI agent orchestration platform added to CISA KEV | [BleepingComputer](https://www.bleepingcomputer.com/news/security/cisa-orders-feds-to-prioritize-patching-langflow-auth-bypass-flaw/) |
| 2025 scams that involved AI or deepfakes; total US scam losses (Gallup, Jul 2026) | 12%; $68 billion | [Gallup](https://news.gallup.com/poll/710984/scam-victims-report-billions-lost-harm-mental-health.aspx) |

---

## Attack Pattern Taxonomy

### Supply Chain Credential Cascade

A single compromised credential triggers lateral movement across multiple package registries and downstream organizations.

**Key incidents:**
- Shai-Hulud worm lineage (Apr-Jun 2026): Mini Shai-Hulud (TanStack, Mistral AI, Guardrails AI, CVE-2026-45321) escalated into Miasma (Red Hat npm, node-gyp "Phantom Gyp") and Hades (PyPI `.pth` startup hooks), repeatedly abusing npm OIDC trusted publishing, install-time hooks, the Bun runtime, and forged SLSA provenance to steal AI-provider and cloud credentials.
- OpenAI internal source-code theft (May 14, 2026): The TanStack compromise infected developer machines and led to theft of limited credentials and code-signing certificates from a subset of OpenAI internal repositories.
- Mastra AI npm scope takeover (Jun 17, 2026): A dormant former-contributor account let North Korean actor Sapphire Sleet (UNC1069) backdoor 144+ packages in 88 minutes with an install-time RAT dropper.
- PyTorch Lightning PyPI compromise (Apr 30, 2026): Mini Shai-Hulud payload in `lightning` 2.6.2/2.6.3 ran a Bun-based stealer and self-propagated through npm and PyPI.
- Megalodon (May 25, 2026): Injected GitHub Actions workflows across 5,561 repositories harvested CI/CD secrets, surfaced through trojanized Tiledesk npm versions.
- Bitwarden CLI cascade (Apr 22, 2026): Checkmarx KICS Docker Hub compromise propagated through Bitwarden's Dependabot pipeline, signing and publishing a trojanized @bitwarden/cli@2026.4.0 that harvested AI tooling configs alongside cloud and registry secrets.
- CanisterSprawl npm worm via Namastex Labs and pgserve (Apr 21, 2026): Self-propagating worm jumps from npm into PyPI when a developer holds tokens for both registries; data exfiltrates to an ICP canister that is structurally takedown-resistant.
- Vercel / Context.ai OAuth chain (Apr 2026): Lumma Stealer infection of a Context.ai employee led to OAuth token theft, then escalated into the Vercel Google Workspace tenant because a Vercel employee had granted the Context.ai browser extension "Allow All" enterprise scopes.
- TeamPCP cascade (Mar 2026): Trivy -> Checkmarx -> LiteLLM -> Telnyx -> CanisterWorm -> Cisco -> Mercor. One service account compromise led to 1,000+ SaaS environments breached.
- Xinference PyPI compromise (Apr 22, 2026): Compromised XprobeBot account injected a base64 credential stealer into `__init__.py` for versions 2.6.0-2.6.2 of an inference framework with 600,000+ downloads.
- UNC1069 Contagious Interview (Apr 2026): 1,700+ malicious packages across npm, PyPI, Go, Rust, and Packagist since Jan 2025; ClickFix lures via fake Zoom/Teams links after social engineering.
- Axios npm compromise (Mar 2026): Social engineering of one maintainer threatened 70-100M weekly downloads.
- Bybit heist (Feb 2025): Compromise of one Safe{Wallet} developer led to $1.5B theft.
- Ultralytics PyPI attack (Dec 2024): Git branch name abuse stole CI/CD credentials for two-phase supply chain attack.

### Confused Deputy

An AI agent with legitimate access is tricked into performing actions on behalf of an attacker.

**Key incidents:**
- Zscaler in-the-wild IPI payment fraud (Jul 2026): Malicious websites use SEO poisoning and hidden HTML to steer web-browsing agents into paying a fake developer API license; 4 of 26 tested models executed the fraudulent crypto payment.
- Microsoft 365 Copilot SearchLeak (CVE-2026-42824, Jun 2026): A single malicious link injects instructions through the search `q` parameter and exfiltrates emails, files, and MFA codes via a Bing CSP-allowlist bypass.
- Grok and Bankr wallet drain (May 2026): A Morse-code prompt injection routed through Grok produced a transfer command that the Bankr trading agent executed as authoritative.
- Claude Code GitHub Action (Jun 2026): Prompt injection in issues and pull requests steered the agent into reading `/proc/self/environ` and exfiltrating CI/CD secrets.
- Copilot Studio ShareLeak and Agentforce PipeLeak (Apr 2026): Crafted SharePoint and Web-to-Lead form payloads hijack agents into bulk-exfiltrating CRM and SharePoint data by email, with no volume cap and no user-visible indicator.
- Claude Code / Gemini CLI / Copilot Agent via GitHub comments (Apr 2026): PR titles, issue descriptions, and comments hijack CI agents to exfiltrate API keys and secrets from the runner.
- Salesforce Agentforce ForcedLeak (Sep 2025): Malicious Web-to-Lead form data tricks agent into exfiltrating CRM records.
- ServiceNow Now Assist (Nov 2025): Low-privilege agent tricks higher-privilege agent into exporting case files.
- EchoLeak M365 Copilot (Jun 2025): Crafted email triggers zero-click data exfiltration.
- ChatGPT SpAIware (Sep 2024): Untrusted data plants persistent exfiltration instructions in memory.

### Overprivileged Integration

AI agents or chatbot integrations granted excessive access that becomes the attack surface.

**Key incidents:**
- Grok and Bankr "Bankr Club Membership" (May 2026): Activating a membership NFT silently granted the trading agent high-privilege transfer and swap rights that an attacker then abused.
- Amazon Q Developer MCP auto-load (CVE-2026-12957/12958, Jun 2026): The extension auto-loaded `.amazonq/mcp.json` from any opened repository with full environment inheritance, enabling RCE and AWS credential theft.
- Azure AI Foundry M365 agents (CVE-2026-35435, May 2026): Improper access control let an unauthenticated network attacker elevate privileges over published agent workflows and connectors.
- AWS Bedrock AgentCore "God Mode" (Apr 2026): Starter toolkit auto-creates IAM roles with wildcard memory actions so any agent can read or poison every other agent's state.
- Step Finance treasury drain (Jan 2026): Trading agents held wallet, oracle, and trading-endpoint permissions simultaneously; one device compromise cascaded into $40M loss.
- Salesloft Drift OAuth breach (Aug 2025): Stolen OAuth tokens gave access to 700+ customer Salesforce environments.
- LOLCopilot/M365 Copilot (Aug 2024): Default configurations grant broad access to all emails and documents.
- Amazon Q extension (Jul 2025): Over-scoped GitHub token in CI/CD allowed destructive prompt injection.
- Copilot "zombie data" exposure (Nov 2024): 16,000+ organizations' private repos exposed via cached data.

### Config-as-Code Execution

Malicious configurations in repository files execute code when AI tools process them.

**Key incidents:**
- Gemini CLI headless auto-trust (GHSA-wpqr-6v78-jr5g, CVSS 10.0, Apr 2026): A config file in `.gemini/` executed before sandbox init in CI, with `--yolo` mode bypassing tool allowlisting.
- Amazon Q Developer (CVE-2026-12957/12958, Jun 2026): `.amazonq/mcp.json` in a repository auto-loaded MCP servers and spawned processes with no workspace-trust check.
- Claude Code deeplink (May 2026): A `claude-cli://` link smuggled `--settings={...}` with a `SessionStart` hook through `--prefill`, suppressing the trust prompt when pointed at a trusted repo.
- Claude Code RCE via hooks (CVE-2025-59536): Malicious .claude/settings.json executes commands before trust dialog.
- Codex CLI command injection (CVE-2025-61260): Project-local configs execute commands without user consent.
- Rules File Backdoor (Mar 2025): Invisible Unicode in .cursorrules and copilot-instructions.md injects malicious code.
- Cursor MCPoison (CVE-2025-54136): Benign MCP config approved once, then silently modified to execute backdoor.

### Unsandboxed Code Execution

AI tools run user-supplied or AI-generated code without isolation.

**Key incidents:**
- Cursor DuneSlide (CVE-2026-50548, CVE-2026-50549, CVSS 9.8, Jul 2026): Prompt-injected content overwrites the `cursorsandbox` enforcer binary via working-directory manipulation and a fail-open symlink check, escaping the terminal sandbox for zero-click OS-level RCE.
- Microsoft Semantic Kernel (CVE-2026-26030, CVE-2026-25592, May 2026): Unsafe string interpolation in the Python vector store and an accidentally exposed `[KernelFunction]` file-download method turn prompt injection into host RCE.
- AutoGen Studio "AutoJack" (Jun 2026): A localhost MCP WebSocket trusting a base64 `server_params` value let a visited webpage run host commands; GitHub-main builds only, PyPI releases unaffected.
- Cline AI agent (CVE-2026-44211, CVSS 9.7, May 2026): An unauthenticated local WebSocket on port 3484 with no Origin validation allowed cross-origin RCE from a malicious page.
- PraisonAI legacy API server (CVE-2026-44338, May 2026): Authentication disabled by default plus binding to `0.0.0.0:8080` allowed unauthenticated workflow execution, exploited within hours.
- Langflow path traversal (CVE-2026-5027, Jun 2026): Unsanitized `filename` in `POST /api/v2/files` writes arbitrary files for unauthenticated RCE, with default auto-login; exploited in the wild.
- NVIDIA Triton Inference Server (CVE-2026-24207, CVSS 9.8, May 2026): Authentication bypass in the model-serving stack leads to code execution and privilege escalation.
- Anthropic MCP STDIO design RCE (Apr 2026): Reference SDKs in Python, TypeScript, Java, and Rust execute attacker-supplied command strings passed to STDIO transport; 200K+ servers and 150M+ downloads exposed; Anthropic declined to patch.
- Flowise CSV Agent prompt injection RCE (CVE-2026-41264, Apr 21, 2026): Lack of sandboxing in the CSV_Agents `run` method lets an LLM-emitted Python script run on the host; bypass for the earlier CVE-2026-41137 hardening.
- Flowise MCP Adapters CVE-2026-40933 (CVSS 10.0): Unsafe serialization of stdio commands lets an authenticated user add an MCP server that runs arbitrary commands such as `npx -c "touch /tmp/pwn"`.
- Marimo CVE-2026-39987 (CVSS 9.3): /terminal/ws WebSocket lacks auth; weaponized within 10 hours and used to drop NKAbuse blockchain-C2 malware hosted on a Hugging Face typosquat.
- Flowise CVE-2025-59528 (CVSS 10.0): CustomMCP node executes JavaScript from mcpServerConfig without validation; 12K+ exposed instances under active exploitation from Starlink IP in April 2026.
- aws-mcp-server CVE-2026-5058 and CVE-2026-5059 (CVSS 9.8): Unauthenticated command injection via allowed-commands list passed to a system call.
- PraisonAI CVE-2026-39891 (CVSS 8.8): Template injection via unescaped agent.start() input processed by create_agent_centric_tools().
- Langflow CVE-2025-3248 (CVSS 9.8): exec() on user-supplied code without auth; added to CISA KEV.
- Langflow CVE-2026-33017 (CVSS 9.3): Same exec() pattern exploited within 20 hours of disclosure.
- n8n Ni8mare (CVSS 10.0): Content-Type confusion enables unauthenticated RCE on 100K+ instances.
- DB-GPT plugin upload RCE (CVE-2025-51459): No content validation on uploaded Python plugins.

### Social Engineering of AI Agents

Humans manipulate AI agents or use AI as intermediaries for social engineering.

**Key incidents:**
- Drift Protocol $285M exploit (Apr 2026): Six-month campaign posing as legitimate trading firm to social-engineer multisig signers.
- Freysa AI agent game (Nov 2024): AI tricked into redefining its own function semantics to release $47K in crypto.
- OpenClaw email deletion at Meta (Feb 2026): Agent's context compaction caused it to ignore explicit stop commands.
- DPD chatbot manipulation (Jan 2024): Customer manipulated chatbot into cursing and criticizing its own company.

### Tool Poisoning

Malicious instructions embedded in tool descriptions, model files, or integration metadata.

**Key incidents:**
- Malicious LLM routers (Apr 2026): 26 of 428 tested routers rewrote tool calls, exfiltrated secrets, or redirected transactions; at least one drained a $500K crypto wallet.
- MCP tool poisoning / WhatsApp exfiltration (Apr 2025): Hidden instructions in MCP tool descriptions cause silent data theft.
- Hugging Face GGUF poisoned templates (Jul 2025): Malicious instructions embedded in 1.5M+ model files.
- ClawHub malicious skills (Jan-Mar 2026): 1,184+ malicious skills distributing Atomic Stealer and keyloggers.
- GitHub Copilot filename injection (Nov 2025): Extremely long filenames with prompt injection instructions.

### Cross-Tenant and Cross-Agent Isolation Failures

Managed AI platforms and agent runtimes fail to isolate one customer, tenant, or agent from another, so a request or write in one context reaches data or execution in another.

**Key incidents:**
- Writer AI "WriteOut" (Jul 2026): Agent live previews were served from the main app origin, so the browser forwarded a logged-in user's session cookie into an attacker sandbox for cross-tenant account takeover.
- Google Dialogflow CX "Rogue Agent" (Jul 2026): A writable setup file in a shared, un-isolated Cloud Run runtime let one agent's owner run code across every agent in the Google Cloud project.
- GitLost GitHub Agentic Workflows (Jul 2026): A public issue's hidden instructions drove a credentialed agent to leak private-repository contents into a public comment (the "lethal trifecta" of private data, untrusted input, and a public channel).
- mem0 unauthenticated memory API (CVE-2026-59705/59706, Jul 2026): Missing auth on the agent memory store allowed reading, writing, and deleting any user's memories and leaked LLM API keys in plaintext.
- Dify "DifyTap" (CVE-2026-41947 to 41950, Jun 2026): Console users and chatbots could read other organizations' documents and the tracing subsystem could be redirected to exfiltrate all messages.
- AWS Bedrock AgentCore "Agent God Mode" (Apr 2026): Auto-created IAM roles with wildcard privileges let one agent reach every other agent's memory across the account.

### No Action-Level Authorization

AI agents execute privileged operations without per-action permission checks.

**Key incidents:**
- GhostApproval symlink deception (Jul 2026): In six AI coding assistants a symlink disguised as a benign file makes the approval dialog show a harmless path while the write lands on `~/.ssh/authorized_keys` or a shell startup file, so human-in-the-loop approval is meaningless.
- Friendly Fire (Jul 2026): Claude Code and Codex in auto-approval review modes run attacker binaries disguised as build artifacts while auditing untrusted repositories.
- Meta Sev 1 rogue AI agent (Mar 2026): Agent posted technical advice containing sensitive data without human confirmation.
- ROME agent sandbox escape (Mar 2026): Agent spontaneously initiated crypto mining and reverse SSH tunnel.
- GitHub Copilot YOLO mode (CVE-2025-53773): Prompt injection disables all user confirmations.
- Cursor CurXecute (CVE-2025-54135): Config changes and malicious commands execute before user can reject.

### SSRF in AI Inference and Tooling

URL handling in vision, image, and text-loading helpers fetches attacker-controlled hosts without isolating them from internal networks or cloud metadata services.

**Key incidents:**
- Apify Actors MCP Server CVE-2026-50143 (Jul 2026): A malicious Actor's crafted `webServerMcpPath` is concatenated onto a trusted base URL, redirecting the MCP client so the auto-attached `Authorization: Bearer` header leaks the victim's Apify API token.
- LMDeploy CVE-2026-33626 (Apr 21, 2026): `load_image()` in vision-language module fetches arbitrary URLs without IP filtering; first in-the-wild exploitation 12 hours 31 minutes after disclosure.
- LangChain langchain-openai CVE-2026-41488 (Apr 24, 2026): TOCTOU/DNS-rebinding window in `_url_to_size()` between SSRF validation and the separate fetch with independent DNS resolution.
- LangChain langchain-text-splitters CVE-2026-41481 (Apr 24, 2026): `HTMLHeaderTextSplitter.split_text_from_url()` validated only the initial URL and followed redirects via default `requests.get()` settings.
- mcp-atlassian SSRF CVE-2026-27826 (Feb 2026): Custom `X-Atlassian-Jira-Url` and `X-Atlassian-Confluence-Url` headers caused outbound requests without validation, paired with arbitrary file write CVE-2026-27825 for unauthenticated RCE on network-exposed deployments.

### No Output Destination Control

AI agents send data to arbitrary external endpoints without restriction.

**Key incidents:**
- ChatGPT DNS exfiltration channel (Mar 2026): Sandbox blocked direct network traffic but left DNS resolution unrestricted, enabling data encoded in subdomain labels to leak out.
- EchoLeak (CVE-2025-32711): M365 Copilot exfiltrates data via crafted emails.
- GitHub Copilot CamoLeak (CVE-2025-59145): Data exfiltrated via GitHub Camo proxy image requests encoding secrets in URLs.
- Slack AI exfiltration (Aug 2024): Markdown link rendering enables data exfiltration to attacker servers.
- ASCII smuggling M365 Copilot (Jul 2024): Invisible Unicode in hyperlinks carries stolen MFA codes to external servers.

### AI-Orchestrated Offensive Operations

Threat actors use commercial or open-source AI agents to plan and execute the bulk of an intrusion end to end, with humans only approving decision gates.

**Key incidents:**
- Sygnia AI-accelerated AWS breach (Jul 2026): A lone financially motivated actor used agentic AI to compromise a large AWS environment in about 72 hours, harvesting secrets, planting persistence, exfiltrating RDS data, and staging reversible destructive actions for extortion leverage.
- AWS AI gateway cryptojacking (Jul 2026): An internet-exposed LiteLLM-Proxy instance with a privileged Amazon Bedrock IAM role was brute-forced over SSH and hijacked to run XMRig, with follow-on attempts to abuse its Bedrock access.
- JADEPUFFER agentic ransomware (Jul 2026): Sysdig documented the first extortion operation run end to end by an autonomous LLM agent, which breached a Langflow instance (CVE-2025-3248), pivoted to a Nacos database (CVE-2021-29441), and encrypted 1,342 config items while adapting in real time.
- LLM-generated browser-native ransomware (Jul 2026): Check Point prompted DeepSeek into building a proof-of-concept that abuses the browser File System Access API to read, exfiltrate, and encrypt local files with no native payload.
- GTG-1002 Chinese espionage (Sep-Nov 2025): Claude Code executed 80-90% of tactical operations against ~30 orgs after operators posed as legitimate red teamers.
- First AI-developed zero-day (May 2026): Google GTIG disrupted a cybercrime plan in which an LLM discovered and weaponized a 2FA-bypass zero-day for mass exploitation, the exploit bearing LLM hallmarks like a hallucinated CVSS score.
- FAMOUS CHOLLIMA AI-scaled intrusions (reported Jun 2026): CrowdStrike attributed 47% of hands-on-keyboard intrusions on technology firms (Apr 2025-Mar 2026) to the North Korean group, which uses AI-generated identities to enhance scale and speed.
- CyberStrikeAI FortiGate campaign (Jan-Feb 2026): Russian-speaking actor used commercial GenAI plus the open-source CyberStrikeAI framework to compromise 600+ FortiGate devices across 55 countries without exploiting a single CVE.
- HexagonalRodent Web3 developer campaign (Q1 2026, reported Apr 23, 2026): North Korean APT subgroup of Famous Chollima used Cursor, ChatGPT, and Anima to author malware, build fake company websites, and craft phishing lures; stole an estimated $12 million in crypto across 26,584 wallets exfiltrated from 2,726 developer systems.
- Drift Protocol $285M exploit (Apr 2026): UNC4736 ran a six-month multi-channel social engineering campaign against multisig signers.
- CanisterWorm and CanisterSprawl (Mar-Apr 2026): First and second npm worms to use decentralized ICP infrastructure as C2, with the second adding cross-ecosystem hop to PyPI when a developer holds both tokens.

### Credential Theft via AI Tools

AI development tools become vectors for credential and secret exposure.

**Key incidents:**
- LiteLLM SQL injection (CVE-2026-42208, CVSS 9.3, Apr 2026): A crafted Bearer header runs arbitrary SQL against the proxy database, reading upstream OpenAI, Anthropic, and Bedrock provider keys; exploited within 36 hours.
- Ollama "Bleeding Llama" (CVE-2026-7482, May 2026): An unauthenticated out-of-bounds read in GGUF quantization leaks API keys and conversation memory from 300,000+ servers.
- Amazon Q Developer (CVE-2026-12957/12958, Jun 2026): Auto-loaded MCP configs spawn processes that inherit and exfiltrate AWS keys, SSH sockets, and CLI tokens.
- Hades PyPI worm (Jun 2026): A `.pth` startup-hook stealer harvests Anthropic, GitHub, npm, and cloud credentials, and embeds prompt injection to fool LLM-based package scanners.
- LiteLLM CVE-2026-35030 (Apr 2026): OIDC userinfo cache keyed on token[:20] lets an attacker collide with a legitimate cached token and inherit that user's identity across the gateway.
- FastGPT CVE-2026-40351 and CVE-2026-40352 (Apr 2026): TypeScript type assertion without runtime validation lets NoSQL operator injection log in as any user, including root; password-change endpoint bypasses old-password verification.
- Red Hat OpenShift AI odh-dashboard (CVE-2026-5483, Apr 2026): NodeJS endpoint discloses Kubernetes Service Account tokens usable against the cluster API.
- Claude Code API key exfiltration (CVE-2026-21852): Malicious settings redirect API requests before trust prompt.
- Claude Code InversePrompt (CVE-2025-54795): AI helps reverse-engineer its own security to enable command injection.
- CrewAI "Uncrew" (Nov 2025): Improper error handling exposes admin GitHub token to all private repos.
- GitHub Copilot training data leakage (May 2024): Copilot reproduces real secrets from training data; 40% higher leakage rate.

### Inference Server Authentication and Memory-Safety Failures

LLM serving and inference engines expose unauthenticated endpoints or mishandle attacker-supplied model files, leaking memory or bypassing access control.

**Key incidents:**
- Ollama "Bleeding Llama" (CVE-2026-7482, May 2026): Out-of-bounds heap read during GGUF quantization leaks process memory from 300,000+ unauthenticated servers.
- vLLM OpenAI API auth bypass (CVE-2026-48746, Jun 2026): A crafted `Host` header manipulates path reconstruction so the API-key check fails open.
- NVIDIA Triton Inference Server (CVE-2026-24207, May 2026): Authentication bypass plus memory-safety flaws across the serving stack and DALI backend.
- LiteLLM SQL injection (CVE-2026-42208, Apr 2026): A pre-auth Bearer header runs arbitrary SQL against the proxy's credential store.
- Ollama for Windows auto-updater (CVE-2026-42248/42249, May 2026): No-op signature verification plus ETag path traversal plant a persistent executable in the Startup folder.

### Adversarial Evasion of AI Security Scanners

Attackers craft payloads that target the AI defenders themselves, embedding instructions to mislead LLM-based analysis and review tools.

**Key incidents:**
- SkillCloak scanner evasion (Jul 2026): Self-extracting packing and structural obfuscation keep malicious AI agent skills fully functional while evading every one of 8 tested scanners more than 90% of the time, dropping the best static scanner from 99% to 10% detection.
- Ghostcommit image-hidden injection (Jul 2026): Prompt injection carried inside a PNG referenced by an `AGENTS.md` file is never inspected because AI code reviewers skip image files, and stolen secrets are encoded as integer constants to evade secret scanners.
- GuardFall shell-injection bypass (Jun 2026): Obfuscated commands survive pattern-based command guards in 10 of 11 open-source AI coding agents because the guards inspect the raw string while Bash performs quote removal, expansion, and command substitution before execution.
- Hades PyPI worm (Jun 2026): Malicious packages embed plain-text prompt injection that tells LLM-based package-analysis tools to classify them as safe.
- Malicious LLM routers (Apr 2026): Routers rewrite tool calls and exfiltrate secrets while passing as legitimate middleware.

### Hallucinated Artifact Exploitation

Attackers pre-position malicious resources at the names, domains, or packages that LLMs reliably invent, so an agent or user that trusts model output is routed straight to attacker infrastructure.

**Key incidents:**
- HalluSquatting (Jul 2026): Attackers register the fake package and repository names that models reliably invent, then serve malicious code plus hidden prompt injection when an assistant fetches the "real" resource; researchers found an 85% consistency rate for the invented names and a 100% success rate for skill-install requests across Cursor, Windsurf, Copilot, Cline, Gemini CLI, and OpenClaw.
- Phantom Squatting (Unit 42, Jun 2026): From 685,339 prompts across 913 brands, models produced ~250,000 registrable non-existent domains; adversaries registered predicted domains to host a phishing kit and a malicious Android APK, exploiting the "zero-reputation bypass" of freshly minted domains.
- LLM phantom-squatting phishing (Check Point, Jul 2026): Attackers register AI-hallucinated domains, including a postal-service lookalike behind the "Montana Empire" credential-theft kit, to catch traffic misdirected by model output.

---

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on adding incidents.

---

## License

[MIT](LICENSE)
