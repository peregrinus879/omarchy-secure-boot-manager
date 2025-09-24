# Omarchy Secure Boot Manager

**Complete secure boot automation for Omarchy with Limine bootloader + UKI, including full snapshot support.**

A robust, single-script solution that handles everything from initial setup to ongoing maintenance with comprehensive error handling and edge case support.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Version](https://img.shields.io/badge/version-1.3.0-brightgreen.svg)](https://github.com/peregrinus879/omarchy-secure-boot-manager)

## 🚀 Quick Start

```bash
# Clone and run
git clone https://github.com/peregrinus879/omarchy-secure-boot-manager.git
cd omarchy-secure-boot-manager
chmod +x omarchy-secure-boot.sh

# Complete setup with snapshot support
./omarchy-secure-boot.sh --setup
```

## ✨ Version History

### v1.3.0 (2025-01-24)
**The Conflict Resolution Update** - Permanently neutralizes the conflicting sbctl hook that causes errors with Omarchy's directory structure.

- **NoExtract Configuration**: Prevents problematic sbctl hook from ever being installed/reinstalled
- **Hook Renamed**: Changed from `99-` to `zz-` prefix for proper execution order
- **Clean Uninstall**: New `--uninstall` command removes automation while preserving keys
- **Automatic Migration**: Seamlessly upgrades from v1.2.x installations
- **Enhanced Status**: Shows NoExtract configuration and conflict resolution status

### v1.2.1 (2025-01-23)
**The Refinement Update** - Code cleanup and intelligent Windows detection for more accurate boot entries.

- **Code Cleanup**: Removed unused functions and variables for maintainability
- **Smart Windows Detection**: Automatically detects Windows 10 vs Windows 11
- **Cleaner Codebase**: Eliminated dead code paths

### v1.2.0 (2025-01-23)
**The Snapshot Perfection Update** - Complete support for Omarchy's complex snapshot naming schemes and professional code organization.

- **Complete Snapshot Hash Management**: Handles files ending with SHA256 instead of .efi
- **Professional Code Structure**: Organized into 9 logical sections for maintainability
- **Bulk Hash Updates**: Fix all mismatches with user confirmation
- **Enhanced Safety**: Automatic backups before configuration changes
- **Better Error Reporting**: Detailed hash mismatch information

### v1.1.0 (2025-01-22)
**The Universal Compatibility Update** - Dynamic discovery and Windows dual-boot support.

- **Dynamic EFI Discovery**: Finds all Linux EFI files automatically
- **Windows Support**: Detects and configures Windows dual-boot
- **Directory Bug Fixes**: Handles files vs directories correctly

### v1.0.0 (2025-01-18)
**The Foundation Release** - Initial robust implementation with core automation features.

- **Robust Setup**: Always installs automation even if other steps fail
- **Automatic Maintenance**: Pacman hook for updates
- **Basic Secure Boot**: Key creation and enrollment

## 📋 What It Does

### 🔧 **Complete Setup**
- Installs required packages (`sbctl`, `efitools`, `sbsigntools`)
- **Always installs automation first** - never gets stuck
- Creates secure boot keys with clear BIOS guidance
- Signs ALL Linux EFI files including snapshots
- Fixes snapshot hash mismatches automatically
- Detects and configures Windows dual-boot
- **Permanently neutralizes conflicting sbctl hook**

### ⚡ **Automatic Maintenance**  
- Signs ALL Linux EFI files after every package update
- Updates BLAKE2B hashes in `limine.conf`
- Correctly handles complex snapshot naming schemes
- Prevents secure boot failures after system updates
- Zero configuration needed after initial setup
- **No more error messages from conflicting hooks**

### 🛡️ **Self-Managing System**
- Clean installation - removes old versions first
- Installs itself to `/usr/local/bin/`
- Creates pacman hook for automatic updates
- Automatic backups with timestamps
- Comprehensive status reporting
- **NoExtract configuration survives package updates**

## 🎮 Commands

| Command | Description |
|---------|-------------|
| `--setup` | Complete robust setup with snapshot support and conflict resolution |
| `--enroll` | Enroll keys after BIOS setup |
| `--status` | Check complete system status including conflict resolution |
| `--fix-hashes` | Fix all hash mismatches (current + snapshots) |
| `--sign` | Manual signing with hash fixes |
| `--add-windows` | Add Windows to boot menu |
| `--update` | Maintenance (used by hook) |
| `--uninstall` | **NEW** - Clean removal of automation (keeps keys/signatures) |
| `--help` | Show all commands |

## 📝 Setup Process

### For New Systems

1. **Initial Setup**
   ```bash
   ./omarchy-secure-boot.sh --setup
   ```
   - ✅ Installs packages
   - ✅ Always installs automation (even if other steps fail)
   - ✅ Creates keys (optional - setup continues if this fails)
   - ✅ Signs ALL Linux EFI files including snapshots
   - ✅ Fixes snapshot hash mismatches automatically
   - ✅ Detects Windows and offers boot entry
   - ✅ **Neutralizes conflicting sbctl hook permanently**
   - ✅ Verifies everything with detailed reporting
   
2. **BIOS Configuration** (if keys were created)
   - Reboot → Enter BIOS/UEFI setup
   - Navigate to Secure Boot settings
   - Clear all existing keys (enter Setup Mode)
   - Save and reboot back to Linux
   
3. **Key Enrollment**
   ```bash
   ./omarchy-secure-boot.sh --enroll
   ```
   
4. **Enable Secure Boot**
   - Reboot → Enter BIOS
   - Enable Secure Boot
   - Save and reboot
   
5. **Done!** Everything is now automated and conflict-free

### For Systems with Existing Snapshots

If you have snapshots created before setting up secure boot:

```bash
# Check for hash mismatches
./omarchy-secure-boot.sh --status

# Fix all mismatches automatically
./omarchy-secure-boot.sh --fix-hashes
```

This eliminates the need to press 'Y' when booting old snapshots!

### For Dual-Boot Systems

```bash
# Setup handles Windows automatically
./omarchy-secure-boot.sh --setup

# Or add Windows later
./omarchy-secure-boot.sh --add-windows
```

### Clean Uninstall

```bash
# Remove automation while keeping keys and signatures
./omarchy-secure-boot.sh --uninstall
```

## ⭐ Key Features

| Feature | Description |
|---------|-------------|
| **Conflict Resolution** | Permanently neutralizes problematic sbctl hook |
| **NoExtract Protection** | Prevents conflicting hook from reinstalling |
| **Clean Uninstall** | Removes automation without affecting security |
| **Robust Setup** | Never fails to install automation |
| **Snapshot Support** | Handles all UKI naming schemes |
| **Hash Management** | Detects and fixes mismatches |
| **Dynamic Discovery** | Finds all EFI files automatically |
| **Windows Support** | Dual-boot configuration |
| **Error Resilient** | Continues even when steps fail |
| **Auto Backup** | Creates timestamped backups |
| **Self-Installing** | Copies itself to system |
| **Self-Maintaining** | Pacman hook automation |
| **User-Friendly** | Colored output with clear instructions |

## 🔬 Technical Details

### The sbctl Hook Conflict (v1.3.0)
The official `sbctl` package includes a hook that fails with Omarchy's directory structure:
- Tries to sign directory `/boot/{machine-id}` as a file
- Looks for snapshots in wrong locations
- Generates scary error messages after every update

Our solution:
- Configures pacman's `NoExtract` to never install the problematic hook
- Removes existing hook if present
- Takes over all signing with proper Omarchy support
- Clean uninstall that reverses all changes

### Snapshot Hash Management
The script handles complex snapshot naming where files don't end with `.efi` but with their SHA256 hash:
```
# Standard: filename.efi
# Your snapshots: filename.efi_sha256_[64-char-hash]
```

- Detects snapshots with incorrect hashes after signing
- Shows detailed comparison of expected vs actual hashes
- Interactive confirmation before bulk updates
- Creates automatic backup of limine.conf
- Handles complex filename patterns safely
- Works with both standard and non-standard naming

### Dynamic EFI Discovery
- Scans `/boot` for all `.efi` files
- Automatically excludes Windows-related files
- No hardcoded paths - works with any installation
- Triple-checks file types before operations
- Individual file verification with status reporting

### Windows Boot Detection
Searches multiple locations:
- `/boot/EFI/Microsoft/Boot/bootmgfw.efi`
- `/boot/efi/Microsoft/Boot/bootmgfw.efi`
- `/efi/Microsoft/Boot/bootmgfw.efi`
- All mounted FAT/NTFS partitions
- Detects Windows 10 vs Windows 11

### System Integration
| Component | Location |
|-----------|----------|
| Script | `/usr/local/bin/omarchy-secure-boot.sh` |
| Pacman hook | `/etc/pacman.d/hooks/zz-omarchy-secure-boot.hook` |
| Old hook (removed) | `/etc/pacman.d/hooks/99-omarchy-secure-boot.hook` |
| Problematic sbctl hook | `/usr/share/libalpm/hooks/zz-sbctl.hook` (disabled) |
| NoExtract config | `/etc/pacman.conf` |
| Limine config | `/boot/limine.conf` |
| Backups | `/boot/limine.conf.backup.YYYYMMDD_HHMMSS` |

### Code Organization
```
Section 1: Configuration & Constants
Section 2: Output Colors & Logging
Section 3: Core Utility Functions
Section 4: Discovery Functions
Section 5: Verification & Check Functions
Section 6: Installation & Setup Functions
Section 7: Core Operations
Section 8: Command Implementations
Section 9: Help & Main Execution
```

## 📊 Requirements

- **Arch Linux** with Limine bootloader
- **UKI (Unified Kernel Image)** setup  
- **UEFI system** with Secure Boot capability
- **Administrative access** for system configuration
- **Optional:** Windows installation for dual-boot

## 🔧 Troubleshooting

### Check Complete Status
```bash
omarchy-secure-boot.sh --status
```
This shows:
- All discovered EFI files (current + snapshots)
- Hash mismatch detection with details
- Windows boot entry status
- Package installation state
- Key enrollment status
- Signature verification results
- **NoExtract configuration status**
- **Conflicting hook status**

### Common Issues & Solutions

| Issue | Solution |
|-------|----------|
| Error messages after update | Already fixed in v1.3.0 - run `--setup` to apply |
| Hash mismatch warnings | Run `--fix-hashes` to update all hashes |
| Snapshots won't boot | Run `--fix-hashes` for snapshot hash updates |
| Windows not in boot menu | Run `--add-windows` to detect and add |
| Some EFI files not signed | Run `--sign` to re-scan and sign all |
| Keys won't enroll | Ensure BIOS is in Setup Mode |
| Want to remove automation | Run `--uninstall` for clean removal |

### Understanding Error Messages

Before v1.3.0, you might see:
```
failed signing /boot/4a1b942b...: is a directory
failed signing /boot/4a1b942b.../limine_history/...: does not exist
```

These are from the conflicting sbctl hook. After running v1.3.0 setup, these errors disappear permanently.

## 💡 Best Practices

1. **New Installation**: Run `--setup` once, handles everything including conflicts
2. **Upgrading from v1.2.x**: Just run `--setup`, it migrates automatically
3. **Existing System**: Check `--status` first, then `--fix-hashes` if needed
4. **After Kernel Updates**: Automatic via pacman hook
5. **After Manual Changes**: Run `--sign` to ensure consistency
6. **Regular Checks**: Use `--status` to verify system health
7. **Clean Removal**: Use `--uninstall` instead of manual deletion

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly (especially with snapshots and sbctl conflicts)
5. Submit a pull request

### Testing Guidelines
- Test with various snapshot naming schemes
- Verify Windows detection on dual-boot systems
- Check hash updates with pre-existing snapshots
- Ensure automation continues after partial failures
- Verify NoExtract configuration persists after sbctl updates
- Test clean uninstall and reinstall

## 📁 Repository Structure

```
omarchy-secure-boot-manager/
├── omarchy-secure-boot.sh    # The complete solution
├── README.md                 # This documentation
└── LICENSE                   # MIT License
```

## 🆘 Support

- **Issues**: [GitHub Issues](https://github.com/peregrinus879/omarchy-secure-boot-manager/issues)
- **Discussions**: [GitHub Discussions](https://github.com/peregrinus879/omarchy-secure-boot-manager/discussions)
- **Wiki**: [Documentation Wiki](https://github.com/peregrinus879/omarchy-secure-boot-manager/wiki)

## 📜 License

MIT License - See [LICENSE](LICENSE) file for details

## 🙏 Acknowledgments

- Omarchy Linux community for testing and feedback
- Limine bootloader developers
- sbctl project for secure boot management tools
- Arch Linux team for the robust package management system

---

**Secure boot should be simple, reliable, and error-free. Version 1.3.0 makes it so.**

*If this tool helped you, consider starring the repository!*
