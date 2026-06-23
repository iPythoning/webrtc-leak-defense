---
name: webrtc-leak-defense
description: Audit, explain, and remediate WebRTC IP leaks that expose a user's real public or local IP while using a VPN, proxy, node, airport, Shadowrocket, Clash, or browser privacy setup. Use when the user mentions WebRTC leak, real IP leak, IP quality tests, VPN/proxy still showing a real city, STUN, browser leak tests, Chrome/Edge/Firefox/Safari/iOS/Android WebRTC settings, or asks for a cross-platform privacy fix.
---

# WebRTC Leak Defense

## Core Rule

Treat this as a privacy diagnostic and remediation workflow. Verify the leak before changing settings, distinguish WebRTC leaks from DNS leaks, and retest after every fix.

## Quick Workflow

1. Identify the user's platform, browser, and network tool:
   - Desktop: Chrome, Edge, Brave, Firefox, Safari, Arc, Opera.
   - Mobile: iPhone/iPad Safari, Android Chrome/Firefox.
   - Network layer: VPN app, proxy client, Shadowrocket, Clash, V2Ray, sing-box, TUN mode.
2. Run a baseline leak test:
   - Ask the user to open `https://ippure.com`, `https://browserleaks.com/webrtc`, or `https://ipleak.net` in the exact browser they use.
   - Compare normal HTTP IP, DNS result, and WebRTC public/local candidates.
3. Interpret results:
   - Public WebRTC IP or city differs from the proxy/VPN exit: critical WebRTC leak.
   - DNS server geography differs but WebRTC candidates are clean: DNS leak, not this workflow.
   - Only private LAN, CGNAT, IPv6 link-local, or mDNS candidates appear: lower risk; explain what is visible and retest on the user's threat model.
   - No WebRTC candidates, or only the proxy/VPN exit appears: pass.
4. Apply the smallest platform-specific fix. For exact steps, read `references/platform-fixes.md`.
5. Retest with the same site and browser. A fix is complete only when WebRTC no longer exposes the real public IP/city.

## Response Style

- Explain in plain language: WebRTC can create direct peer-connection discovery traffic, often through STUN, that may bypass browser proxy routing.
- Do not promise anonymity. Say what was checked, what still might leak, and which apps/browsers remain untested.
- Warn that blocking WebRTC can break or degrade video calls, voice calls, live chat, P2P transfer, and some web meeting tools.
- Prefer reversible browser settings or official extensions before OS/firewall changes.
- Keep affiliate links, creator promotions, and unrelated product recommendations out of the fix unless the user explicitly asks.

## Completion Criteria

Report the final state as one of:

- `pass`: tested browser no longer exposes real public IP through WebRTC.
- `partial`: WebRTC is fixed, but DNS, IPv6, another browser, or another app still needs testing.
- `blocked`: the user's target browser/app does not expose a reliable WebRTC control, or the user cannot run the retest.

