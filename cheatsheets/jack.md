## how to fix jack on macOS

[source][001]

1. first we should put this script by following  file path:
`~/Library/LaunchAgents/org.jackaudio.jackd.plist`

2. Script:
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
  <dict>
    <key>Label</key>
    <string>org.jackaudio.jackd</string>
    <key>WorkingDirectory</key>
    <string>/Users/Maksim/</string>
    <key>ProgramArguments</key>
    <array>
      <string>/usr/local/Cellar/jack/1.9.19/bin/jackd</string>
      <string>-d</string>
      <string>coreaudio</string>
    </array>
    <key>EnableGlobbing</key>
    <true/>
    <key>RunAtLoad</key>
    <true/>
    <key>KeepAlive</key>
    <true/>
  </dict>
</plist>
```

3. Then reload the script:
```
launchctl unload -w ~/Library/LaunchAgents/org.jackaudio.jackd.plist
launchctl load -w ~/Library/LaunchAgents/org.jackaudio.jackd.plist
```
4. Check then jackd is running:
```
ps -ef | grep jack

## output
1000 10763     1   0  5:15PM ??         0:27.48 /usr/local/bin/jackd -d coreaudio
1000 11717  4885   0  5:39PM ttys003    0:00.00 grep --color=auto jack
```

---

THE END

[001]: https://elatov.github.io/2018/03/creating-a-launchd-plist-for-jackd/
"jack instructions"
