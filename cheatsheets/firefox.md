## firefox cheatsheet

### firefox advanced security settings

1. make sure you are running the latest version of Firefox
2. make sure you configured Firefox for privacy using [SK video][001] and/or
   [THO video][002]
3. in Privacy & Security, disable Deceptive Content and Dangerous Software
   Protection
4. in new tab open `about:config` and set the following

* from [this source][001]

```
identity.fxaccounts.enabled = false
dom.event.clipboardevents.enabled = false
geo.enabled = false
media.eme.enabled = false
media.navigator.enabled = false
media.peerconnection.enabled = false          # disable WebRTC
network.dns.disablePrefetch = true
network.http.sendRefererHeader = 0
network.prefetch-next = false
privacy.firstparty.isolate = true
privacy.resistFingerprinting = true
privacy.trackingprotection.enabled = true
webgl.disabled =  true
```

* from [this source][004]

```
browser.display.use_document_fonts = 0        # disable fonts fingerprint
dom.storage.enabled  = false                  # disable unic ID for browser
browser.cache.disk.enable = false             # hide the fact of re-visiting the page
browser.cache.memory.enable = false           # hide the fact of re-visiting the page
network.proxy.socks_remote_dns = true         # make it harder for the provider to keep track of which sites you visited
browser.safebrowsing.enabled = false          # to disable the transmission of information about visited sites
browser.safebrowsing.malware.enabled = false  # to disable the transmission of information about visited sites
```

* from [this source][003]

```
security.ssl3.rsa_des_ede3_sha = false        # exclude weak encryption sceam
security.ssl.require_safe_negotiation = true  #
```

5. to copy and backup your profiles open `about:profiles` in new tab

[001]: https://www.youtube.com/watch?v=NH4DdXC0RFw "SK firefox security guide"
[002]: https://www.youtube.com/watch?v=tQhWdsFMc24&list=WL&index=2 "THO
Firefox security guide"
[003]: https://www.youtube.com/watch?v=dwZpjKH8nbo "MO firefox security guide"
[004]: https://am.news/worlds/anonymous.pdf "Anonhandbook"

---

THE END
