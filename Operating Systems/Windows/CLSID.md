# CLSID Keys

According to [Microsoft](https://msdn.microsoft.com/en-us/library/ms886195.aspx),

> A CLSID is a globally unique identifier that identifies a COM class object. If your server or container allows linking to its embedded objects, then you need to register a CLSID for each supported class of objects. The CLSID is stored under the following registry key:
>
> `HKEY_LOCAL_MACHINE\SOFTWARE\Classes\CLSID = <CLSID>`
>
> The CLSID value specifies a name that can be displayed in the user interface. The CLSID value contains information used by the default COM handler to return information about a class when it is in the running state. To obtain a CLSID for your application, you can use CoCreateGuid. The CLSID is a 128-bit number, spelled in hexadecimal format, within a pair of braces.

## Applet Commands

From the Windows Run dialog, you can launch a location using a CLSID by prefixing it with `shell:::`. For instance, to open the Control Panel on Windows 7, enter the following:

    shell:::{21EC2020-3AEA-1069-A2DD-08002B30309D}

To do this from the command prompt, you have to add `explorer` before it (to run from explorer):

    explorer shell:::{21EC2020-3AEA-1069-A2DD-08002B30309D}

**See also:**

 - [Sysnative "Shells, Shortcuts, and CLSID Listing (Windows 10, 8.1, 8, 7)" forum post](https://www.sysnative.com/forums/windows-8-windows-rt-tutorials/12157-shells-shortcuts-clsid-listing-windows-10-8-1-8-7-a.html)

  - [Geoff Chappell's "The Windows Explorer Command Line"](http://www.geoffchappell.com/studies/windows/shell/explorer/cmdline.htm)

## CLSID Lists

Lists of CLSIDs obtained from [Krypsec Digital Security "All CLSID For Windows"](http://krypsec.com/all-clsid-for-windows-to-used-in-ethical-hacking-and-batch-programming/).

### Main (at least up to Windows 7)

    CLSID_ControlPanel                {21EC2020-3AEA-1069-A2DD-08002B30309D}
    CLSID_Printers                    {2227A280-3AEA-1069-A2DE-08002B30309D}
    CLSID_MyDocuments                 {450D8FBA-AD25-11D0-98A8-0800361B1103}
    CLSID_MyComputer                  {20D04FE0-3AEA-1069-A2D8-08002B30309D}
    CLSID_NetworkPlaces               {208D2C60-3AEA-1069-A2D7-08002B30309D}
    CLSID_CompositeFolder             {FEF10DED-355E-4e06-9381-9B24D7F7CC88}
    CLSID_RecycleBin                  {645FF040-5081-101B-9F08-00AA002F954E}
    CLSID_Briefcase                   {85BBD920-42A0-1069-A2E4-08002B30309D}
    CLSID_BriefcaseFolder             {86747AC0-42A0-1069-A2E6-08002B30309D}
    CLSID_TaskScheduler               {D6277990-4C6A-11CF-8D87-00AA0060F5BF}
    CLSID_RecycleBinCleaner           {5ef4af3a-f726-11d0-b8a2-00c04fc309a4}
    CLSID_ShellViewerExt              {84F26EA0-42A0-1069-A2E3-08002B30309D}
    CLSID_ShellDrvDefExt              {5F5295E0-429F-1069-A2E2-08002B30309D}
    CLSID_ShellFSFolder               {F3364BA0-65B9-11CE-A9BA-00AA004AE837}
    CLSID_ShellFileDefExt             {21B22460-3AEA-1069-A2DC-08002B30309D}
    CLSID_FileTypes                   {B091E540-83E3-11CF-A713-0020AFD79762}
    CLSID_MIMEFileTypesHook           {FBF23B41-E3F0-101B-8488-00AA003E56F8}
    CLSID_DocFindFolder               {e17d4fc0-5564-11d1-83f2-00a0c90dc849}
    CLSID_ComputerFindFolder          {1f4de370-d627-11d1-ba4f-00a0c91eedba}
    CLSID_DocFindCommand              {B005E690-678D-11d1-B758-00A0C90564FE}
    CLSID_DocFindPersistHistory       {bab33640-1280-11d2-aa30-00a0c91eedba}
    CLSID_DefViewPersistHistory       {7febaf7c-18cf-11d2-993f-00a0c91f3880}
    CLSID_Internet                    {871C5380-42A0-1069-A2EA-08002B30309D}
    CLSID_ShellSearchExt              {169A0691-8DF9-11d1-A1C4-00C04FD75D13}
    CLSID_ShellTreeWalker             {95CE8412-7027-11D1-B879-006008059382}
    CLSID_DocObjColumnProvider        {24F14F01-7B1C-11d1-838f-0000F80461CF}
    CLSID_URLColumnProvider           {24F14F02-7B1C-11d1-838f-0000F80461CF}
    CLSID_FileSysColumnProvider       {0D2E74C4-3C34-11d2-A27E-00C04FC30871}
    CLSID_VersionColProvider          {66742402-F9B9-11D1-A202-0000F81FEDEE}
    CLSID_FileSearchBand              {C4EE31F3-4768-11D2-BE5C-00A0C9A83DA1}
    CLSID_ShellNetDefExt              {86422020-42A0-1069-A2E5-08002B30309D}
    CLSID_CFSIconOverlayManager       {63B51F81-C868-11D0-999C-00C04FD655E1}
    CLSID_CopyToMenu                  {C2FBB630-2971-11d1-A18C-00C04FD75D13}
    CLSID_MoveToMenu                  {C2FBB631-2971-11d1-A18C-00C04FD75D13}
    CLSID_CWebViewMimeFilter          {733AC4CB-F1A4-11d0-B951-00A0C90312E1}
    CLSID_DeskMovr                    {72267F6A-A6F9-11D0-BC94-00C04FB67863}
    CLSID_DeskHtmlProp                {3FC0B520-68A9-11D0-8D77-00C04FD70822}
    CLSID_ActiveDesktop               {75048700-EF1F-11D0-9888-006097DEACF9}
    CLSID_Shell                       {13709620-C279-11CE-A49E-444553540000}
    CLSID_WebViewFolderContents       {1820FED0-473E-11D0-A96C-00C04FD705A2}
    CLSID_MigrationWizardAuto         {67331D85-BE17-42f6-8D3F-47B8E8B26637}
    CLSID_ShellFolderView             {62112AA1-EBE4-11cf-A5FB-0020AFE7292D}
    CLSID_ShellFolderViewOC           {9BA05971-F6A8-11CF-A442-00A0C90A8F39}
    CLSID_Mshtml                      {25336920-03F9-11CF-8FD0-00AA00686F13}
    CLSID_ShellFldSetExt              {6D5313C0-8C62-11D1-B2CD-006097DF8C11}
    CLSID_StartMenu                   {4622AD11-FF23-11d0-8D34-00A0C90F2719}
    CLSID_PersonalStartMenu           {3F6953F0-5359-47FC-BD99-9F2CB95A62FD}
    CLSID_CmdFileIcon                 {57651662-CE3E-11D0-8D77-00C04FC99D61}
    CLSID_ShellDesktop                {00021400-0000-0000-C000-000000000046}
    CLSID_SendToMenu                  {7BA4C740-9E81-11CF-99D3-00AA004AE837}
    CLSID_NewMenu                     {D969A300-E7FF-11d0-A93B-00A0C90F2719}
    CLSID_OpenWithMenu                {09799AFB-AD67-11d1-ABCD-00C04FC30936}
    CLSID_OverlayIdentifier_SlowFile  {7D688A77-C613-11D0-999B-00C04FD655E1}
    CLSID_WebFolders                  {BDEADF00-C265-11D0-BCED-00A0C90AB50F}
    CLSID_Shell32TypeLib              {50a7e9b0-70ef-11d1-b75a-00a0c90564fe}
    CLSID_FolderShortcut              {0AFACED1-E828-11D1-9187-B532F1E9575D}
    CLSID_MountedVolume               {12518493-00B2-11d2-9FA5-9E3420524153}
    CLSID_MTAInjector                 {111DCCED-3B96-4170-A076-681669ED1512}
    CLSID_ShellLink                   {00021401-0000-0000-C000-000000000046}
    CLSID_NetworkConnections          {7007ACC7-3202-11D1-AAD2-00805FC1270E}
    CLSID_FontsFolderShortcut         {D20EA4E1-3957-11d2-A40B-0C5020524152}
    CLSID_AdminFolderShortcut         {D20EA4E1-3957-11d2-A40B-0C5020524153}
    CLSID_DragDropHelper              {4657278A-411B-11d2-839A-00C04FD918D0}
    CLSID_ExeDropTarget               {86C86720-42A0-1069-A2E8-08002B30309D}
    CLSID_CShellMonikerHelper         {679d9e37-f8f9-11d2-8deb-00c04f6837d5}
    CLSID_FolderOptions               {6DFD7C5C-2451-11d3-A299-00C04F8EF6AF}
    CLSID_TaskbarOptions              {0DF44EAA-FF21-4412-828E-260A8728E7F1}
    CLSID_FolderItem                  {FEF10FA2-355E-4e06-9381-9B24D7F7CC88}
    CLSID_FolderItemsFDF              {53C74826-AB99-4d33-ACA4-3117F51D3788}
    CLSID_ShellNetCrawler             {601ac3dc-786a-4eb0-bf40-ee3521e70bfb}

### Control Panel Options for Windows 8.1

    Power Option                      {025A5937-A6BE-4686-A844-36FE4BEC8B6D}
    Taskbar Notification Icon Control Panel   {05d7b0f4-2121-4eff-bf6b-ed3f69b894d9}
    Taskbar and Start menu            {0DF44EAA-FF21-4412-828E-260A8728E7F1}
    Credential Manager                {1206F5F1-0569-412C-8FEC-3204630DFB70}
    Set user Default                  {17cd9488-1228-4b2f-88ce-4298e93e0966}
    Workspaces Center                 {241D7C96-F8BF-4F85-B01F-E2B043341A4B}
    Window Update                     {36eef7db-88ad-4e81-ad49-0e313f0c35f8}
    View Available Network            {38A98528-6CBF-4CA9-8DC0-B1E1D10F7B1B}
    Window Firewall                   {4026492F-2F69-46B8-B9BF-5654FC07E423}
    Phone and Modem                   {40419485-C444-4567-851A-2DD7BFA1684D}
    Speech Recognition                {58E3C745-D971-4081-9034-86E34B30836A}
    Mobility Center                   {5ea4f148-308c-46d7-98a9-49041b1dd468}
    User Account                      {60632754-c523-4b62-b45c-4172da012619}
    Region and Language               {62D8ED13-C9D0-4CE8-A914-47DD628FB1B0}
    HomeGroup Control Panel           {67CA7650-96E6-4FDD-BB43-A8E774F73A57}
    Mouse                             {6C8EEC18-8D75-41B2-A177-8831D59D2D50}
    Folder Option                     {6DFD7C5C-2451-11d3-A299-00C04F8EF6AF}
    Keyboard                          {725BE8F7-668E-4C7B-8F90-46BDB0936430}
    Device Manager                    {74246bfc-4c96-11d0-abef-0020af6b0b7a}
    Programs and Features             {7b81be6a-ce2b-4676-a29e-eb907a5126c5}
    Tablet PC Setting                 {80F3F1D5-FECA-45F3-BC32-752C152E456E}
    Indexing Option                   {87D66A43-7B11-4A28-9811-C86EE395ACF7}
    Network and Sharing Center        {8E908FC9-BECC-40f6-915B-F4CA0E70D03D}
    Parental Control                  {96AE8D84-A250-4520-95A5-A47A7E3C548B}
    Autoplay                          {9C60DE1E-E5FC-40f4-A487-460851A8D915}
    Sync Center Folder                {9C73F5E5-7AE7-4E32-A8E8-8D23B85255BF}
    Recovery                          {9FE63AFD-59CF-4419-9775-ABCC3849F861}
    Infrared                          {A0275511-0E86-4ECA-97C2-ECD8F1221D08}
    Internet Option                   {A3DD4F92-658A-410F-84FD-6FBBBEF2FFFE}
    Device Center                     {A8A91A66-3A7D-4424-8D24-04E180695C7A}
    Color Management                  {B2C761C6-29BC-4f19-9251-E6195265BAF1}
    System                            {BB06C0E4-D293-4f75-8A90-CB05B6477EEE}
    Action Center CPL                 {BB64F8A7-BEE7-4E1A-AB8D-7D8273F7FDB6}
    Font Folder                       {BD84B380-8CA2-1069-AB1D-08000948F534}
    Language Setting                  {BF782CC9-5A52-4A17-806C-2A894FFEEAC5}
    Display                           {C555438B-3C23-4769-A71F-B6D3D9B6053A}
    Troubleshooting                   {C58C4893-3BE0-4B45-ABB5-A63E4B8C8651}
    Text to Speech                    {D17D1D6D-CC3F-4815-8FE3-607E7D5D10B3}
    Administrative tool               {D20EA4E1-3957-11d2-A40B-0C5020524153}
    Ease of Access                    {D555645E-D4F8-4c29-A827-D93C859C4F2A}
    WIndows Defender                  {D8559EB9-20C0-410E-BEDA-7ED416AECC2A}
    Secure Startup                    {D9EF8727-CAC2-4e60-809E-86F80A666C91}
    Date and Time                     {D9EF8727-CAC2-4e60-809E-86F80A666C91}
    Sensor                            {E9950154-C418-419e-A90A-20C5287AE24B}
    ECS                               {ECDB0924-4208-451E-8EE0-373C0956DE16}
    Personalization                   {ED834ED6-4B5A-4bfe-8F11-A626DCB6A921}
    Sound                             {F2DDFC82-8F12-4CDD-B7DC-D4FE1425AA4D}
    History Vault                     {F6B6E965-E9B2-444B-9286-10C9152EDBC5}
    Pen and Touch                     {F82DF8F7-8B9F-442E-A48C-818EA735FF9B}
    Storage Spaces                    {F942C606-0914-47AB-BE56-1321B8035096}
    Programs Folder                   {7be9d83c-a729-4d97-b5a7-1b7313c39e0a}
    Programs Folder and Fast Items    {865e5e76-ad83-4dca-a109-50dc2113ce9a}
    Public Folder                     {4336a54d-038b-4685-ab02-99bb52d3fb8b}
    Recent Places                     {22877a6d-37a1-461a-91b0-dbda5aaebc99}
    Recovery                          {9FE63AFD-59CF-4419-9775-ABCC3849F861}
    Recycle Bin                       {645FF040-5081-101B-9F08-00AA002F954E}
    Region and Language               {62d8ed13-c9d0-4ce8-a914-47dd628fb1b0}
    RemoteApp and Desktop Connections {241D7C96-F8BF-4F85-B01F-E2B043341A4B}
    Run                               {2559a1f3-21d7-11d4-bdaf-00c04f60b9f0}
    Download Folder                   {374DE290-123F-4565-9164-39C4925E467B}
    Ease of Access Center             {D555645E-D4F8-4c29-A827-D93C859C4F2A}
    E-mail (default program)          {2559a1f5-21d7-11d4-bdaf-00c04f60b9f0}
    Control Panel (All Settings)      {F90C627B-7280-45DB-BC26-CCE7BDD620A4}
    Applications                      {4234d49b-0245-4df3-b780-3893943456e1}
    AutoPlay                          {9C60DE1E-E5FC-40f4-A487-460851A8D915}
    Biometric Devices (Win 8 only)    {0142e4d0-fb7a-11dc-ba4a-000ffe7ab428}
    BitLocker Drive Encryption        {D9EF8727-CAC2-4e60-809E-86F80A666C91}
    Bluetooth Devices                 {28803F59-3A75-4058-995F-4EE5503B023C}
    Briefcase                         {85BBD920-42AO-1069-A2E4-08002B30309D}
    Color Management                  {B2C761C6-29BC-4f19-9251-E6195265BAF1}
    Command Folder                    {437ff9c0-a07f-4fa0-af80-84b6c6440a16}
    Common Places FS Folder           {d34a6ca6-62c2-4c34-8a7c-14709c1ad938}
    Computer (This PC)                {20d04fe0-3aea-1069-a2d8-08002b30309d}
    Connect To                        {38A98528-6CBF-4CA9-8DC0-B1E1D10F7B1B}
    Control Panel                     {5399E694-6CE5-4D6C-8FCE-1D8870FDCBA0}
