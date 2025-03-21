# PowerShell setup

Some customizations to my `$profile` file.

## Launch the first *.sln file found

```PowerShell
function Launch-Sln
{
    Invoke-Item (Get-ChildItem -r -i *.sln | Select-Object -first 1).Fullname
}
New-Alias vsgo Launch-Sln
```

## Git CLI aliases

```PowerShell
function Git-Status
{
    git status
}
New-Alias gst Git-Status
```

```PowerShell
function Git-Checkout-Main-And-Pull
{
    git checkout main && git pull
}
New-Alias gmp Git-Checkout-Main-And-Pull
```

## Quick start

```PowerShell
function New-Dotnet-Project([string] $ProjectName, [string] $MainType = 'classlib')
{
    # Create working directory

    New-Item $ProjectName -ItemType Directory
    Set-Location $ProjectName
    $workingDirectory = Get-Location

    New-Item .github/workflows -ItemType Directory

    New-Item docs -ItemType Directory
    New-Item docs/README.md -ItemType File
    Add-Content -Path docs/README.md -Value "# $ProjectName"

    # Create solution file
    
    dotnet new gitignore
    dotnet new sln --output "$workingDirectory" --name "$ProjectName"

    # Create main project

    $mainProjectDir = "$workingDirectory/src/$ProjectName"
    $mainProjectPath = "$mainProjectDir/$ProjectName.csproj"

    New-Item $mainProjectDir -ItemType Directory

    dotnet new $MainType --output $mainProjectDir --name "$ProjectName"
    dotnet sln add $mainProjectPath

    # Create test project

    $testProjectDir = "$workingDirectory/test/$ProjectName.Tests"
    $testProjectPath = "$testProjectDir/$ProjectName.Tests.csproj"

    New-Item $testProjectDir -ItemType Directory

    dotnet new xunit --output $testProjectDir --name "$ProjectName.Tests"
    dotnet add $testProjectPath reference $mainProjectPath
    dotnet sln add $testProjectPath

    # Create harness project

    $harnessProjectDir = "$workingDirectory/tools/Harness"
    $harnessProjectPath = "$harnessProjectDir/Harness.csproj"

    New-Item $harnessProjectDir -ItemType Directory

    dotnet new console --output $harnessProjectDir --name "Harness"
    dotnet add $harnessProjectPath reference $mainProjectPath
    dotnet sln add $harnessProjectPath
}

New-Alias magic New-Dotnet-Project
```
