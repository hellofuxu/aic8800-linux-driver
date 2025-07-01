# AIC8800 Linux Driver (Patched for Modern Kernels)

This repository provides patched versions of the AIC8800 Wi-Fi driver, ensuring compatibility with modern Linux kernels (version 6.x and newer). Due to frequent API changes in the Linux kernel, a single driver version cannot support all kernel releases. This project maintains separate branches for different kernel versions.

## Which Branch Should I Use?

**First, check your kernel version:**
```bash
uname -r
```

**Then, choose the branch that most closely matches your kernel version:**

*   **For Kernel `6.14.x`** (e.g., `6.14.0-24-generic` on Ubuntu 24.04):
    *   **Branch:** [`6.14.0-24-generic`](https://github.com/hellofuxu/aic8800-linux-driver/tree/6.14.0-24-generic)
    *   This branch contains patches for API changes introduced up to the 6.14 kernel series.

*   **For Kernel `6.11.x`** (e.g., `6.11.0-28-generic`):
    *   **Branch:** [`6.11.0-28-generic`](https://github.com/hellofuxu/aic8800-linux-driver/tree/6.11.0-28-generic)
    *   This branch is tailored for the 6.11 kernel series.

**Please navigate to the appropriate branch for detailed instructions and the correct source code.**

---

### Quick Installation Overview

While detailed instructions are in each branch's `README`, the general process is as follows:

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/hellofuxu/aic8800-linux-driver.git
    cd aic8800-linux-driver
    ```

2.  **Switch to the correct branch for your kernel:**
    ```bash
    # Replace with your target branch name
    git checkout <branch-name> 
    ```

3.  **Build and install the `.deb` package.** (See branch `README` for details)

### General Information

#### Troubleshooting

*   **Secure Boot:** If your system has Secure Boot enabled, it may block the loading of this unsigned kernel module. You may need to sign the module yourself or disable Secure Boot in your UEFI/BIOS settings.
*   **Kernel Headers:** Ensure `linux-headers-$(uname -r)` is installed and matches your running kernel. If you've just updated your kernel, reboot before building.

#### Disclaimer

This is an unofficial community patch provided "as is" without warranty. The original driver source code belongs to its respective copyright holders. Use at your own risk.

#### Contributing

If you've fixed the driver for a kernel version not listed here, please submit a pull request with your changes in a new, clearly named branch. Bug reports for existing branches are also welcome.
