# Omarchy Secure Boot Manager

**Complete secure boot automation for Omarchy with Limine bootloader + UKI.**

One script handles everything from packages to automation with robust error handling.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Version](https://img.shields.io/badge/version-1.1-brightgreen.svg)](https://github.com/peregrinus879/omarchy-secure-boot-manager)

## Quick Start

```bash
# Clone and run
git clone https://github.com/peregrinus879/omarchy-secure-boot-manager.git
cd omarchy-secure-boot-manager
chmod +x omarchy-secure-boot.sh

# Complete setup (robust)
./omarchy-secure-boot.sh --setup
```

## What's New in v1.1

🔍 **Dynamic EFI Discovery**
- Automatically finds ALL Linux-related .efi files
- No more hardcoded file lists - adapts to any installation
- Intelligently excludes Windows files from Linux signing

✅ **Enhanced Verification**
- Only processes actual .efi files (not directories or symlinks)
- Individual file verification with clear status display
- Completely eliminates directory operation errors
- Triple-checks file types before operations

🪟 **Windows Boot Support**
- Automatically detects Windows installations
- Adds Windows entry to Limine boot menu if missing
- Supports both same-partition and multi-partition setups

🔧 **Critical Fixes**
- Fixed "can't verify directory" errors
- Added timeout protection for each file operation
- Individual file processing prevents bulk failures

## What It Does

🔧 **Complete Setup**
- Installs required packages (`sbctl`, `efitools`, `sbsigntools`)
- **Installs automation FIRST** (never gets stuck)
- Creates secure boot keys with clear BIOS guidance
- Handles errors gracefully - essential components always install
- **NEW:** Detects and configures Windows dual-boot

⚡ **Automatic Maintenance**  
- Signs ALL Linux EFI files after every package update
- Updates BLAKE2B hashes in `limine.conf`
- Prevents secure boot failures after system updates
- **NEW:** Dynamic file discovery - no maintenance needed

🛡️ **Self-Managing**
- Clean installation - removes old versions first
- Installs itself to `/usr/local/bin/`
- Creates pacman hook for automatic updates
- Timeout protection prevents hanging commands

## Commands

```bash
./omarchy-secure-boot.sh --setup        # Complete robust setup
./omarchy-secure-boot.sh --enroll       # Enroll keys (after BIOS setup) 
./omarchy-secure-boot.sh --status       # Check secure boot status
./omarchy-secure-boot.sh --sign         # Manual signing if needed
./omarchy-secure-boot.sh --add-windows  # Add Windows to boot menu
./omarchy-secure-boot.sh --update       # Maintenance (used by hook)
./omarchy-secure-boot.sh --help         # Show all commands
```

## Setup Process

### For New Systems
1. **Clone and setup**: `./omarchy-secure-boot.sh --setup`
   - ✅ Installs packages
   - ✅ **Always installs automation** (even if other steps fail)
   - ✅ Creates keys (optional - setup continues if this fails)
   - ✅ Signs ALL Linux EFI files found
   - ✅ Checks for Windows and offers to add boot entry
   - ✅ Verifies status
   
2. **BIOS configuration** (if keys were created): 
   - Reboot → Enter BIOS/UEFI setup
   - Clear all existing secure boot keys (enter Setup Mode)
   - Save and reboot back to Linux
   
3. **Enroll keys**: `./omarchy-secure-boot.sh --enroll`
   
4. **Enable secure boot**:
   - Reboot → Enter BIOS → Enable Secure Boot
   - Save and reboot
   
5. **Done!** Everything is now automated

### For Dual-Boot Systems
```bash
# Setup handles Windows automatically
./omarchy-secure-boot.sh --setup

# Or add Windows later
./omarchy-secure-boot.sh --add-windows
```

## Features

✅ **Complete Setup** - Never gets stuck, always installs automation  
✅ **Dynamic Discovery** - Finds all EFI files automatically  
✅ **Windows Support** - Configures dual-boot seamlessly  
✅ **Error Resilient** - Continues even when individual steps fail  
✅ **Clean Installation** - Removes old versions before installing new  
✅ **Timeout Protection** - Commands can't hang indefinitely  
✅ **Self-Installing** - Copies itself to system during setup  
✅ **Self-Maintaining** - Creates pacman hook for automatic updates  
✅ **Comprehensive** - Handles packages, keys, signing, hashes  
✅ **User-Friendly** - Clear instructions and colored output  
✅ **Smart Recovery** - Provides guidance when things go wrong  

## Technical Details

### Dynamic EFI Discovery (v1.1)
- Scans `/boot` for all `.efi` files
- Automatically excludes Windows-related files
- No hardcoded paths - works with any installation
- Triple-checks file types before any operation
- Individual file verification with clear status reporting
- Completely eliminates directory operation errors

### Windows Boot Detection (v1.1)
Searches for Windows Boot Manager in:
- `/boot/EFI/Microsoft/Boot/bootmgfw.efi`
- `/boot/efi/Microsoft/Boot/bootmgfw.efi`
- `/efi/Microsoft/Boot/bootmgfw.efi`
- Other standard locations

Creates proper Limine entry with:
- Correct EFI chainload protocol
- Proper partition detection
- Priority ordering

### Integration
- Creates pacman hook: `/etc/pacman.d/hooks/99-omarchy-secure-boot.hook`
- Installs script: `/usr/local/bin/omarchy-secure-boot.sh`
- Updates BLAKE2B hashes in `/boot/limine.conf`
- Works alongside existing Omarchy hooks (proper execution order)

### Robust Design

#### Setup Flow (v1.1)
1. **Install packages** (critical foundation)
2. **Install automation FIRST** (ensures updates work even if setup fails later)
3. **Create keys** (optional - setup continues regardless)
4. **Sign ALL Linux EFI files** (dynamic discovery with type validation)
5. **Check for Windows** (add boot entry if found)
6. **Verify status** (individual file verification with clear reporting)

#### Error Handling
- Setup never exits early - automation always gets installed
- Clear error messages with recovery instructions
- Graceful handling of missing files or failed commands
- Smart status reporting based on actual system state
- Timeout protection prevents hanging on EFI operations
- Complete elimination of directory operation errors
- Individual file verification prevents bulk failures

## Requirements

- **Arch Linux** with Limine bootloader
- **UKI (Unified Kernel Image)** setup  
- **UEFI system** with Secure Boot capability
- **Administrative access** for system configuration
- **Optional:** Windows installation for dual-boot

## Version History

- **v1.1** (2024-09-22) - Dynamic EFI discovery, Windows boot support, upfront auth, fixed directory errors
- **v1.0** (2024-09) - Initial release with complete setup and robust error handling

## Troubleshooting

### Check Status
```bash
omarchy-secure-boot.sh --status
```
Shows:
- All discovered Linux EFI files
- Windows boot entry status
- Package installation state
- Key enrollment status
- Signature verification

### Manual Operations
```bash
# Sign all Linux EFI files
omarchy-secure-boot.sh --sign

# Add Windows to boot menu
omarchy-secure-boot.sh --add-windows

# Verify everything
sudo sbctl verify
```

### Common Issues

**Can't verify/sign directory error**: Fixed in v1.1 - now only processes actual files

**Windows not in boot menu**: Run `--add-windows` or `--setup` to detect and add

**Some EFI files not signed**: v1.1 dynamically finds all files - re-run `--sign`

**Setup stops at key creation**: v1.0+ fixes this - automation always installs first

**Old script still running**: v1.0+ does clean installation - removes old versions

**Keys won't enroll**: Check BIOS settings, ensure Setup Mode is enabled

**Commands hang**: v1.0+ adds timeout protection to prevent hanging

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly on various setups
5. Submit a pull request

## Repository Structure

```
omarchy-secure-boot-manager/
├── omarchy-secure-boot.sh    # The complete solution
├── README.md                 # This documentation
└── LICENSE                   # MIT License
```

## Support

- **Issues**: Use GitHub Issues for bug reports
- **Discussions**: Use GitHub Discussions for questions
- **Documentation**: This README covers all functionality

---

**Secure boot should be simple, reliable, and universal. This tool makes it so.**
