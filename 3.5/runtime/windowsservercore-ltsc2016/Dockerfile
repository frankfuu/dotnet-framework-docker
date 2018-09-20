# escape=`

FROM microsoft/windowsservercore:ltsc2016

# Install .NET Fx 3.5
RUN powershell -Command `
        $ErrorActionPreference = 'Stop'; `
        $ProgressPreference = 'SilentlyContinue'; `
        Invoke-WebRequest `
            -UseBasicParsing `
            -Uri "https://dotnetbinaries.blob.core.windows.net/dockerassets/microsoft-windows-netfx3-ltsc2016.zip" `
            -OutFile microsoft-windows-netfx3.zip; `
        Expand-Archive microsoft-windows-netfx3.zip; `
        Remove-Item -Force microsoft-windows-netfx3.zip; `
        Add-WindowsPackage -Online -PackagePath .\microsoft-windows-netfx3\microsoft-windows-netfx3-ondemand-package.cab; `
        Remove-Item -Force -Recurse microsoft-windows-netfx3

# Apply latest patch
RUN powershell -Command `
        $ErrorActionPreference = 'Stop'; `
        $ProgressPreference = 'SilentlyContinue'; `
        Invoke-WebRequest `
            -UseBasicParsing `
            -Uri "http://download.windowsupdate.com/d/msdownload/update/software/secu/2018/09/windows10.0-kb4457131-x64_a4b53b04e7398bd10fbfbb54df4ae6e10b877d22.msu" `
            -OutFile patch.msu; `
        New-Item -Type Directory patch; `
        Start-Process expand -ArgumentList 'patch.msu', 'patch', '-F:*' -NoNewWindow -Wait; `
        Remove-Item -Force patch.msu; `
        Add-WindowsPackage -Online -PackagePath C:\patch\Windows10.0-KB4457131-x64.cab; `
        Remove-Item -Force -Recurse \patch

# ngen .NET Fx
ENV COMPLUS_NGenProtectedProcess_FeatureEnabled 0
RUN \Windows\Microsoft.NET\Framework64\v4.0.30319\ngen update & `
    \Windows\Microsoft.NET\Framework\v4.0.30319\ngen update & `
    \Windows\Microsoft.NET\Framework64\v2.0.50727\ngen update & `
    \Windows\Microsoft.NET\Framework\v2.0.50727\ngen update