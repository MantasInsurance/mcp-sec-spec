# MCP-Sec v0.1 Specification

**Title:** Model Context Protocol Security (MCP-Sec)  
**Version:** 0.1  
**Author:** Basil Mimi  
**Date:** April 6, 2025

---

## 1. Introduction

Model Context Protocol Security (MCP-Sec) is a lightweight, extensible open standard for securing the dynamic context fed into AI models.  
It ensures that context payloads are authentic, untampered, auditable, and safe for model consumption.

MCP-Sec protects AI models against manipulation through compromised or malicious input at runtime.

---

## 2. Threat Model

MCP-Sec defends against:

- **Context Tampering**: Payloads modified in transit.
- **Unauthorized Context Injection**: Malicious entities inserting fake payloads.
- **Context Replay Attacks**: Reusing old valid payloads to manipulate behavior.
- **Context Poisoning**: Embedding subtle backdoors or adversarial triggers in context.
- **Privacy Breaches**: Leaking sensitive context data.
- **Audit Evasion**: Hiding traces of manipulated or injected contexts.

---

## 3. Protocol Overview

MCP-Sec secures context via:

1. **Context Creation**: Payload built, signed, and timestamped by trusted producer.
2. **Secure Transport**: Encrypted transmission (TLS/mTLS).
3. **Context Reception**: Signature verification, schema validation, timestamp/nonce check.
4. **Context Consumption**: Only verified context feeds into models.
5. **Auditing**: Secure, immutable logging of context usage.

---

## 4. Message Format

| Field | Type | Description |
|:--|:--|:--|
| context_id | UUID | Unique ID for context payload |
| issuer | String | Trusted producer identity |
| issued_at | ISO8601 | UTC timestamp of creation |
| expires_at | ISO8601 | UTC expiration (optional) |
| nonce | String | Unique random token |
| schema_version | String | Schema version for payload |
| payload | Object | Actual context data |
| signature | Base64 String | Digital signature |

**Example:**

```json
{
  "context_id": "uuid-1234",
  "issuer": "api.service.com",
  "issued_at": "2025-04-06T12:00:00Z",
  "expires_at": "2025-04-06T12:05:00Z",
  "nonce": "ae32dc30f1c64f1c8b73",
  "schema_version": "1.0",
  "payload": {
    "user_id": "123456",
    "location": "UAE",
    "device": "mobile"
  },
  "signature": "BASE64_ENCODED_SIGNATURE"
}
