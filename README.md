# Irun

Irun is a package manager for Linux distributions designed around absolute user freedom and simplicity.

At its core, Irun empowers anyone, anywhere, at any time to create, implement, and maintain their own packages. You do not need to wait for upstream maintainers, manage complex build pipelines, or navigate restrictive distribution rules. With minimal terminal experience, basic programming concepts, and more or less time (depending on the package you want), you can turn any software into an installable recipe for your system.

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

---

# Creating Your Own Package Recipe

Creating a package recipe requires only basic familiarization with YAML syntax and standard Linux command-line operations.

1. Create a file ending in .yml (e.g., mypackage.yml).
2. Define the package metadata, download URL, and build/install step instructions using the correct syntax (which you can find on other packages' recipe).
3. Place your file into your recipe directory or install it directly with `irun install -l ./mypackage.yml` (or `irun in -l ./mypackage.yml`).
