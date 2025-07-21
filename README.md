# AIC8800 Linux Driver for Kernel 6.14+

This branch provides the source code for the AIC8800 Wi-Fi driver, specifically patched for compatibility with **Linux Kernel 6.14.x** and newer versions with a similar API structure.

## The Problem

The original driver fails to compile on Linux Kernel 6.14 (e.g., found in Ubuntu 24.04 updates) due to significant breaking changes in the kernel's wireless subsystem API.

Compilation errors typically include:
```
error: incompatible pointer types in function assignments for 'set_monitor_channel'
error: too few arguments to function ‘cfg80211_cac_event’
error: expected ‘,’ or ‘;’ before ‘VFS_internal_I_am_really_a_filesystem_and_am_NOT_a_driver’
```
These errors arise because kernel function signatures have been updated, and obsolete macros have been removed.

## The Solution

The source code in this branch has been patched to address these specific issues. The key fixes are:
-   Updated function signatures in `rwnx_main.c` and `rwnx_radar.c` to match the 6.14 kernel API.
-   Removed the obsolete `MODULE_IMPORT_NS` macro.
-   Corrected parameter types and counts in function calls to the `cfg80211` subsystem.

### Tested Environment
*   **Kernel Version**: `6.14.0-24-generic`
*   **Distribution**: Ubuntu 24.04 LTS
*   **Target Chip**: AIC8800 (USB Wi-Fi Adapter)

---

## Installation Guide

The recommended installation method is to build a `.deb` package.

### Prerequisites

Install the necessary build tools and kernel headers for your system.
```bash
sudo apt update
sudo apt install build-essential linux-headers-$(uname -r) git dpkg-deb
```

### Building and Installing the `.deb` Package

1.  **Clone the repository and switch to this branch:**
    ```bash
    git clone https://github.com/hellofuxu/aic8800-linux-driver.git
    cd aic8800-linux-driver
    git checkout 6.14.0-24-generic
    ```

2.  **Build the `.deb` package:**
    Run this command from *outside* the `aic8800-linux-driver` directory.
    ```bash
    cd ..
    dpkg-deb --build aic8800-linux-driver
    ```
    This will create a new file named `aic8800-linux-driver.deb`.

3.  **Install the package:**
    ```bash
    sudo dpkg -i aic8800-linux-driver.deb
    ```
    The driver will now be compiled and installed. Your Wi-Fi adapter should be recognized automatically. You may need to unplug and replug the adapter.

---

## Uninstallation

To remove the driver, simply use `dpkg` to uninstall the package:
```bash
sudo dpkg -r aic8800fdrvpackage
```
(The package name inside the `.deb` is `aic8800fdrvpackage`).