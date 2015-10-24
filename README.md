EVhdParser
==============
This is a reverse engeneered Microsoft Vhdparser driver, which is originally used as a shim
between storvsp (Storage Virtualization Service Provider) and vhdmp (VHD miniport driver).
This shim enables us to write own filter drivers for virtual storages in Hyper-V
(i.e. encrypting drivers).

Hyper-V clients (VMs) communicate with storvsp using SCSI protocol, this driver is
used to filter I/O requests and pass it to the backing store. The whole architecture is
briefly overviewed at
http://download.microsoft.com/download/5/b/9/5b97017b-e28a-4bae-ba48-174cf47d23cd/vir049_wh06.ppt

It seems that Microsoft had planned to implement an expandability of virtual storage services
(as it is done in Extensible Hyper-V switch) and to enable third-party SAN developers
to write their own enlightened I/O parser for their solutions, but unfortunatelly StorVSP API
has not become public.

Caution
--------------
The purpose of this project is purely investigative (i.e. not meant to be used in production).

Currently only this OS versions are supported:
- Windows Server 2012 (only replaces original driver)
- Windows Server 2012 R2 (replaces original driver and allows to encrypt on the fly specific virtual disks)

Installation
--------------
1. Install this driver as a kernel mode service:

> sc create evhdparser binPath=[path to evhdparser.sys] type=kernel

2. Register this service as a default parser by replacing registry value

>HKLM\CurrentControlSet\Control\StorVSP\Parsers\{f916c826-f0f5-4cd9-be68-4fd638cf9a53}\ServiceName

from *vhdparser* to *evhdparser* (may require to take registry key ownership)
