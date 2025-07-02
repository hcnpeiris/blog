# Web Cache Poisoning


## Introduction
**Web Cache Poisoning (WCP)** means caching an unusual response and serving it to other users. There are mainly two types of attacks:

1. Caching with **XSS**
2. **DoS** by triggering an error page

To understand this properly, you need a good idea of how caching works — including cache keys and collisions, cache oracles, and cache busters. A clear explanation of these concepts is available here:  
[PortSwigger | Web cache poisoning](https://portswigger.net/web-security/web-cache-poisoning)

---

## How to Identify It

### 01. Identifying cache oracles

First, we need to check if a response is being cached and analyze its behavior (this involves identifying a **good cache oracle**).

Examples of cache oracles:
- `Age`
- `Cache-Status`

> Note: These oracles depend on the CDN.

The research paper **[Internet’s Invisible Enemy: Detecting and Measuring Web Cache Poisoning in the Wild](https://www.jianjunchen.com/p/web-cache-posioning.CCS24.pdf)** lists various CDN cache oracle identification fields in **Table 06**:  

---

### 02. Understanding Cache Keys

Next, understand which parts of the request are:
- **Keyed** (used in the cache key)
- **Unkeyed** (ignored by cache but can affect response)

Typically:
- `Host` and `Query String` → **Keyed**
- Most headers → **Unkeyed**

### Attack Concept

By keeping **cache key headers unchanged** and modifying **unkeyed headers**, an attacker might cause the cache to store an unusual or malicious response after the original cache expires.

To avoid affecting live users:
- Add a **unique query parameter** or
- Change part of a **cache key header field**

This way, the poisoned response won’t impact normal users.

---

## Security Risks

Saving a poisoned response can lead to two main risks:

1. **Reflected XSS Attacks**  
   🔗 [Research Paper – *Internet’s Invisible Enemy*](https://www.jianjunchen.com/p/web-cache-posioning.CCS24.pdf)

2. **Caching Error Pages → DoS Attacks**  
   🔗 [PortSwigger Article](https://portswigger.net/research/responsible-denial-of-service-with-web-cache-poisoning)

---

## Tools to Help

To find **unkeyed headers** that can change the response:

- Use **Burp Suite Param Miner extension**
- Refer to **mostly reported headers** listed in:
  - [PortSwigger](https://portswigger.net/web-security/web-cache-poisoning)
  - *Internet’s Invisible Enemy: Detecting and Measuring Web Cache Poisoning in the Wild*  
    🔗 [PDF Link](https://www.jianjunchen.com/p/web-cache-posioning.CCS24.pdf)

---

