# Windows File Times

**What is a file time?** A 64-bit timestamp that stores the number of 100-nanosecond intervals since midnight on January 1, 1601 UTC.

Digital forensics practitioners need to be careful when making assertions about events on the basis of file times (this is true of all timestamps). We are dealing with an *approximation* of an event when describing activity and providing a file time. Granted, that approximation is often highly accurate; but according to Microsoft,

> Time stamps are updated at various times and for various reasons. The only guarantee about a file time stamp is that the file time is correctly reflected when the handle that makes the change is closed....
>
> The NTFS file system delays updates to the last access time for a file by up to 1 hour after the last access.

SOURCE: Microsoft, “File Times”, Windows Dev Center. Retrieved on August 21, 2017 from https://msdn.microsoft.com/en-us/library/windows/desktop/ms724290.

## File System Tunneling

**What is file system tunneling?** The Windows operating system caches file system metadata (FILE records for NTFS and directory entries for FAT) for a short time period after a file is deleted or renamed and will reuse/restore this metadata when a new file with that name is created/renamed shortly after.

According to [Microsoft](https://support.microsoft.com/en-us/help/172190/windows-nt-contains-file-system-tunneling-capabilities),

> The idea is to mimic the behavior MS-DOS programs expect when they use the safe save method. They copy the modified data to a temporary file, delete the original and rename the temporary to the original. This should seem to be the original file when complete. Windows performs tunneling on both FAT and NTFS file systems to ensure long/short file names are retained when 16-bit applications perform this safe save operation.

The file tunneling settings are controlled by the Windows Registry key `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\FileSystem`.

 - To increase the tunneling cache time, modify/add the DWORD value `MaximumTunnelEntryAgeInSeconds`. The default cache time is 15 seconds.

 - To disable tunneling, modify/add the DWORD value `MaximumTunnelEntries` and set it to `0`.

SOURCE: Microsoft, “Windows NT Contains File System Tunneling Capabilities”, KB Article 172190, Rev. 1 (October 23, 2008). Retrieved August 21, 2017 from https://support.microsoft.com/en-us/help/172190/windows-nt-contains-file-system-tunneling-capabilities.

**What is the purpose of file tunneling?** According to [Raymond Chen](https://blogs.msdn.microsoft.com/oldnewthing/20050715-14/?p=34923/),

> When you use a program to edit an existing file, then save it, you expect the original creation timestamp to be preserved, since you're editing a file, not creating a new one. But internally, many programs save a file by performing a combination of save, delete, and rename operations…, and without tunneling, the creation time of the file would seem to change even though from the end user's point of view, no file got created.
>
> As another example of the importance of tunneling, consider that file “`File with long name.txt`”, whose short name is say "`FILEWI~1.TXT`". You load this file into a program that is not long-filename-aware and save it. It deletes the old "`FILEWI~1.TXT`" and creates a new one with the same name. Without tunnelling, the associated long name of the file would be lost. Instead of a friendly long name, the file name got corrupted into this thing with squiggly marks. Not good.

SOURCE: Raymond Chen, “The apocryphal history of file system tunnelling”, Microsoft Developer Blog: The Old New Thing (July 15, 2005). Retrieved August 21, 2017 from https://blogs.msdn.microsoft.com/oldnewthing/20050715-14/?p=34923/.

SEE ALSO:

 - [My blog post on this](https://medium.com/@4n68r/some-reminders-about-windows-file-times-2debe1edb978)

 - [Important articles about Windows/NTFS (my Gist)](https://gist.github.com/danzek/624d5c3aaa349f0e846281c75c2ec371)

 - [Harlan Carvey's blog post](http://windowsir.blogspot.com/2014/07/file-system-ops-effects-on-mft-records.html) where he does testing of file tunneling behavior (and he links to several other blog posts that do also).
