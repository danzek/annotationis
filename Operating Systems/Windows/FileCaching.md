# File Caching (a.k.a. "Lazy Write")

According to [Microsoft,](https://msdn.microsoft.com/en-us/library/windows/desktop/aa364218(v=vs.85).aspx)

> By default, Windows caches file data that is read from disks and written to disks. This implies that read operations read file data from an area in system memory known as the system file cache, rather than from the physical disk. Correspondingly, write operations write file data to the system file cache rather than to the disk, and this type of cache is referred to as a write-back cache. Caching is managed per file object.
>
> Caching occurs under the direction of the *cache manager*, which operates continuously while Windows is running. File data in the system file cache is written to the disk at intervals determined by the operating system, and the memory previously used by that file data is freed—this is referred to as *flushing* the cache. The policy of delaying the writing of the data to the file and holding it in the cache until the cache is flushed is called lazy writing, and it is triggered by the cache manager at a determinate time interval. The time at which a block of file data is flushed is partially based on the amount of time it has been stored in the cache and the amount of time since the data was last accessed in a read operation. This ensures that file data that is frequently read will stay accessible in the system file cache for the maximum amount of time....
> 
> The frequency at which flushing occurs is an important consideration that balances system performance with system reliability. If the system flushes the cache too often, the number of large write operations flushing incurs will degrade system performance significantly. If the system is not flushed often enough, then the likelihood is greater that either system memory will be depleted by the cache, or a sudden system failure (such as a loss of power to the computer) will happen before the flush. In the latter instance, the cached data will be lost.
>
> To ensure that the right amount of flushing occurs, the cache manager spawns a process every second called a lazy writer. The lazy writer process queues one-eighth of the pages that have not been flushed recently to be written to disk. It constantly reevaluates the amount of data being flushed for optimal system performance, and if more data needs to be written it queues more data. Lazy writers do not flush temporary files, because the assumption is that they will be deleted by the application or system.
>
> Some applications, such as virus-checking software, require that their write operations be flushed to disk immediately; Windows provides this ability through write-through caching. A process enables write-through caching for a specific I/O operation by passing the `FILE_FLAG_WRITE_THROUGH` flag into its call to [`CreateFile`](https://msdn.microsoft.com/en-us/library/windows/desktop/aa363858(v=vs.85).aspx). With write-through caching enabled,  data is still written into the cache, but the cache manager writes the data  immediately to  disk rather than incurring a delay by using the lazy writer. A process can also force a flush of a file it has opened by calling the [`FlushFileBuffers`](https://msdn.microsoft.com/en-us/library/windows/desktop/aa364439(v=vs.85).aspx) function.
>
> File system metadata is always cached. Therefore, to store any metadata changes to disk, the file must either be flushed or be opened with `FILE_FLAG_WRITE_THROUGH`.

Sometimes this process can go awry, in which case the System event log will record an event ID 50 error message. As described by [Microsoft,](https://support.microsoft.com/en-us/help/816004/description-of-the-event-id-50-error-message)

> An event ID 50 message is logged if a generic error occurs when Windows is trying to write information to the disk. This error occurs when Windows is trying to commit data from the file system Cache Manager (not hardware level cache) to the physical disk. This behavior is part of the memory management of Windows. For example, if a program sends a write request, the write request is cached by Cache Manager and the program is told the write is completed successfully. At a later point in time, Cache Manager tries to lazy write the data to the physical disk. When Cache Manager tries to commit the data to disk, an error occurs writing the data, and the data is flushed from the cache and discarded. Write-back caching improves system performance, but data loss and volume integrity loss can occur as a result of lost delayed-write failures....
>
> There are several different sources for an event ID 50 message. For example, an event ID 50 message logged from a MRxSmb source occurs if there is a network connectivity problem with the redirector. To avoid performing incorrect troubleshooting steps, make sure to review the event ID 50 message to confirm that it refers to a disk I/O issue and that this article applies.

## Resources

 - MSDN, ["File Caching"](https://msdn.microsoft.com/en-us/library/windows/desktop/aa364218(v=vs.85).aspx), *Microsoft Windows Dev Center*

 - Microsoft, ["Description of the Event ID 50 Error Message"](https://support.microsoft.com/en-us/help/816004/description-of-the-event-id-50-error-message), *Microsoft Support*

 - Ramazan Can, ["Event ID 50 – NTFS Delayed Write Lost"](https://blogs.technet.microsoft.com/ramazancan/2017/10/17/event-id-50-ntfs-delayed-write-lost/), *Microsoft TechNet Heartbeat from the Datacenter*
