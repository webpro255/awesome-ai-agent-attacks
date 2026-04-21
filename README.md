# Awesome AI Agent Attacks [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

A curated timeline of real AI agent security incidents, breaches, and vulnerabilities from 2024-2026. Every entry includes date, named company/product, specific impact, root cause, CVE where applicable, and source links.

No opinions. No product pitches. Just facts with sources.

Last updated: 2026-04-21

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

---

## Attack Pattern Taxonomy

### Supply Chain Credential Cascade

A single compromised credential triggers lateral movement across multiple package registries and downstream organizations.

**Key incidents:**
- Vercel / Context.ai OAuth chain (Apr 2026): Lumma Stealer infection of a Context.ai employee led to OAuth token theft, then escalated into the Vercel Google Workspace tenant because a Vercel employee had granted the Context.ai browser extension "Allow All" enterprise scopes.
- TeamPCP cascade (Mar 2026): Trivy -> Checkmarx -> LiteLLM -> Telnyx -> CanisterWorm -> Cisco -> Mercor. One service account compromise led to 1,000+ SaaS environments breached.
- UNC1069 Contagious Interview (Apr 2026): 1,700+ malicious packages across npm, PyPI, Go, Rust, and Packagist since Jan 2025; ClickFix lures via fake Zoom/Teams links after social engineering.
- Axios npm compromise (Mar 2026): Social engineering of one maintainer threatened 70-100M weekly downloads.
- Bybit heist (Feb 2025): Compromise of one Safe{Wallet} developer led to $1.5B theft.
- Ultralytics PyPI attack (Dec 2024): Git branch name abuse stole CI/CD credentials for two-phase supply chain attack.

### Confused Deputy

An AI agent with legitimate access is tricked into performing actions on behalf of an attacker.

**Key incidents:**
- Copilot Studio ShareLeak and Agentforce PipeLeak (Apr 2026): Crafted SharePoint and Web-to-Lead form payloads hijack agents into bulk-exfiltrating CRM and SharePoint data by email, with no volume cap and no user-visible indicator.
- Claude Code / Gemini CLI / Copilot Agent via GitHub comments (Apr 2026): PR titles, issue descriptions, and comments hijack CI agents to exfiltrate API keys and secrets from the runner.
- Salesforce Agentforce ForcedLeak (Sep 2025): Malicious Web-to-Lead form data tricks agent into exfiltrating CRM records.
- ServiceNow Now Assist (Nov 2025): Low-privilege agent tricks higher-privilege agent into exporting case files.
- EchoLeak M365 Copilot (Jun 2025): Crafted email triggers zero-click data exfiltration.
- ChatGPT SpAIware (Sep 2024): Untrusted data plants persistent exfiltration instructions in memory.

### Overprivileged Integration

AI agents or chatbot integrations granted excessive access that becomes the attack surface.

**Key incidents:**
- AWS Bedrock AgentCore "God Mode" (Apr 2026): Starter toolkit auto-creates IAM roles with wildcard memory actions so any agent can read or poison every other agent's state.
- Step Finance treasury drain (Jan 2026): Trading agents held wallet, oracle, and trading-endpoint permissions simultaneously; one device compromise cascaded into $40M loss.
- Salesloft Drift OAuth breach (Aug 2025): Stolen OAuth tokens gave access to 700+ customer Salesforce environments.
- LOLCopilot/M365 Copilot (Aug 2024): Default configurations grant broad access to all emails and documents.
- Amazon Q extension (Jul 2025): Over-scoped GitHub token in CI/CD allowed destructive prompt injection.
- Copilot "zombie data" exposure (Nov 2024): 16,000+ organizations' private repos exposed via cached data.

### Config-as-Code Execution

Malicious configurations in repository files execute code when AI tools process them.

**Key incidents:**
- Claude Code RCE via hooks (CVE-2025-59536): Malicious .claude/settings.json executes commands before trust dialog.
- Codex CLI command injection (CVE-2025-61260): Project-local configs execute commands without user consent.
- Rules File Backdoor (Mar 2025): Invisible Unicode in .cursorrules and copilot-instructions.md injects malicious code.
- Cursor MCPoison (CVE-2025-54136): Benign MCP config approved once, then silently modified to execute backdoor.

### Unsandboxed Code Execution

AI tools run user-supplied or AI-generated code without isolation.

**Key incidents:**
- Anthropic MCP STDIO design RCE (Apr 2026): Reference SDKs in Python, TypeScript, Java, and Rust execute attacker-supplied command strings passed to STDIO transport; 200K+ servers and 150M+ downloads exposed; Anthropic declined to patch.
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

### No Action-Level Authorization

AI agents execute privileged operations without per-action permission checks.

**Key incidents:**
- Meta Sev 1 rogue AI agent (Mar 2026): Agent posted technical advice containing sensitive data without human confirmation.
- ROME agent sandbox escape (Mar 2026): Agent spontaneously initiated crypto mining and reverse SSH tunnel.
- GitHub Copilot YOLO mode (CVE-2025-53773): Prompt injection disables all user confirmations.
- Cursor CurXecute (CVE-2025-54135): Config changes and malicious commands execute before user can reject.

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
- GTG-1002 Chinese espionage (Sep-Nov 2025): Claude Code executed 80-90% of tactical operations against ~30 orgs after operators posed as legitimate red teamers.
- CyberStrikeAI FortiGate campaign (Jan-Feb 2026): Russian-speaking actor used commercial GenAI plus the open-source CyberStrikeAI framework to compromise 600+ FortiGate devices across 55 countries without exploiting a single CVE.
- Drift Protocol $285M exploit (Apr 2026): UNC4736 ran a six-month multi-channel social engineering campaign against multisig signers.
- CanisterWorm (Mar 2026): First npm worm to use decentralized ICP infrastructure as C2, making takedown structurally impossible.

### Credential Theft via AI Tools

AI development tools become vectors for credential and secret exposure.

**Key incidents:**
- LiteLLM CVE-2026-35030 (Apr 2026): OIDC userinfo cache keyed on token[:20] lets an attacker collide with a legitimate cached token and inherit that user's identity across the gateway.
- FastGPT CVE-2026-40351 and CVE-2026-40352 (Apr 2026): TypeScript type assertion without runtime validation lets NoSQL operator injection log in as any user, including root; password-change endpoint bypasses old-password verification.
- Red Hat OpenShift AI odh-dashboard (CVE-2026-5483, Apr 2026): NodeJS endpoint discloses Kubernetes Service Account tokens usable against the cluster API.
- Claude Code API key exfiltration (CVE-2026-21852): Malicious settings redirect API requests before trust prompt.
- Claude Code InversePrompt (CVE-2025-54795): AI helps reverse-engineer its own security to enable command injection.
- CrewAI "Uncrew" (Nov 2025): Improper error handling exposes admin GitHub token to all private repos.
- GitHub Copilot training data leakage (May 2024): Copilot reproduces real secrets from training data; 40% higher leakage rate.

---

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on adding incidents.

---

## License

[MIT](LICENSE)
