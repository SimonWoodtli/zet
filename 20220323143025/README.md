# Firefox Privacy Settings 🤓

* Change this settings in `about:config`. The settings get stored in `about:support` in the profile directory as prefs.js.
* To create a new profile: `about:profiles`

```
# Disable WebRTC:
# WebRTC can give up your real IP even when using VPN or Tor.

media.peerconnection.enabled = false

# Enable fingerprint resistance:  With this alone we pretty much negate the need for canvas defender, or any other fingerprint blocking addon.

privacy.resistfingerprinting = true

# 3DES Cypher:
# 3DES has known security flaws

security.ssl3.rsa_des_ede3_sha = false

# Require Safe Negotiation:  Optimize SSL

security.ssl.require_safe_negotiation = true


# Disable older TLS like 1.0, 1.1

security.tls.version.min = 3
tls.version.max = 4

# Disable 0 round trip time to better secure your forward secrecy
security.tls.enable_0rtt_data = false

# Disable Automatic Formfill
browser.formfil.enable = false

# Disable disk caching
browser.cache.disk.enable = false
browser.cache.disk_cache_ssl = false
browser.cache.memory.enable = false
browser.cache.offline.enable = false
browser.cache.insecure.enable = false

# Disable geolocation services
geo.enabled = false


# Disable plugin scanning.
# Can improve functionality, as some sites scan for adblockers and script blockers. Should be used even on non-hardened firefox.

plugin.scan.plid.all = false


# Disable ALL telemetery

browser.newtabpage.activity-stream.feeds.telemetry browser.newtabpage.activity-stream.telemetry = false
browser.pingcentre.telemetry = false
devtools.onboarding.telemetry-logged = false
media.wmf.deblacklisting-for-telemetry-in-gpu-process = false
toolkit.telemetry.archive.enabled = false
toolkit.telemetry.bhrping.enabled = false
toolkit.telemetry.firstshutdownping.enabled = false
toolkit.telemetry.hybridcontent.enabled = false
toolkit.telemetry.newprofileping.enabled = false
toolkit.telemetry.unified = false
toolkit.telemetry.updateping.enabled = false
toolkit.telemetry.shutdownpingsender.enabled = false


# Disable WebGL
# Allows direct access to GPU.
webgl.disabled = true

# Enable first-party isolation
# Prevents browsers from making requests outside of the primary domain of the website. Prevents supercookies. may cause websites that rely on 3rd party scripts and libraries to break, however those are generally only used for tracking.

privacy.firstparty.isolate = true

# Disable TLS false start
security.ssl.enable_false_start = false
```
