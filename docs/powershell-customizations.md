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
