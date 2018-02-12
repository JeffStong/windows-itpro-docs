---
title: Automatic and customized exclusions for Windows Defender AV on Windows Server 2016
description: Windows Server 2016 includes automatic exclusions, based on Server Role. You can also add custom exclusions.
keywords: exclusions, server, auto-exclusions, automatic, custom, scans
search.product: eADQiWindows 10XVcnh
ms.pagetype: security
ms.prod: w10
ms.mktglfcycl: manage
ms.sitesec: library
ms.pagetype: security
ms.localizationpriority: medium
author: iaanw
ms.author: iawilt
ms.date: 07/27/2017
---

# Configure exclusions in Windows Defender AV on Windows Server


**Applies to:**

- Windows Server 2016

**Audience**

- Enterprise security administrators


**Manageability available with**

- Group Policy
- PowerShell
- Windows Management Instrumentation (WMI)

If you are using Windows Defender Antivirus to protect Windows Server 2016 machines, you are automatically enrolled in certain exclusions, as defined by your specified Windows Server Role. A list of these exclusions is provided at [the end of this topic](#list-of-automatic-exclusions).

These exclusions will not appear in the standard exclusion lists shown in the [Windows Defender Security Center app](windows-defender-security-center-antivirus.md#exclusions).

You can still add or remove custom exclusions (in addition to the Server Role-defined automatic exclusions) as described in the other exclusion-related topics:
- [Configure and validate exclusions based on file name, extension, and folder location](configure-extension-file-exclusions-windows-defender-antivirus.md)
- [Configure and validate exclusions for files opened by processes](configure-process-opened-file-exclusions-windows-defender-antivirus.md)

Custom exclusions take precedence over the automatic exclusions.

> [!TIP]
> Custom and duplicate exclusions do not conflict with automatic exclusions.

Windows Defender AV uses the Deployment Image Servicing and Management (DSIM) tools to determine which roles are installed on your computer.


## Opt out of automatic exclusions

In Windows Server 2016 the predefined exclusions delivered by definition updates only exclude the default paths for a role or feature. If you installed a role or feature in a custom path, or you want to manually control the set of exclusions, you need to opt-out of the automatic exclusions delivered in definition updates.

> [!WARNING]
> Opting out of automatic exclusions may adversely impact performance, or result in data corruption. The exclusions that are delivered automatically are optimized for Windows Server 2016 roles.

You can disable the auto-exclusions lists with Group Policy, PowerShell cmdlets, and WMI.

**Use Group Policy to disable the auto-exclusions list on Windows Server 2016:**

1.  On your Group Policy management machine, open the [Group Policy Management Console](https://technet.microsoft.com/library/cc731212.aspx), right-click the Group Policy Object you want to configure and click **Edit**.

3.  In the **Group Policy Management Editor** go to **Computer configuration**.

4.  Click **Policies** then **Administrative templates**.

5.  Expand the tree to **Windows components > Windows Defender Antivirus > Exclusions**.

6. Double-click the **Turn off Auto Exclusions** setting and set the option to **Enabled**. Click **OK**. 

**Use PowerShell cmdlets to disable the auto-exclusions list on Windows Server 2016:**

Use the following cmdlets:

```PowerShell
Set-MpPreference -DisableAutoExclusions $true
```

See [Use PowerShell cmdlets to configure and run Windows Defender Antivirus](use-powershell-cmdlets-windows-defender-antivirus.md) and [Defender cmdlets](https://technet.microsoft.com/itpro/powershell/windows/defender/index) for more information on how to use PowerShell with Windows Defender Antivirus.

**Use Windows Management Instruction (WMI) to disable the auto-exclusions list on Windows Server 2016:**

Use the [**Set** method of the **MSFT_MpPreference**](https://msdn.microsoft.com/en-us/library/dn455323(v=vs.85).aspx) class for the following properties:

```WMI
DisableAutoExclusions
```

See the following for more information and allowed parameters:
- [Windows Defender WMIv2 APIs](https://msdn.microsoft.com/en-us/library/dn439477(v=vs.85).aspx)





## List of automatic exclusions
The following sections contain the exclusions that are delivered with automatic exclusions file paths and file types.

### Default exclusions for all roles
This section lists the default exclusions for all Windows Server 2016 roles.

-   Windows "temp.edb" files:

    -   *%windir%*\SoftwareDistribution\Datastore\\*\tmp.edb

    -   *%ProgramData%*\Microsoft\Search\Data\Applications\Windows\\*\\\*.log

-   Windows Update files or Automatic Update files:

    -   *%windir%*\SoftwareDistribution\Datastore\\*\Datastore.edb

    -   *%windir%*\SoftwareDistribution\Datastore\\*\edb.chk

    -   *%windir%*\SoftwareDistribution\Datastore\\*\edb\*.log

    -   *%windir%*\SoftwareDistribution\Datastore\\*\Edb\*.jrs

    -   *%windir%*\SoftwareDistribution\Datastore\\*\Res\*.log

-   Windows Security files:

    -   *%windir%*\Security\database\\*.chk

    -   *%windir%*\Security\database\\*.edb

    -   *%windir%*\Security\database\\*.jrs

    -   *%windir%*\Security\database\\*.log

    -   *%windir%*\Security\database\\*.sdb

-   Group Policy files:

    -   *%allusersprofile%*\NTUser.pol

    -   *%SystemRoot%*\System32\GroupPolicy\Machine\registry.pol

    -   *%SystemRoot%*\System32\GroupPolicy\User\registry.pol

-   WINS files:

    -   *%systemroot%*\System32\Wins\\*\\\*.chk

    -   *%systemroot%*\System32\Wins\\*\\\*.log

    -   *%systemroot%*\System32\Wins\\*\\\*.mdb

    -   *%systemroot%*\System32\LogFiles\

    -   *%systemroot%*\SysWow64\LogFiles\

-   File Replication Service (FRS) exclusions:

    -   Files in the File Replication Service (FRS) working folder. The FRS working folder is specified in the registry key `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\NtFrs\Parameters\Working Directory`

        -   *%windir%*\Ntfrs\jet\sys\\*\edb.chk

        -   *%windir%*\Ntfrs\jet\\*\Ntfrs.jdb

        -   *%windir%*\Ntfrs\jet\log\\*\\\*.log

    -   FRS Database log files. The FRS Database log file folder is specified in the registry key `HKEY_LOCAL_MACHINE\System\Currentcontrolset\Services\Ntfrs\Parameters\DB Log File Directory`

        -   *%windir%*\Ntfrs\\*\Edb\*.log

    -   The FRS staging folder. The staging folder is specified in the registry key `HKEY_LOCAL_MACHINE\System\Currentcontrolset\Services\NtFrs\Parameters\Replica Sets\GUID\Replica Set Stage`

        -   *%systemroot%*\Sysvol\\*\Nntfrs_cmp\*\

    -   The FRS preinstall folder. This folder is specified by the folder `Replica_root\DO_NOT_REMOVE_NtFrs_PreInstall_Directory`

        -   *%systemroot%*\SYSVOL\domain\DO_NOT_REMOVE_NtFrs_PreInstall_Directory\\*\Ntfrs\*\

    -   The Distributed File System Replication (DFSR) database and working folders. These folders are specified by the registry key `HKEY_LOCAL_MACHINE\System\Currentcontrolset\Services\DFSR\Parameters\Replication Groups\GUID\Replica Set Configuration File`

        -   *%systemdrive%*\System Volume Information\DFSR\\$db_normal$

        -   *%systemdrive%*\System Volume Information\DFSR\FileIDTable_*

        -   *%systemdrive%*\System Volume Information\DFSR\SimilarityTable_*

        -   *%systemdrive%*\System Volume Information\DFSR\\*.XML

        -   *%systemdrive%*\System Volume Information\DFSR\\$db_dirty$

        -   *%systemdrive%*\System Volume Information\DFSR\\$db_clean$

        -   *%systemdrive%*\System Volume Information\DFSR\\$db_lostl$

        -   *%systemdrive%*\System Volume Information\DFSR\Dfsr.db

        -   *%systemdrive%*\System Volume Information\DFSR\\*.frx

        -   *%systemdrive%*\System Volume Information\DFSR\\*.log

        -   *%systemdrive%*\System Volume Information\DFSR\Fsr*.jrs

        -   *%systemdrive%*\System Volume Information\DFSR\Tmp.edb

-   Process exclusions

    -   *%systemroot%*\System32\dfsr.exe

    -   *%systemroot%*\System32\dfsrs.exe

-   Hyper-V exclusions:

    -   This section lists the file type exclusions, folder exclusions, and process exclusions that are delivered automatically when you install the Hyper-V role

        -   File type exclusions:

            -   *.vhd

            -   *.vhdx

            -   *.avhd

            -   *.avhdx

            -   *.vsv

            -   *.iso

            -   *.rct

            -   *.vmcx

            -   *.vmrs

        -   Folder exclusions:

            -   *%ProgramData%*\Microsoft\Windows\Hyper-V

            -   *%ProgramFiles%*\Hyper-V

            -   *%SystemDrive%*\ProgramData\Microsoft\Windows\Hyper-V\Snapshots

            -   *%Public%*\Documents\Hyper-V\Virtual Hard Disks

        -   Process exclusions:

            -   *%systemroot%*\System32\Vmms.exe

            -   *%systemroot%*\System32\Vmwp.exe

-   SYSVOL files:

    -   *%systemroot%*\Sysvol\Domain\\*.adm

    -   *%systemroot%*\Sysvol\Domain\\*.admx

    -   *%systemroot%*\Sysvol\Domain\\*.adml

    -   *%systemroot%*\Sysvol\Domain\Registry.pol

    -   *%systemroot%*\Sysvol\Domain\\*.aas

    -   *%systemroot%*\Sysvol\Domain\\*.inf

    -   *%systemroot%*\Sysvol\Domain\\*.Scripts.ini

    -   *%systemroot%*\Sysvol\Domain\\*.ins

    -   *%systemroot%*\Sysvol\Domain\Oscfilter.ini

### Active Directory exclusions
This section lists the exclusions that are delivered automatically when you install Active Directory Domain Services.

-   NTDS database files. The database files are specified in the registry key `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\NTDS\Parameters\DSA Database File`

    -   %windir%\Ntds\ntds.dit

    -   %windir%\Ntds\ntds.pat

-   The AD DS transaction log files. The transaction log files are specified in the registry key `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\NTDS\Parameters\Database Log Files`

    -   %windir%\Ntds\EDB*.log

    -   %windir%\Ntds\Res*.log

    -   %windir%\Ntds\Edb*.jrs

    -   %windir%\Ntds\Ntds*.pat

    -   %windir%\Ntds\EDB*.log

    -   %windir%\Ntds\TEMP.edb

-   The NTDS working folder. This folder is specified in the registry key `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\NTDS\Parameters\DSA Working Directory`

    -   %windir%\Ntds\Temp.edb

    -   %windir%\Ntds\Edb.chk

-   Process exclusions for AD DS and AD DS-related support files:

    -   %systemroot%\System32\ntfrs.exe

    -   %systemroot%\System32\lsass.exe

### DHCP Server exclusions
This section lists the exclusions that are delivered automatically when you install the DHCP Server role. The DHCP Server file locations are specified by the *DatabasePath*, *DhcpLogFilePath*, and *BackupDatabasePath* parameters in the registry key `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\DHCPServer\Parameters`

-   *%systemroot%*\System32\DHCP\\*\\\*.mdb

-   *%systemroot%*\System32\DHCP\\*\\\*.pat

-   *%systemroot%*\System32\DHCP\\*\\\*.log

-   *%systemroot%*\System32\DHCP\\*\\\*.chk

-   *%systemroot%*\System32\DHCP\\*\\\*.edb

### DNS Server exclusions
This section lists the file and folder exclusions and the process exclusions that are delivered automatically when you install the DNS Server role.

-   File and folder exclusions for the DNS Server role:

    -   *%systemroot%*\System32\Dns\\*\\\*.log

    -   *%systemroot%*\System32\Dns\\*\\\*.dns

    -   *%systemroot%*\System32\Dns\\*\\\*.scc

    -   *%systemroot%*\System32\Dns\\*\BOOT

-   Process exclusions for the DNS Server role:

    -   *%systemroot%*\System32\dns.exe

    

### File and Storage Services exclusions
This section lists the file and folder exclusions that are delivered automatically when you install the File and Storage Services role. The exclusions listed below do not include exclusions for the Clustering role.

-   *%SystemDrive%*\ClusterStorage

-   *%clusterserviceaccount%*\Local Settings\Temp

-   *%SystemDrive%*\mscs

### Print Server exclusions
This section lists the file type exclusions, folder exclusions, and the process exclusions that are delivered automatically when you install the Print Server role.

-   File type exclusions:

    -   *.shd

    -   *.spl

-   Folder exclusions. This folder is specified in the registry key `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Print\Printers\DefaultSpoolDirectory`

    -   *%system32%*\spool\printers\\*

-   Process exclusions:

    -   spoolsv.exe

### Web Server exclusions
This section lists the folder exclusions and the process exclusions that are delivered automatically when you install the Web Server role.

-   Folder exclusions:

    -   *%SystemRoot%*\IIS Temporary Compressed Files

    -   *%SystemDrive%*\inetpub\temp\IIS Temporary Compressed Files

    -   *%SystemDrive%*\inetpub\temp\ASP Compiled Templates

    -   *%systemDrive%*\inetpub\logs

    -   *%systemDrive%*\inetpub\wwwroot

-   Process exclusions:

    -   *%SystemRoot%*\system32\inetsrv\w3wp.exe

    -   *%SystemRoot%*\SysWOW64\inetsrv\w3wp.exe

    -   *%SystemDrive%*\PHP5433\php-cgi.exe

### Windows Server Update Services exclusions
This section lists the folder exclusions that are delivered automatically when you install the Windows Server Update Services (WSUS) role. The WSUS folder is specified in the registry key `HKEY_LOCAL_MACHINE\Software\Microsoft\Update Services\Server\Setup`

-   *%systemroot%*\WSUS\WSUSContent

-   *%systemroot%*\WSUS\UpdateServicesDBFiles

-   *%systemroot%*\SoftwareDistribution\Datastore

-   *%systemroot%*\SoftwareDistribution\Download




## Related topics

- [Configure and validate exclusions for Windows Defender AV scans](configure-exclusions-windows-defender-antivirus.md)
- [Configure and validate exclusions based on file name, extension, and folder location](configure-extension-file-exclusions-windows-defender-antivirus.md)
- [Configure and validate exclusions for files opened by processes](configure-process-opened-file-exclusions-windows-defender-antivirus.md)
- [Customize, initiate, and review the results of Windows Defender AV scans and remediation](customize-run-review-remediate-scans-windows-defender-antivirus.md)
- [Windows Defender Antivirus in Windows 10](windows-defender-antivirus-in-windows-10.md)