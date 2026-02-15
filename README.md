# Gibs MCP Server

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)

**Regulatory compliance for AI-powered development tools** — classify AI systems, check obligations, and get article-level citations across 240+ articles and 3 major regulations.

[Full Documentation](https://docs.gibs.dev) · [Get API Key](https://gibs.dev) · [API Reference](https://docs.gibs.dev/api-reference)

---

## Overview

Gibs MCP Server connects your AI development environment directly to a regulatory compliance knowledge base. Currently covering:

| Regulation | Scope | Articles |
|---|---|---|
| **EU AI Act** | AI system classification, prohibited practices, obligations by risk level | 113 articles + 13 annexes |
| **GDPR** | Data protection, processing obligations, data subject rights | 99 articles |
| **DORA** | ICT risk management, incident reporting, third-party oversight for financial entities | 64 articles + 12 delegated/implementing acts |

Every response includes **article-level citations** to binding legal text, with real-time corpus updates as regulations evolve.

---

## Quick Start

### 1. Get an API Key

Sign up at [gibs.dev](https://gibs.dev) and grab your API key from the dashboard.

### 2. Connect via Claude Desktop

Add this to your Claude Desktop configuration file (`claude_desktop_config.json`):

```json
{
  "mcpServers": {
    "gibs": {
      "url": "https://mcp.gibs.dev/sse"
    }
  }
}
```

### 3. Authenticate

Pass your API key as the `user_api_key` parameter when calling any tool, or set it in your environment:

```bash
export GIBS_API_KEY=your_api_key_here
```

---

## Tools

### `classify_ai_system`

Classify an AI system under AI Act risk levels (unacceptable, high-risk, limited, minimal) with full legal reasoning.

| Parameter | Type | Required | Description |
|---|---|---|---|
| `description` | string | Yes | What the AI system does (10–5000 chars) |
| `data_types` | list[string] | No | Types of data processed (e.g., `["biometric", "personal"]`) |
| `decision_scope` | string | No | What decisions the system influences |
| `sector` | string | No | Industry sector (e.g., `"healthcare"`, `"finance"`, `"hr"`) |

**Example prompt:**

> Classify my facial recognition attendance system for schools

**What you get back:**

- Risk classification (e.g., **high-risk** under Annex III, point 1(a))
- Specific Article 6 analysis with Annex III cross-references
- Applicable obligations based on classification
- Relevant exemptions or exceptions, if any

---

### `check_compliance`

Ask any regulatory compliance question and get a structured answer with article-level citations.

| Parameter | Type | Required | Description |
|---|---|---|---|
| `question` | string | Yes | Compliance question (10–2000 chars) |
| `system_description` | string | No | Context about your AI system or organization |
| `regulation` | string | No | Target regulation: `"ai_act"`, `"gdpr"`, `"dora"`, or `"both"` (default: `"both"`) |

**Example prompts:**

> What are the GDPR requirements for a data breach notification?

Returns Article 33 (notification to supervisory authority) and Article 34 (communication to data subjects) obligations, including the 72-hour notification timeline, required content, and exceptions.

> Does DORA apply to a fintech startup processing payments?

Returns Article 2 scope analysis with financial entity classification, applicable ICT risk management obligations, and relevant delegated act requirements.

---

### `health_check`

Check if the compliance API is available. No parameters required.

---

## Usage Examples

### 1. AI System Classification

```
You: Classify my facial recognition attendance system for schools

Gibs: This system is classified as HIGH-RISK under the EU AI Act.

- Article 6(2) read with Annex III, point 1(a): biometric identification
  and categorisation of natural persons
- As used in an educational context, also caught by Annex III, point 3(a):
  AI systems intended to be used to determine access to educational institutions

Obligations include:
- Risk management system (Article 9)
- Data governance (Article 10)
- Technical documentation (Article 11)
- Record-keeping (Article 12)
- Transparency to users (Article 13)
- Human oversight (Article 14)
- Accuracy, robustness, cybersecurity (Article 15)
- Conformity assessment before placing on market (Article 43)
```

### 2. GDPR Data Breach Obligations

```
You: What are the GDPR requirements for a data breach notification?

Gibs: GDPR imposes two notification obligations following a personal data breach:

1. Notification to supervisory authority — Article 33
   - Without undue delay and no later than 72 hours after becoming aware
   - Must include: nature of breach, categories and approximate number of
     data subjects, likely consequences, measures taken or proposed
   - Exception: unlikely to result in a risk to rights and freedoms

2. Communication to data subjects — Article 34
   - Required when breach is likely to result in a HIGH risk
   - Must describe in clear and plain language the nature of the breach
   - Not required if: appropriate technical safeguards applied (e.g.,
     encryption), subsequent measures taken that eliminate the high risk,
     or it would involve disproportionate effort (use public communication instead)

The controller must also document all breaches per Article 33(5), regardless
of whether they meet the notification threshold.
```

### 3. DORA Scope Analysis

```
You: Does DORA apply to a fintech startup processing payments?

Gibs: Yes. Under Article 2(1), DORA applies to payment institutions as
defined in Directive 2015/2366 (PSD2), which includes entities authorized
to provide payment services.

As a financial entity under DORA, the startup must comply with:
- ICT risk management framework (Chapter II, Articles 5–16)
- ICT-related incident management and reporting (Chapter III, Articles 17–23)
- Digital operational resilience testing (Chapter IV, Articles 24–27)
- ICT third-party risk management (Chapter V, Articles 28–44)
- Information-sharing arrangements (Chapter VI, Article 45)

Note: Proportionality applies — Article 4 allows competent authorities to
consider the size, risk profile, and complexity of the financial entity when
assessing compliance.
```

---

## Connection Details

| | |
|---|---|
| **SSE Endpoint** | `https://mcp.gibs.dev/sse` |
| **Protocol** | [Model Context Protocol (MCP)](https://modelcontextprotocol.io/) over Server-Sent Events |
| **Authentication** | API key from [gibs.dev](https://gibs.dev) dashboard |

---

## Supported Clients

Any MCP-compatible client can connect to the Gibs server. Tested with:

- [Claude Desktop](https://claude.ai/download)
- [Claude Code](https://docs.anthropic.com/en/docs/claude-code)
- [Cursor](https://cursor.sh)
- Any client supporting the MCP SSE transport

---

## Links

- **Website:** [gibs.dev](https://gibs.dev)
- **Documentation:** [docs.gibs.dev](https://docs.gibs.dev)
- **API Reference:** [docs.gibs.dev/api-reference](https://docs.gibs.dev/api-reference)
- **Python SDK:** [`pip install gibs`](https://pypi.org/project/gibs/)
- **Status:** [status.gibs.dev](https://status.gibs.dev)

---

## License

MIT License. See [LICENSE](LICENSE) for details.

---

Built by [Gibbr AB](https://gibs.dev) — making regulatory compliance accessible for developers.
