# Omarchy Secure Boot Manager

**Complete secure boot automation for Omarchy with Limine bootloader + UKI, including full snapshot support.**

A robust, single-script solution that handles everything from initial setup to ongoing maintenance with comprehensive error handling and edge case support.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Version](https://img.shields.io/badge/version-1.2-brightgreen.svg)](https://github.com/peregrinus879/omarchy-secure-boot-manager)

## 🚀 Quick Start

```bash
# Clone and run
git clone https://github.com/peregrinus879/omarchy-secure-boot-manager.git
cd omarchy-secure-boot-manager
chmod +x omarchy-secure-boot.sh

# Complete setup with snapshot support
./omarchy-secure-boot.sh --setup
```

## ✨ What's New in v1.2

### 🔄 **Complete Snapshot Hash Management**
- Handles non-standard snapshot naming (files ending with SHA256 instead of .efi)
- Exact matching for snapshots with identical base names
- Smart detection shows which snapshots need updates
- Bulk updates with safety confirmations

### 🏗️ **Professional Code Organization**
- 9 logical sections for maintainability
- Utility functions to eliminate code duplication
- Consistent error handling patterns
- Clean separation of concerns

### 🛡️ **Enhanced Safety & Reliability**
- Automatic backups before configuration changes
- Better hash mismatch detection and reporting
- Improved status checking that finds correct files
- Timeout protection on all operations

## 📋 What It Does

### 🔧 **Complete Setup**
- Installs required packages (`sbctl`, `efitools`, `sbsigntools`)
- **Always installs automation first** - never gets stuck
- Creates secure boot keys with clear BIOS guidance
- Signs ALL Linux EFI files including snapshots
- Fixes snapshot hash mismatches automatically
- Detects and configures Windows dual-boot

### ⚡ **Automatic Maintenance**  
- Signs ALL Linux EFI files after every package update
- Updates BLAKE2B hashes in `limine.conf`
- Correctly handles complex snapshot naming schemes
- Prevents secure boot failures after system updates
- Zero configuration needed after initial setup

### 🛡️ **Self-Managing System**
- Clean installation - removes old versions first
- Installs itself to `/usr/local/bin/`
- Creates pacman hook for automatic updates
- Automatic backups with timestamps
- Comprehensive status reporting

## 🎮 Commands

| Command | Description |
|---------|-------------|
| `--setup` | Complete robust setup with snapshot support |
| `--enroll` | Enroll keys after BIOS setup |
| `--status` | Check complete system status |
| `--fix-hashes` | Fix all hash mismatches |
| `--sign` | Manual signing with hash fixes |
| `--add-windows` | Add Windows to boot menu |
| `--update` | Maintenance (used by hook) |
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
   
5. **Done!** Everything is now automated

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

## ⭐ Key Features

| Feature | Description |
|---------|-------------|
| **Robust Setup** | Never fails to install automation |
| **Snapshot Support** | Handles all UKI naming schemes |
| **Hash Management** | Detects and fixes mismatches |
| **Dynamic Discovery** | Finds all EFI files automatically |
| **Windows Support** | Dual-boot configuration |
| **Error Resilient** | Continues even when steps fail |
| **Auto Backup** | Creates timestamped backups |
| **Clean Installation** | Removes old versions first |
| **Timeout Protection** | Commands can't hang |
| **Self-Installing** | Copies itself to system |
| **Self-Maintaining** | Pacman hook automation |
| **User-Friendly** | Colored output with clear instructions |

## 🔬 Technical Details

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

### How Hash Mismatches Occur
1. **Pre-existing snapshots**: Created with unsigned UKIs
2. **Signing changes files**: Adds signature data to the EFI
3. **Hash becomes invalid**: BLAKE2B hash no longer matches
4. **Without fix**: Boot shows warning, requires pressing 'Y'
5. **With v1.2 fix**: Hashes updated, boots without warnings

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

### System Integration
| Component | Location |
|-----------|----------|
| Pacman hook | `/etc/pacman.d/hooks/99-omarchy-secure-boot.hook` |
| Script | `/usr/local/bin/omarchy-secure-boot.sh` |
| Limine config | `/boot/limine.conf` |
| Backups | `/boot/limine.conf.backup.YYYYMMDD_HHMMSS` |

### Code Organization (v1.2)
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

### Common Issues & Solutions

| Issue | Solution |
|-------|----------|
| Hash mismatch warnings | Run `--fix-hashes` to update all hashes |
| Snapshots won't boot | Run `--fix-hashes` for snapshot hash updates |
| Windows not in boot menu | Run `--add-windows` to detect and add |
| Some EFI files not signed | Run `--sign` to re-scan and sign all |
| Keys won't enroll | Ensure BIOS is in Setup Mode |
| Commands hang | Script has timeout protection (15s) |

### Understanding Limine Hashes

The script manages two types of hashes:

1. **SHA256 in filename**: Part of the snapshot naming convention
   ```
   4a1b942b..._linux.efi_sha256_5ab709df...
   ```
   
2. **BLAKE2B after #**: Actually verified by Limine at boot
   ```
   image_path: .../file.efi#255a5c8055f4d06c...
   ```
   This is what causes "hash mismatch" warnings

## 📚 Version History

| Version | Date | Changes |
|---------|------|---------|
| **v1.2** | 2024-09-23 | Snapshot hash management, code reorganization, improved safety |
| **v1.1** | 2024-09-22 | Dynamic EFI discovery, Windows support, directory error fixes |
| **v1.0** | 2024-09 | Initial release with robust setup |

## 💡 Best Practices

1. **New Installation**: Run `--setup` once, handles everything
2. **Existing System**: Check `--status` first, then `--fix-hashes` if needed
3. **After Kernel Updates**: Automatic via pacman hook
4. **After Manual Changes**: Run `--sign` to ensure consistency
5. **Regular Checks**: Use `--status` to verify system health

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly (especially with snapshots)
5. Submit a pull request

### Testing Guidelines
- Test with various snapshot naming schemes
- Verify Windows detection on dual-boot systems
- Check hash updates with pre-existing snapshots
- Ensure automation continues after partial failures

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

---

**Secure boot should be simple, reliable, and work with any setup. This tool makes it so.**

*If this tool helped you, consider starring the repository!*
