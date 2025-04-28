# Github Actions fitting to Obtainium demo

- Set "Settings" -> "Actions" -> "General" -> "Workflow permissions" to "Read and write permissions"
- Add an git tag fiting the pattern 'v*.*.*', that will trigger a release build
- Minimal major version is '1'
- Install [Obtainium](https://github.com/ImranR98/Obtainium?tab=readme-ov-file#installation)
- Add "https://github.com/stesee/NetMauiObtainiumHelloWorld" to Obtainium.
- Use obtainium://github.com/stesee/NetMauiObtainiumHelloWorld on websites to outomate the last step, e.g. [![Obtainium](https://img.shields.io/badge/Obtainium%20Store-NetMauiObtainiumHelloWorld-green?style=flat&logo=android)](https://codeuctivity.github.io/obtainium/redirect?r=obtainium://add/https://github.com/stesee/NetMauiObtainiumHelloWorld)

## Development

### Reuse notice - forks

You will need to overwrite the existing keystore. Steps to create a new one:

### Initialize Keystore

Based on <https://learn.microsoft.com/en-us/dotnet/maui/android/deployment/publish-cli?view=net-maui-9.0>

``` powershell
keytool -genkeypair -v -keystore myapp.keystore -alias myapp -keyalg RSA -keysize 2048 -validity 10000
keytool -list -keystore myapp.keystore
dotnet publish -f net8.0-android -c Release -p:AndroidKeyStore=true -p:AndroidSigningKeyStore=myapp.keystore -p:AndroidSigningKeyAlias=myapp -p:AndroidSigningKeyPass=XXXXXXXXXXXXXXXXXXXXXXX -p:AndroidSigningStorePass=XXXXXXXXXXXXXXXXXXXXXXX
```

Put the used key into "https://github.com/stesee/NetMauiObtainiumHelloWorld/settings/secrets/actions/new" with the name "ANDROID_SIGN_KEY"

### Windows dev setup

That is not entirely passive.

``` powershell
# OpenJdk
winget install --id=Microsoft.OpenJDK.17 -e

#Android SDK
$url = "https://dl.google.com/android/repository/commandlinetools-win-9477386_latest.zip"
$output = "$env:LOCALAPPDATA\Temp\commandlinetools.zip"
Invoke-WebRequest -Uri $url -OutFile $output
$destination = "$env:LOCALAPPDATA\Android\Sdk\cmdline-tools\latest" 
New-Item -ItemType Directory -Force -Path $destination
Expand-Archive -Path $output -DestinationPath $destination
Move-Item -Path "$destination\cmdline-tools\*" -Destination $destination -Force
$sdkManager = "$env:LOCALAPPDATA\Android\Sdk\cmdline-tools\latest\bin\sdkmanager.bat"
& $sdkManager "platform-tools" "platforms;android-34" "build-tools;34.0.2"

## dotnet

dotnet workload install maui

## run as admin - will fail silently on started without admin rights
Start-Process -Wait -FilePath  "C:\Program Files (x86)\Microsoft Visual Studio\Installer\vs_installer.exe" -ArgumentList "modify --installPath ""C:\Program Files\Microsoft Visual Studio\2022\Professional"" --add Microsoft.VisualStudio.Workload.NetCrossPlat --includeRecommended --passive"

## 1st build

dotnet build -c Release /p:Version=1.2.3

## 1st signed build (windows/PS specific)

dotnet publish -f net8.0-android -c Release -p:Version=1.2.3 -p:AndroidKeyStore=true -p:AndroidSigningKeyStore=$pwd\myapp.keystore -p:AndroidSigningKeyAlias=myapp -p:AndroidSigningKeyPass=XXXXXXXXXXXXXXXXXXXXXXX -p:AndroidSigningStorePass=XXXXXXXXXXXXXXXXXXXXXXX -p:AndroidSdkDirectory=$env:LOCALAPPDATA\Android\Sdk

```
