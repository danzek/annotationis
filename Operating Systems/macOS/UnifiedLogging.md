# Apple Unified Logging (AUL)

According to [Apple](https://developer.apple.com/documentation/os/logging),

>The unified logging system is available in iOS 10.0 and later, macOS 10.12 and later, tvOS 10.0 and later, and watchOS 3.0 and later. This system supersedes the 
Apple System Logger (ASL) and Syslog APIs.

## Preservation

The AUL lives in the following location(s):

    /private/var/db/diagnostics/
    /var/db/diagnostics/

And the support files are in:

    /var/db/uuidtext

The logs are stored in the [tracev3](https://github.com/libyal/dtformats/blob/main/documentation/Apple%20Unified%20Logging%20and%20Activity%20Tracing%20formats.asciidoc#tracev3_file_format) and [timesync DB](https://github.com/libyal/dtformats/blob/main/documentation/Apple%20Unified%20Logging%20and%20Activity%20Tracing%20formats.asciidoc#timesync_database_file_format) formats.

You can create a `logarchive` file of the logs on a live system with:

    log collect

You can use the `--output` parameter to specify output location. The `log show` command can be used with the `*.logarchive` file.

## Live logs

Stream live logs either by:

- Console app ("Start streaming")
- `log stream`

The latter can also be used with a predicate to filter the logs in realtime.

## Predicate Reference

Predicates include:

**Predicate** | **Description**
------------- | ---------------
`eventType` | Type of event (e.g.,  `aactivityCreateEvent`, `activityTransitionEvent`, `logEvent`, `signpostEvent`, `stateEvent,` `timesyncEvent`, `traceEvent`, `userActionEvent`)
`eventMessage` | Message text
`messageType` | Message type / verbosity level (e.g., `default`, `info`, `debug`, `error`, `fault`)
`process` | Originating process
`processImagePath` | Full path of originating process
`sender` | Originating code (e.g., lib / framework, kext)
`senderImagePath` | Full path of originating code
`subsystem` | `os_log(3)` [API subsystems](https://gist.github.com/krypted/495e48a995b2c08d25dc4f67358d1983) which comes from the `CFBundleIdentifier` in applications' respective `Info.plist` files
`category` | `os_log(3)` API subsystem categories

Examples of how to determine the subsystem associated with various apps:

    $ defaults read /Applications/Cyberduck.app/Contents/Info.plist CFBundleIdentifier
    ch.sudo.cyberduck
    
    $ defaults read /Applications/010\ Editor.app/Contents/Info.plist CFBundleIdentifier
    com.SweetScape.010Editor

## Filters

Various filters to use with the `log` utility's `--predicate` parameter on macOS with potentially associated [MITRE ATT&CK](https://attack.mitre.org/) tactic (to have some semblance of organization&mdash;several of these could be associated with multiple tactics so this is fairly arbitrary). I also include the last version I (or someone else) tested it on and whether there is private data in the log entries:

**Tactic** | **Query** | **Description** | **Last Tested On** | **Private Data**
---------- | --------- | --------------- | ------------------ | ----------------
Execution | `process == "sudo" && eventMessage contains "COMMAND"` | Commands executed with `sudo` privileges | 12.6 | No
Execution | `subsystem == "com.apple.syspolicy.exec" AND process == "syspolicyd" AND category == "default"` | Gatekeeper scans when file(s) opened | 12.6 | Yes
Persistence | `subsystem == "com.apple.ManagedClient" AND process == "mdmclient" AND category == "MDMDaemon" and eventMessage CONTAINS "Installed configuration profile:" AND eventMessage CONTAINS "Source: Manual"` | *Manual* installation of configuration profile | 12.6 | No
Persistence | `subsystem == "com.apple.opendirectoryd" AND process == "opendirectoryd" AND category == "auth" AND eventMessage CONTAINS "Password changed for"` | *Successful* local user password change | 12.6 | No
Persistence | `subsystem == "com.apple.opendirectoryd" AND process == "opendirectoryd" AND category == "auth" AND eventMessage CONTAINS "Failed to change password"` | *Failed* local user password change | 12.6 | No
Defense Evasion | `subsystem == "com.apple.launchservices" AND process == "CoreServicesUIAgent" AND category == "uiagent" AND (eventMessage BEGINSWITH "Saving rejection record:" OR eventMessage CONTAINS "Gatekeeper rejection record")` | Gatekeeper rejection / bypass | 12.6 | Yes
Defense Evasion | `subsystem == "com.apple.ManagedClient" AND process == "mdmclient" AND category == "MDMDaemon" and eventMessage CONTAINS "Removed configuration profile:" AND eventMessage CONTAINS "Source: Manual"` | *Manual* removal of configuration profile | 12.6 | No
Lateral Movement | `processImagePath BEGINSWITH "/System/Library/CoreServices" AND process == "loginwindow" AND eventMessage CONTAINS[c] "INCORRECT"` | Failed lock screen unlock attempt | 12.6 | No
Exfiltration | `subsystem == "com.apple.sharing" AND process == "AirDrop" AND processImagePath BEGINSWITH "/System/Library" AND eventMessage BEGINSWITH "Successfully issued sandbox extension for"` | Outbound Airdrop file transfer (shows filename) | 12.6 | No

*More coming soon(ish)....*

## References

- Apple 
  - [Logging documentation](https://developer.apple.com/documentation/os/logging)
  - [Predicate Programming Guide](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Predicates/AdditionalChapters/Introduction.html)
  - [SystemLogging](https://developer.apple.com/documentation/devicemanagement/systemlogging) &mdash; covers `Enable-Private-Data` key
- [Joachim Metz: Apple Unified Logging and Activity Tracing formats](https://github.com/libyal/dtformats/blob/main/documentation/Apple%20Unified%20Logging%20and%20Activity%20Tracing%20formats.asciidoc)
- [Jamf Unified Log Filters](https://github.com/jamf/jamfprotect/tree/main/unified_log_filters)
- [cmdSecurity: Unified Logs: How to Enable Private Data](https://www.cmdsec.com/unified-logs-enable-private-data/)
- [Jamf: Unified Logs: How to Enable Private Data](https://www.jamf.com/blog/unified-logs-how-to-enable-private-data/)
- Sarah Edwards / mac4n6
  - [Introducing 'Analysis of Apple Unified Logs: Quarantine Edition' [Entry 0]](https://www.mac4n6.com/blog/2020/4/19/introducing-analysis-of-apple-unified-logs-quarantine-edition-entry-0)
  - [Analysis of Apple Unified Logs: Quarantine Edition [Entry 1] – Converting Log Archive Files on 10.15 (Catalina)](https://www.mac4n6.com/blog/2020/4/20/analysis-of-apple-unified-log-quarantine-edition-entry-1-converting-log-archive-files-on-1015-catalina) (*See also* [Converting Unified Logs – A Great Disturbance In The Force](https://cellebrite.com/en/converting-unified-logs-a-great-disturbance-in-the-force/))
  - [...](https://www.mac4n6.com/blog/category/logs)
  - [Analysis of Apple Unified Logs: Quarantine Edition [Entry 11] – AirDropping Some Knowledge](https://www.mac4n6.com/blog/2020/4/20/analysis-of-apple-unified-log-quarantine-edition-entry-1-converting-log-archive-files-on-1015-catalina)
- [CrowdStrike: Finding Waldo: Leveraging the Apple Unified Log for Incident Response](https://www.crowdstrike.com/blog/how-to-leverage-apple-unified-log-for-incident-response/)
- [Mandiant: macOS Unified Logs tool](https://github.com/mandiant/macos-UnifiedLogs)
- [macOS logging subsystems](https://gist.github.com/krypted/495e48a995b2c08d25dc4f67358d1983)
- Howard Oakley / Eclectic Light Company
  - macOS Unified log &mdash; [Part 1: Why, what, and how](https://eclecticlight.co/2018/03/19/macos-unified-log-1-why-what-and-how/) | [Part 2: Content and extraction](https://eclecticlight.co/2018/03/20/macos-unified-log-2-content-and-extraction/) | [Part 3: Finding your way](https://eclecticlight.co/2018/03/21/macos-unified-log-3-finding-your-way/)
  - [Log literacy: all about the log](https://eclecticlight.co/2023/02/08/log-literacy-all-about-the-log/)
- [Skartek: Unified Logging for macOS, an introduction](https://skartek.dev/2022/05/04/unified-logging-for-macos-an-introduction/)
- Cellebrite
  - [How to Add Unified Logs to Cellebrite Inspector](https://cellebrite.com/en/adding-unified-logs-to-cellebrite-inspector/)
  - [Making Sense of Unified Logs in Cellebrite Inspector](https://cellebrite.com/en/making-sense-of-unified-logs-in-cellebrite-inspector/)
  - [Converting Unified Logs – A Great Disturbance In The Force](https://cellebrite.com/en/converting-unified-logs-a-great-disturbance-in-the-force/)
  - [*Archived Blackbag article:* Accessing Unified Logs from an Image](https://web.archive.org/web/20200925031904/https://www.blackbagtech.com/blog/accessing-unified-logs-image/)
- [Yogesh Khatri: UnifiedLogReader tool](https://github.com/ydkhatri/UnifiedLogReader)
