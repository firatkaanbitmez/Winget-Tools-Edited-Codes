# WinGet Tools (Edited Codes)
Update and Install your software using WinGet and PowerShell

# Features 

*Detailed Table

*C programing

*use for Powershell


# Example Image

![example](https://user-images.githubusercontent.com/74864221/217322309-fa6ae15c-08c4-450e-9a0f-2d3516a2c9f8.png)


# Upgrade Code

------------------------------------------------------------------------------

class Software {
    [string]$Name
    [string]$Id
    [string]$Version
    [string]$AvailableVersion
	
	
}

$upgradeResult = winget upgrade | Out-String

$lines = $upgradeResult.Split([Environment]::NewLine)

# Find the line that starts with Name, it contains the header
$fl = 0
while (-not $lines[$fl].StartsWith("Name"))
{
    $fl++
}

# Line $i has the header, we can find char where we find ID and Version
$NameStart = $lines[$fl].IndexOf("Name")
$idStart = $lines[$fl].IndexOf("Id")
$versionStart = $lines[$fl].IndexOf("Version")
$availableStart = $lines[$fl].IndexOf("Available")
$sourceStart = $lines[$fl].IndexOf("Source")

# Now cycle in real package and split accordingly
$upgradeList = @()
For ($i = $fl + 1; $i -le $lines.Length; $i++) 
{
    $line = $lines[$i]
    if ($line.Length -gt ($sourceStart + 1) -and -not $line.StartsWith('-'))
    {
		
        $name = $line.Substring(0, $idStart).TrimEnd()
        $id = $line.Substring($idStart, $versionStart - $idStart).TrimEnd()
        $version = $line.Substring($versionStart, $availableStart - $versionStart).TrimEnd()
        $available = $line.Substring($availableStart, $sourceStart - $availableStart).TrimEnd()
		
        $software = [Software]::new()
        $software.Name = $name+",";
        $software.Id = $id+",";
        $software.Version = $version+",";
        $software.AvailableVersion = $available;		
		
		
        $upgradeList += $software
		
    }
}

$upgradeList | Format-Table
