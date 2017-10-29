+++
date = 2017-10-29
draft = false
tags = ["hugo"]
title = "Hugo on Windows"
math = true
+++

Abstract: An introduction on how to install and use hugo on Windows

{{% toc %}}

## Install Hugo on Windows

Install Hugo on macOS, Windows, Linux, FreeBSD, and on any machine where the Go compiler tool chain can run.
The details of install is listed here: https://gohugo.io/getting-started/installing/#quick-install

### Install Hugo on Windows by **Chocolatey**

If you are on a Windows machine and use [Chocolatey](https://chocolatey.org/) for package management, you can install Hugo with the following one-liner: `install-with-chocolatey.ps1`

1. Install [Chocolatey](https://chocolatey.org/) for package management

> The document of Chocolatey is: https://chocolatey.org/install

- Install with **cmd.exe**. Run the following command:

```
@"%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe" -NoProfile -InputFormat None -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"
```

- Install with **PowerShell.exe**, see document of [Chocolatey installation](https://chocolatey.org/install).

### Install Hugo with Chocolaey

Just open your cmd.exe and type or copy 

```
choco install hugo -confirm
```

## Use Hugo on Windows

1. Change your directory to your hugo project
2. `hugo server` to local test.
3. `hugo` to generate the public.