# Background Intelligent Transfer Service (BITS)

According to [Microsoft](https://msdn.microsoft.com/en-us/library/windows/desktop/bb968799(v=vs.85).aspx),

> Background Intelligent Transfer Service (BITS) transfers files (downloads or uploads) between a client and server and provides progress information related to the transfers. You can also download files from a peer.

This is sort of like `wget` for Windows except it does asynchronous transfers (in background or foreground) and uses "idle" network bandwidth (self-throttles and prioritizes traffic). It also can survive network disconnection and computer reboots and will automatically resume file transfers after such events (it also continues transferring data *even if the user logs off* &mdash; see ["About BITS"](https://msdn.microsoft.com/en-us/library/windows/desktop/aa362708(v=vs.85).aspx)). Windows uses it a lot for downloading OS and/or Defender updates.

Windows 10 has notoriously made it difficult to disable BITS (some users report that you can no longer disable it from `services.msc`, but I've heard conflicting responses to this). [Some report success](https://superuser.com/a/1121161/102096) disabling it by configuring WiFi as a "Metered Connection".

## Types of Transfer Jobs

There are three types of BITS transfer jobs (defined in [the `BG_JOB_TYPE` enum](https://msdn.microsoft.com/en-us/library/windows/desktop/aa362811(v=vs.85).aspx)):

1. **Download** (`BG_JOB_TYPE_DOWNLOAD`) &mdash; downloads files to the client
2. **Upload** (`BG_JOB_TYPE_UPLOAD`, *not supported in BITS 1.2 and earlier*) &mdash; uploads a file to the server
3. **Upload-reply** (`BG_JOB_TYPE_UPLOAD_REPLY`, *not supported in BITS 1.2 and earlier*) &mdash; uploads a file to the server and receives a reply from the server application

## Peer Caching

According to [Microsoft](https://msdn.microsoft.com/en-us/library/windows/desktop/aa964314(v=vs.85).aspx),

> Starting with Background Intelligent Transfer Service (BITS) 4.0, the BITS service was extended to allow subnet-level peer caching for downloaded URL data by using Windows BranchCache. BITS clients can retrieve data from other computers in their own subnet that have already downloaded the data, instead of retrieving the data from remote servers. For more information about Windows BranchCache, see the BranchCache Overview.
>
> If an administrator enables Windows BranchCache on client and server computers in an organization through a group policy or local configuration settings, BITS will use Windows BranchCache for data transfers. 

Admins can disable BranchCache via Group Policy, or individual applications can disable it by calling the [`IBackgroundCopyJob4::SetPeerCachingFlags` method](https://msdn.microsoft.com/en-us/library/windows/desktop/aa964249(v=vs.85).aspx) and setting the `BG_DISABLE_BRANCH_CACHE` flag.

This does not stop Windows from using BITS to transfer data over SMB, though. For BITS 3.0, "starting with Windows 7, the BITS 3.0 peer caching model is deprecated. If BITS 4.0 is installed, the BITS 3.0 peer caching model is unavailable" ([SOURCE](https://msdn.microsoft.com/en-us/library/windows/desktop/aa362708(v=vs.85).aspx)).

## PowerShell & Interfaces

According to [Microsoft](https://msdn.microsoft.com/en-us/library/windows/desktop/aa363160(v=vs.85).aspx),

> Starting with Windows 10, version 1607, you can also run PowerShell Cmdlets and use BITSAdmin or other applications that use the BITS [interfaces](https://msdn.microsoft.com/en-us/library/windows/desktop/aa362819(v=vs.85).aspx) from a PowerShell Remote command line connected to another machine (physical or virtual). This capability is not available when using a [PowerShell Direct](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/vmsession) command line to a virtual machine on the same physical machine, and it is not available when using WinRM cmdlets.
>
> A BITS Job created from a Remote PowerShell session will run under that session’s user account context and will only make progress when there is at least one active local logon session or Remote PowerShell session associated with that user account. For more information, see [To manage PowerShell Remote sessions](https://msdn.microsoft.com/en-us/library/windows/desktop/ee663885(v=vs.85).aspx#manage_ps_remote_sessions).

To download a file using BITS via PowerShell, as an example, this pulls down the license file from this repo to my machine:

    Start-BitsTransfer https://raw.githubusercontent.com/danzek/annotationis/master/LICENSE.md

You can also use `-Source` and `-Destination` parameters and/or use the `-TransferType` parameter to use another transfer job type. [See "Using Windows PowerShell to Create BITS Transfer Jobs"](https://msdn.microsoft.com/en-us/library/windows/desktop/ee663885(v=vs.85).aspx) for many more examples.

The BITS interface allows transfer policy settings to be specified such as avoiding downloads over metered connections via [flags in the `BITS_COST_STATE` enum.](https://msdn.microsoft.com/en-us/library/windows/desktop/mt595901(v=vs.85).aspx) While this makes sense to avoid making a user pay for downloads where bandwidth is expensive, this could also be abused by malware. For instance, policy for a network (that has a firewall, IDS, other monitoring, etc.) could be set to say that it is a metered network, and thus the malware could avoid download activity while connected to that network.

## Queue Manager

BITS can create Queue Manager files that track transfers (I've observed this on Windows 7). These files are typically saved with a `.dat` extension to:

    %ALLUSERSPROFILE%\Microsoft\Network\Downloader

Typically `qmgr0.dat` and `qmgr1.dat`. You can also view current transfers on a live system using PowerShell with admin privileges:

    Get-BitsTransfer -AllUsers

Relevant event logs are stored in:

    Microsoft-Windows-Bits-Client/(Microsoft-Windows-Bits-Client/Operational.evtx

On Windows 10, it apppears the BITS transfers are stored in a `.db` file (and I also observed `.log` files) in the same location. BITS v5.0 is included in Windows 10, where the version of `%windir%\System32\QMgr.dll` is "`7.8.xxxx.xxxx`". I am not sure how to parse this database file yet (please contact me to let me know or fork this repo and issue a pull request to this page explaining it more!).

## Tools

 - The [French National Agency for Information Systems Security (Agence nationale de la sécurité des systèmes d'information / ANSSI-FR)](https://www.ssi.gouv.fr/) released [bits_parser](https://github.com/ANSSI-FR/bits_parser) which extracts BITS jobs from QMGR queues and stores parsed results in CSV format. [Xavier Mertens wrote a blog post](https://isc.sans.edu/forums/diary/Investigating+Microsoft+BITS+Activity/23281/) about using this tool. Note that *Python 3.3+ is required* (this was not documented anywhere when I first went to install it).

 - Matthew Geiger, ["Finding your naughty BITS"](https://www.dfrws.org/sites/default/files/session-files/pres-finding_your_naughty_bits.pdf), presentation delivered at *DFRWS 2015 USA*, August 2015

 - MSDN, ["How to control whether a BITS job is allowed to download over an expensive connection"](https://msdn.microsoft.com/en-us/library/hh994437%28v=vs.85%29.aspx)

 - MSDN, ["BITS Reference"](https://msdn.microsoft.com/en-us/library/aa362820%28v=vs.85%29.aspx)

 - [Microsoft BITS Samples and Tools](https://msdn.microsoft.com/en-us/library/windows/desktop/aa362824(v=vs.85).aspx)

 - [The Microsoft BITSAdmin tool](https://msdn.microsoft.com/en-us/library/windows/desktop/aa362813(v=vs.85).aspx) is deprecated as of Windows 7 (BITS is now integrated into PowerShell).

 - [SecureWorks has written about malware leveraging BITS](https://www.secureworks.com/blog/malware-lingers-with-bits) to evade remediation. It also appears they wrote a [010 Editor](https://www.sweetscape.com/download/010editor/) template in the screenshots, but they did not share it publicly in this post.
