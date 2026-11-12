# Irun

Irun is a package manager for Linux distributions designed around absolute user freedom and simplicity.

**Warning: Irun is not user friendly and is complicated and very bare-metal by design. Do not put this on your primary device and use it as your primary package manager. Please do not adventure and torture yourself into this if you don't have:**
- Time (a lot of time depending of your use of Irun).
- A basic knowledge of the linux terminal and/or bash.
- A basic knowledge of what is and how to use APT.
- A basic programming (like Python or other and Json/YaML or other).

**Warning: Also keep in mind that Irun is still in EXTREME early days. There are NO security whatsoever, the UX is practically non-existant and the few .desktop there is does NOT work on every configurations and every desktop environment. PLEASE do NOT use this as your primarily package manager! I WARNED YOU! It WILL NOT be a good time!**

Irun's concept is that anyone, anywhere, at any time can create, implement, and maintain their own packages. You do not need to wait for upstream maintainers, manage complex build pipelines, or navigate restrictive distribution rules. With minimal terminal experience, basic programming concepts, and more or less time (depending on the package you want), you can turn any software into an installable recipe for your system.

---

# Why Choose Irun?

## Recipes
Package creation is fully decentralized. Every package is defined by a standard .yml recipe file. If a tool or library is missing from the repository, you can write a recipe yourself using simple key-value fields and shell commands, making package creation accessible to virtually any user.

## Hybrid Installation System
Irun offers you the choice between grabbing the pre-made binary and compiling directly from source. You decide whether to grab pre-built packages for speed or build them locally for custom configurations or if there aren't pre-built binaries.

## Experimental Support for Immutable Systems
Irun is architected to run on immutable Linux distributions such as Fedora Silverblue or Bazzite without altering system root files.

> [!NOTE]  
> Support for immutable systems is theoretical and currently untested across all environments. Certain software (such as Firefox, VLC, or Fastfetch) are not working on immutable file systems. Work in progress to fix these.

---

# Dependencies

Before using or creating recipes with Irun, make sure your system has the following requirements installed.

## Mandatory Core
- python3
- python-yaml (or PyYAML)
- git

## Recommended Build Environment
- cmake
- gcc
- base-devel (or equivalent build-essential package)
- vulkan-headers
- tar
- unzip
- pip

## Arch Linux Package Names
- yaml
- python3
- python-pip
- gcc
- cmake
- base-devel
- vulkan-headers
- git
- tar
- unzip

---

# Basic Usage

## Managing Packages

Installing a package:
```bash
irun install <package_name>
irun in <package_name>
```

Removing a package:
```bash
irun remove <package_name>
irun rem <package_name>
```

## Updating Repositories and System

Refresh the GitHub recipe repository (similar to apt update):
```bash
irun refresh @gr
```

Update a single package (similar to apt upgrade):
```bash
irun update <package_name>
```

Update all installed packages across the whole system:
```bash
irun update @ws
```

## Maintenance

Clean temporary build and cached repository files:
```bash
irun clean
```
It is recommended to run the clean command after heavy package updates, large source compilations, or whenever disk space needs to be reclaimed.

## Informations
You can get a lot of informations of the packages WITHOUT even installing the package.
You can get every info commands by typing "irun info -h" in your terminal.
Here's the info -h command and some comments for easier interpretation:
usage: irun info [-h] (-d | -v | -i | --install-method | --id | --desktop) target

```bash
positional arguments:
  target            Package name or ID

options:
  -h, --help        show this help message and exit
  -d                Show dependencies
  -v                Show package and recipe versions
  -i                Show installation status
  --install-method  Show install method
  --id              Show package ID
  --desktop         Show the package desktop file's file path

example:
"irun info -v firefox" to print version informations of Firefox
```

---

# Creating Your Own Package Recipe

Creating a package recipe requires only basic familiarization with YAML syntax and standard Linux command-line knowledge.

### Create the recipe file
You will need to create a recipe file with the .yml extension, and the main file name as your package's name.
You have two ways to install it:
1. Place it in ~/.irun/repositories/gr/recipes/yourpackage.yml
2. Install it directly from the download/creation path like so: `irun install -l ./yourpackage.yml`

### The YAML recipe syntax
This is the example configuration for a recipe:
```yaml
recipe_version: 1 # this is a very important value. You NEED to change it for Irun to see that your package got updated.
# The recipe_version must be an integer
package:
  name: "MyPackage" # this is your package name
  version: 1.69 # this is your package version
  description: "my description" # this is your package description. isn't really useful, you can skip or fill.
  author: "Michael Jackson" # this is the author of the package. Officially built and distributed packages from Irun will have the association with "Author | Irun Official". Please do NOT use this association. Also the developer did NOT create a affiliation with Irun.
  id: "aaaa.0000.aaaa.0001" # this is the package ID. please put an available ID.

install_method: "" # this is the installation method. this tells irun whether to compile from source or just go grab the binary

binary: # all of the following block is for downloading and installing the pre-made binary
  url: "" # this is the download URL. make sure to have a correct URL or else it will fail. it downloads in ~/.irun/tmp/
  additional_urls: [] # additional URLs, they are downloaded in ~/.irun/tmp/
  sha256: "" # this is the sha256. there are currently no SHA256 verification so this don't matter if it is filled or not
  bin_install_steps: # here are the binary installation steps. keep in mind these commands are bash
    - "mkdir -p ~/.irun/pkg/mypackage/bin/" # recommended with every bin_install_steps to create the installation directory
    - "unzip ~/.irun/tmp/package.zip -d ~/.irun/pkg/mypackage/bin/" # this just unzips the pre-built binary to the package directory

source: # this is the source block. the literal exact same as the binary thing with different (but similar) names
  url: ""
  additional_urls: []
  sha256: ""
  build_steps:
    # Build/compilation steps, in pure bash like so:
    - "echo Step 1"
    - "touch myfile.txt"
    # these commands are examples and do not do anything useful
  install_steps:
    - ""
  
dependencies: # this defines the dependencies
  runtime: # there is not really any difference between runtime and build, just to signify to the user which one is for what
    - "python3"
  build:
    - "gcc"
  # also the dependency part is practically useless as there is literally nothing to handle dependencies in any way
```

4. Define the package metadata, download URL, and build/install step instructions using the correct syntax (which you can find on other packages' recipe).
5. Place your file into your recipe directory or install it directly with `irun install -l ./mypackage.yml` (or `irun in -l ./mypackage.yml`).
