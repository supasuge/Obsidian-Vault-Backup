```powershell
# Define variables
$url = "https://download.sysinternals.com/files/SysinternalsSuite.zip"
$output = "$env:TEMP\SysinternalsSuite.zip"
$destination = "C:\SysinternalsSuite"

# Download the Sysinternals Suite
Invoke-WebRequest -Uri $url -OutFile $output

# Create the destination directory if it doesn't exist
if (-Not (Test-Path -Path $destination)) {
    New-Item -ItemType Directory -Path $destination
}

# Extract the ZIP file
Expand-Archive -Path $output -DestinationPath $destination

# Clean up the downloaded ZIP file
Remove-Item -Path $output

# Add Sysinternals Suite to the System PATH
$sysinternalsPath = "C:\SysinternalsSuite"
$currentPath = [Environment]::GetEnvironmentVariable("PATH", [EnvironmentVariableTarget]::Machine)

if ($currentPath -notlike "*$sysinternalsPath*") {
    [Environment]::SetEnvironmentVariable("PATH", "$currentPath;$sysinternalsPath", [EnvironmentVariableTarget]::Machine)
}

# Output message to restart PowerShell
Write-Output "Sysinternals Suite has been installed and added to the PATH. Please restart your PowerShell session to apply the changes."
```

### Steps to Run the Script

1. Open PowerShell with administrative privileges.
2. Copy the script above.
3. Paste the script into the PowerShell window and press Enter.
4. Wait for the script to complete.
5. Restart your PowerShell session to apply the PATH changes.

[Microsoft Documentation](https://learn.microsoft.com/en-us/sysinternals/downloads/)


