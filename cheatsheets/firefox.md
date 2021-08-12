---
title: firefox advanced security settings
category: pivacy
layout:
updated:
---

1. make sure you are running the latest version of Firefox
2. make sure you configured Firefox for privacy using [this video][001]
3. in Privacy & Security, disable Deceptive Content and Dangerous Software
   Protection
4. open `about:config` and set the following

        identity.fxaccounts.enabled = false
        dom.event.clipboardevents.enabled = false
        geo.enabled = false
        media.eme.enabled = false
        media.navigator.enabled = false
        media.peerconnection.enabled = false
        network.dns.disablePrefetch = true
        network.http.sendRefererHeader = 0
        network.prefetch-next = false
        privacy.firstparty.isolate = true
        privacy.resistFingerprinting = true
        privacy.trackingprotection.enabled = true
        webgl.disabled =  true


[001]: https://www.youtube.com/watch?v=NH4DdXC0RFw "SK firefox security guide"

---

THE END
