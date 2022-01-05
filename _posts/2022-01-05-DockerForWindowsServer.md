---
layout: post
author: ledinhtrunghieu
title: Docker for Windows Server
---


# 1. Install Docker

**Source:**

https://4sysops.com/archives/install-docker-on-windows-server-2019/

https://www.virtualizationhowto.com/2020/12/install-docker-in-windows-server-2019/

https://docs.microsoft.com/en-us/virtualization/windowscontainers/quick-start/set-up-environment?tabs=Windows-Server

**Those links have the same steps:**

## 1.1. Enable Hyper-V and Container (Complete)

```bash
Install-WindowsFeature -Name Hyper-V -IncludeManagementTools -Restart
Install-WindowsFeature containers -Restart
```

<img src="/assets/images/20220105_DockerWinServer/pic1.png" class="largepic"/>

<img src="/assets/images/20220105_DockerWinServer/pic2.png" class="largepic"/>

## 1.2. Install DockerMsftProvider and latest Docker version 

```bash
Install-Module -Name DockerMsftProvider -Repository PSGallery -Force
Install-Package -Name docker -ProviderName DockerMsftProvider
Restart-Computer -force
```

<img src="/assets/images/20220105_DockerWinServer/pic3.png" class="largepic"/>


Counter issues can not install NuGet

<img src="/assets/images/20220105_DockerWinServer/pic4.png" class="largepic"/>


# 2. Fix Nuget issues

**Source:**

Links below also have the same steps:

https://www.alitajran.com/unable-to-install-nuget-provider-for-powershell/

https://answers.microsoft.com/en-us/windows/forum/all/trying-to-install-program-using-powershell-and/4c3ac2b2-ebd4-4b2a-a673-e283827da143

https://stackoverflow.com/questions/55826791/powershell-installing-nuget-says-unable-to-access-internet-but-i-actually-can

https://stackoverflow.com/questions/43323123/warning-unable-to-find-module-repositories

https://stackoverflow.com/questions/16657778/install-nuget-via-powershell-script/26421187



## 2.1. Check Powershell version is 5.0 or higher (Complete)

```bash
Get-Host | Select-Object Version
```
<img src="/assets/images/20220105_DockerWinServer/pic5.png" class="largepic"/>


Power version already 5.0

## 2.2 Update TLS versions (Complete)

```
[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.SecurityProtocolType]::Tls12;
```

Restart the Powershell and check whether the desired effect is achieved: 

```
[Net.ServicePointManager]::SecurityProtocol
```

<img src="/assets/images/20220105_DockerWinServer/pic6.png" class="largepic"/>
 
Already Tls, Tls11, Tls12, etc.

## 2.3. Install NuGet 
```
Install NuGet: Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force
```

<img src="/assets/images/20220105_DockerWinServer/pic7.png" class="largepic"/>

or trigger Install from other comments:

```bash
Install-Module PowershellGet -Force
```

<img src="/assets/images/20220105_DockerWinServer/pic8.png" class="largepic"/>
 
Still not work

# 3. Addtional steps:

## 3.1. https://stackoverflow.com/questions/16657778/install-nuget-via-powershell-script/26421187

```
$sourceNugetExe = "https://dist.nuget.org/win-x86-commandline/latest/nuget.exe"
$targetNugetExe = "$rootPath\nuget.exe"
Invoke-WebRequest $sourceNugetExe -OutFile $targetNugetExe
Set-Alias nuget $targetNugetExe -Scope Global -Verbose
```
(Not Work)

```
Invoke-WebRequest https://dist.nuget.org/win-x86-commandline/latest/nuget.exe -OutFile Nuget.exe
```
(Not Work)
```
Set-ItemProperty -Path 'HKLM:\SOFTWARE\Wow6432Node\Microsoft\.NetFramework\v4.0.30319' -Name 'SchUseStrongCrypto' -Value '1' -Type DWord
```
(Not Work)

## 3.2. https://answers.microsoft.com/en-us/windows/forum/all/trying-to-install-program-using-powershell-and/4c3ac2b2-ebd4-4b2a-a673-e283827da143
```
Set-ItemProperty -Path 'HKLM:\SOFTWARE\Wow6432Node\Microsoft\.NetFramework\v4.0.30319' -Name 'SchUseStrongCrypto' -Value '1' -Type DWord
Set-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\.NetFramework\v4.0.30319' -Name 'SchUseStrongCrypto' -Value '1' -Type DWord
```
(Not Work)

## 3.3. https://stackoverflow.com/questions/55826791/powershell-installing-nuget-says-unable-to-access-internet-but-i-actually-can
```
[System.Net.WebRequest]::DefaultWebProxy.Credentials = System.Net.CredentialCache]::DefaultCredentials
```
( Not Work )

## 3.4. https://stackoverflow.com/questions/43323123/warning-unable-to-find-module-repositories
```
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12 
Install-Module PowerShellGet -RequiredVersion 2.2.4 -SkipPublisherCheck
[Net.ServicePointManager]::SecurityProtocol = [Net.ServicePointManager]::SecurityProtocol -bor [Net.SecurityProtocolType]::Tls12
Register-PSRepository -Default -Verbose
Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted
```
(Not Work)


https://stackoverflow.com/questions/45638302/error-trying-to-install-docker-in-windows-server-2016-with-install-module

[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
 
set-executionpolicy unrestricted
 
Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force

Install-Module -Name DockerMsftProvider -Force

Install-Package -Name docker -ProviderName DockerMsftProvider -Force
Restart-Computer -Forc