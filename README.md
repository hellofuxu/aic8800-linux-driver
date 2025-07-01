# AIC8800 Linux Driver for Kernel 6.11

This branch provides the source code for the AIC8800 Wi-Fi driver, specifically patched for compatibility with **Linux Kernel 6.11.x**.

## The Problem

The original driver fails to compile on Linux Kernel 6.11 (e.g., used in early versions of Ubuntu 24.04) due to breaking changes in the `cfg80211` wireless subsystem API.

The primary compilation errors encountered are:
```
error: too many arguments to function ‘cfg80211_ch_switch_notify’
...
error: too many arguments to function ‘cfg880211_ch_switch_started_notify’
```
This occurs because the number and type of arguments for these kernel functions were changed, and the driver was calling them with an outdated signature.

## The Solution

The source code in this branch has been patched to fix these function calls. Specifically, the calls to `cfg80211_ch_switch_notify` and `cfg80211_ch_switch_started_notify` in `AIC8800/drivers/aic8800/aic8800_fdrv/rwnx_main.c` have been updated to match the API of the 6.11 kernel.

### Tested Environment
*   **Kernel Version**: `6.11.0-28-generic`
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
    git checkout 6.11.0-28-generic
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