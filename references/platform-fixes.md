# Platform Fixes

Use these notes after the main workflow confirms a suspected WebRTC leak. Retest after each change in the same browser profile where the leak was observed.

## Detection Checklist

- Test while the VPN/proxy/node is connected.
- Use at least one WebRTC-specific page: `https://browserleaks.com/webrtc`, `https://ipleak.net`, or `https://ippure.com`.
- Record three values separately: normal browsing IP, DNS result, WebRTC candidates.
- If the leak appears only in one browser, fix that browser first; if it appears across browsers, inspect the VPN/proxy client and TUN/UDP behavior.

## Chromium Browsers

Applies to Chrome, Edge, Arc, Brave, and most Chromium-based browsers.

1. Prefer a browser-native WebRTC IP handling control or a trusted extension with clear permissions and source/privacy disclosures.
2. Do not assume "installed" means "protected". Pin/open the extension, turn protection on, and confirm its enabled state.
3. If using WebRTC Network Limiter, remember it limits WebRTC routing but may not fully disable WebRTC; configure its options and retest.
4. If using a disable/toggle-style WebRTC leak extension, keep protection on for normal browsing and turn it off only when a meeting, voice call, or P2P site requires WebRTC.
5. Retest. If a real public IP still appears, test another browser to separate browser behavior from VPN/proxy routing.

Notes:
- Extension labels vary by version. The intent is to stop WebRTC from using network interfaces or public IPs that normal web traffic is not using.
- Some Chromium browsers expose this as a privacy setting such as "WebRTC IP handling policy" or "disable non-proxied UDP".

## Firefox Desktop

Most reliable privacy-first fix:

1. Open `about:config`.
2. Search `media.peerconnection.enabled`.
3. Set it to `false`.
4. Restart or reload the test page, then retest.

Less disruptive alternative for users who need WebRTC:

- Keep WebRTC enabled but harden ICE/proxy settings if the browser version exposes them, then retest carefully. If the user needs a guaranteed no-leak answer, use the full disable switch above.

## Firefox Android

1. Use Firefox, Firefox Beta, or another Android Firefox build that exposes `about:config`.
2. Set `media.peerconnection.enabled` to `false`.
3. Retest in that same browser.

If the installed mobile browser hides advanced settings, recommend switching to a browser that allows WebRTC control for privacy-sensitive browsing.

## Safari on macOS

The source video treats Safari as hard to fix reliably at the browser layer. Prefer solving it in the proxy/VPN client.

For Shadowrocket-style clients on macOS:

1. Open the proxy client settings.
2. Find `UDP`.
3. Enable `Disable STUN` or the equivalent STUN-blocking option.
4. Retest in Safari and Chromium. The expected result is that both browsers stop exposing the real WebRTC IP.

If the current proxy client has no UDP/STUN control, prefer Firefox with WebRTC disabled for privacy-sensitive sessions, or switch to a client/profile that can proxy or block STUN/UDP.

## iPhone and iPad

Safari/iOS WebRTC controls vary by iOS release and may be limited.

For Shadowrocket-style clients:

1. Open the proxy app.
2. Go to settings.
3. Find `UDP`.
4. Enable `Disable STUN` or the equivalent STUN-blocking option.
5. Retest in Safari and any in-app browser that matters.

This should apply in both global and rule-based routing modes if the client implements the STUN switch correctly. If Safari still leaks and cannot be controlled, use a browser/proxy combination that passes the test or move the sensitive workflow to a tested desktop browser.

## Android Chrome

Chrome on Android may expose fewer WebRTC privacy controls than desktop. If testing shows a leak:

1. Try Firefox Android and disable `media.peerconnection.enabled`.
2. Confirm the VPN/proxy app is in full-tunnel/TUN mode and handles UDP.
3. Retest. Do not assume the fix applies to Chrome unless Chrome itself passes.

## Proxy Client and Network Layer

When multiple browsers leak:

- Enable full-tunnel/TUN mode when the client supports it.
- Ensure UDP handling is explicit: proxy it, block it, or disable STUN when the client exposes that control.
- Check IPv6 separately; a WebRTC fix may not cover IPv6 leaks.
- Re-run DNS leak tests separately. DNS leaks are adjacent but require their own fix.

## Source Notes

- Original video source: `https://www.youtube.com/watch?v=ONd_rAYLji0`
- The video's practical flow: demonstrate a WebRTC leak, explain STUN/peer-connection exposure, test with IP leak sites, then fix Chrome, Firefox, Safari/macOS, iPhone Shadowrocket, and Android Firefox.
- The video warns that Chrome extensions must be explicitly enabled and retested; it specifically cautions that Network Limiter-style tools may not be equivalent to fully disabling WebRTC.
- The video's macOS/iPhone proxy-client fix is `UDP` -> `Disable STUN` in Shadowrocket-style clients.
- BrowserLeaks documents the common split: Chrome can use WebRTC routing-limiter controls/extensions; Firefox can disable WebRTC with `media.peerconnection.enabled=false`: `https://browserleaks.com/webrtc`
- Chrome Web Store describes WebRTC Network Limiter as limiting private interface IPs, non-default public IPs, and requiring WebRTC traffic to use configured proxy routing: `https://chromewebstore.google.com/detail/webrtc-network-limiter/npeicpdbkakmehahjeeohfdhnlpdklia`
