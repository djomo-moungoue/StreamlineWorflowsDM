# StreamlineWorflows

# PowerShell

Retrieve the PowerShell Version
~~~ps1
$PSVersionTable
~~~

Upgrade The Current PowerShell Version to PowerShell 7
~~~ps1
"Installing PowerShell 7 ..."
winget install --id Microsoft.Powershell --source winget

"Retrieving the latest version of PowerShell ..."
winget search Microsoft.PowerShell
~~~
By default the package is installed to `$env:ProgramFiles\PowerShell\<version>`

Install PowerShell or PowerShell Preview using the id parameter
~~~ps1
"Install PowerShell or PowerShell Preview using the id parameter ..."
winget install --id Microsoft.Powershell --source winget
winget install --id Microsoft.Powershell.Preview --source winget
~~~

Retrieve Windows Environment Variables
~~~ps1
Get-ChildItem env:
~~~

 Retrieve Windows Paths
~~~ps1
$env:PATH -split ';'
~~~

Add to the Windows PATH environment variable
~~~ps1
"Initializing the path to append ..."
$addPath = "C:\Users\yesse\AppData\Local\Programs\Microsoft VS Code\bin"

"Creating the backup of the current env variables path ..."
$BackupEnvVarPath = "$env:USERPROFILE\BackupEnvVarPath.csv"
New-Item -ItemType File $BackupEnvVarPath -force
Set-Content -Path $BackupEnvVarPath -Value $env:PATH
$BackupContent = (Get-Content $BackupEnvVarPath).Split(';')
$BackupContent

"Removig the path if it already exists ..."
$arrPath = $env:Path -split ';' | Where-Object {$_ -notlike "$addPath*"}

"Appending the new path at the end of existing environment paths existing ..."
$env:Path = ($arrPath + $addPath) -join ';'

"New list of paths ..."
$env:PATH -split ';'
~~~

Remove a Path from the Windows PATH environment variable
~~~ps1
"Initializing the path to remove ..."
$removePath = ""
"Current list of paths ..."

"Creating the backup of the current env variables path"
$BackupEnvVarPath = "$env:USERPROFILE\BackupEnvVarPath.csv"
New-Item -ItemType File $BackupEnvVarPath -force
Set-Content -Path $BackupEnvVarPath -Value $env:PATH
$BackupContent = (Get-Content $BackupEnvVarPath).Split(';')
$BackupContent

"Removig the path if it already exists ..."
$arrPath = $env:Path -split ';' | Where-Object {$_ -notlike "$removePath*"}
$env:Path = $arrPath -join ';'

"New list of paths ..."
$env:PATH -split ';'
~~~

# Git 

## Create a new repository on the command line
CAUTiON: Works only if the credential Manager is deactivated
~~~ps1
echo "# $UserRepo" >> README.md
git init
git add README.md
git commit -m "Automatd commit"
git branch -M main
git remote add origin https://github.com/djomo-moungoue/$UserRepo.git
git push -u origin main
~~~

## Push an existing repository from the command line
`CAUTiON`: Works only if the credential Manager is deactivated
~~~ps1
"Adding a new remote repository"
git remote add origin https://github.com/djomo-moungoue/$UserRepo.git
# git remote add origin git@github.com:User/UserRepo.git # Alternative command
git branch -M main
git push -u origin main
~~~

## Usual Commands

Change the url of an existing remote repository
~~~ps1
"Changing the url of an existing remote repository"
git remote set-url origin git@github.com:User/UserRepo.git
"Verifying that you the remote URL is set correctly ..."
git remote -v
~~~

Push your code to the master branch of the remote repository defined with origin and -u let you point your current local branch to the remote master branch:
~~~ps1
git push --set-upstream origin main # Alias -u
~~~

## Troubleshooting

### Problem
Pushing change to the remote repository https://github.com/$GitHubUser/$UserRepo.git ...
fatal: 'origin' does not appear to be a git repository
fatal: Could not read from remote repository.

### Solution
~~~ps1
git remote add origin https://github.com/$GitHubUser/$UserRepo.git
~~~

### Problem
git remote add origin https://github.com/$GitHubUser/$UserRepo.git
git push --set-upstream origin main # alias -u
remote: Repository not found.
fatal: repository 'https://github.com/$GitHubUser/$UserRepo.git/' not found

### Solution
- Open "Manage Windows Credentials" from the Start menu.
- Delete any credentials related to Git or GitHub.
- Once you deleted all then try to clone again.


# GitHub

## Install Github CLI

Open Powershell as an Administrator.

### Install Chocolatey
Source: https://curriculeon.github.io/Curriculeon/lectures/terminal/dos/install-chocolatey/content.html
~~~ps1
Write-Host
"Installing Chocolatey on Windows OS ..."
Write-Host
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))

Write-Host
"Adding chocolatey to the env var path ..."
$env:PATH = $env:PATH + ";C:\ProgramData\chocolatey"
Write-Host

Write-Host
"Verifying that Github CLI has been added correctly ..."
Write-Host
$env:PATH -split ';'

Write-Host
"Verifying that chocolatey has been installed correctly ..."
Write-Host
choco --version # Alias: -v
~~~

### Upgrade Chocolatey
~~~ps1
Write-Host
"Upgrade Chocolatey ..."
Write-Host
choco upgrade chocolatey
~~~

### Install Gitbhub CLI
Source: https://curriculeon.github.io/Curriculeon/lectures/version-control-systems/git/ghcli-installation/content.html
~~~ps1
Write-Host
"Installing Github CLI on Windows OS ..."
Write-Host
choco install gh # Use --force to reinstall, specify a version to install, or try upgrade.

Write-Host
"Adding Github CLI to the env var path ..."
Write-Host
$env:PATH = $env:PATH + ";C:\Program Files\GitHub CLI"

Write-Host
"Verifying that Github CLI has been added correctly ..."
Write-Host
$env:PATH -split ';'

Write-Host
"Verifying that Github CLI has been installed correctly ..."
Write-Host
gh version

Write-Host
"Authenticating Github CLI assuming that the user environment variable GH_TOKEN is set properly ..."
Write-Host
#   -p, --git-protocol string   The protocol to use for git operations on this host: {ssh|https}
#  -h, --hostname string       The hostname of the GitHub instance to authenticate with
#  -w, --web                   Open a browser to authenticate
#      --with-token            Read token from standard input
gh auth login --git-protocol "https" --hostname "GitHub.com" --with-token
~~~

Help Command Usage
~~~
gh <command> <subcommand> --help
# Example
gh auth login --help
~~~

Make the current repository public
~~~ps1
# gh repo edit --help
gh repo edit --visibility="public"
~~~

Clone a remote repository
~~~ps1
# gh repo clone --help
gh repo clone $GitHubUserName/$RepoName       
~~~

## Troubleshooting

### Problem
gh : The term 'gh' is not recognized as the name of a cmdlet, function, script file, or 
operable program. Check the spelling of the name, or if a path was included, verify that   
the path is correct and try again.

### Solution
Install GitHub CLI and add its path to the Windows environment variables

### Solution


# Conda
Prevent Conda from activating the base environment by default
~~~ps1
conda config --set auto_activate_base false
~~~

Deactive a conda environment on the current terminal session
~~~ps1
conda activate env_name
~~~

Deactive a conda environment on the current terminal session
~~~ps1
conda deactivate
~~~
