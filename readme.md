# Showcase repo for CD fitting to Obtainium

- Set "Settings" -> "Actions" -> "General" -> "Workflow permissions" to "Read and write permissions"
- Add an git tag fiting the pattern 'v*.*.*', that will trigger a release build
- Minimal major version is '1'
- Install [Obtainium](https://github.com/ImranR98/Obtainium?tab=readme-ov-file#installation)
- Add "https://github.com/stesee/NetMauiOptainiumHelloWorld" to Obtainium.
- Use obtainium://github.com/stesee/NetMauiOptainiumHelloWorld on websites to outomate the last step, e.g. [![Obtainium](https://img.shields.io/badge/Obtainium%20Store-NetMauiOptainiumHelloWorld-green?style=flat&logo=android)](https://apps.obtainium.imranr.dev/redirect.html?r=obtainium://add/https://github.com/stesee/NetMauiOptainiumHelloWorld)

## Development

### Windows dev setup

winget install --id=Microsoft.OpenJDK.17 -e
$url = "https://dl.google.com/android/repository/commandlinetools-win-9477386_latest.zip"
$output = "$env:LOCALAPPDATA\Temp\commandlinetools.zip"
Invoke-WebRequest -Uri $url -OutFile $output
$destination = "$env:LOCALAPPDATA\Android\Sdk\cmdline-tools\latest"
New-Item -ItemType Directory -Force -Path $destination
Expand-Archive -Path $output -DestinationPath $destination
Move-Item -Path "$destination\cmdline-tools\*" -Destination $destination -Force
$sdkManager = "$env:LOCALAPPDATA\Android\Sdk\cmdline-tools\latest\bin\sdkmanager.bat"
& $sdkManager "platform-tools" "platforms;android-33" "build-tools;33.0.2"
& $sdkManager "platform-tools" "platforms;android-34" "build-tools;34.0.2"
dotnet build -c Release /p:Version=1.2.3 /p:AndroidSdkDirectory=$env:LOCALAPPDATA\Android\Sdk

dotnet build -c Release /p:AndroidSdkDirectory=$env:LOCALAPPDATA\Android\Sdk