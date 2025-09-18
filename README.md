# Omarchy Secure Boot Manager

**Complete secure boot automation for Omarchy with Limine bootloader + UKI.**

One script handles everything from packages to automation with robust error handling.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Version](https://img.shields.io/badge/version-1.0-brightgreen.svg)](https://github.com/YOUR_USERNAME/omarchy-secure-boot-manager)

## Quick Start

```bash
# Clone and run
git clone https://github.com/YOUR_USERNAME/omarchy-secure-boot-manager.git
cd omarchy-secure-boot-manager
chmod +x omarchy-secure-boot.sh

# Complete setup (robust)
./omarchy-secure-boot.sh --setup
```

## What It Does

🔧 **Complete Setup**
- Installs required packages (`sbctl`, `efitools`, `sbsigntools`)
- **Installs automation FIRST** (never gets stuck)
- Creates secure boot keys with clear BIOS guidance
- Handles errors gracefully - essential components always install

⚡ **Automatic Maintenance**  
- Signs EFI files after every package update
- Updates BLAKE2B hashes in `limine.conf`
- Prevents secure boot failures after system updates
- Comprehensive error handling and recovery

🛡️ **Self-Managing**
- Clean installation - removes old versions first
- Installs itself to `/usr/local/bin/`
- Creates pacman hook for automatic updates
- Timeout protection prevents hanging commands

## Commands

```bash
./omarchy-secure-boot.sh --setup    # Complete robust setup
./omarchy-secure-boot.sh --enroll   # Enroll keys (after BIOS setup) 
./omarchy-secure-boot.sh --status   # Check secure boot status
./omarchy-secure-boot.sh --sign     # Manual signing if needed
./omarchy-secure-boot.sh --update   # Maintenance (used by hook)
./omarchy-secure-boot.sh --help     # Show all commands
```

## Setup Process

### For New Systems
1. **Clone and setup**: `./omarchy-secure-boot.sh --setup`
   - ✅ Installs packages
   - ✅ **Always installs automation** (even if other steps fail)
   - ✅ Creates keys (optional - setup continues if this fails)
   - ✅ Signs files and verifies status
   
2. **BIOS configuration** (if keys were created): 
   - Reboot → Enter BIOS/UEFI setup
   - Clear all existing secure boot keys (enter Setup Mode)
   - Save and reboot back to Linux
   
3. **Enroll keys**: `./omarchy-secure-boot.sh --enroll`
   
4. **Enable secure boot**:
   - Reboot → Enter BIOS → Enable Secure Boot
   - Save and reboot
   
5. **Done!** Everything is now automated

### For New PCs/Laptops
```bash
git clone https://github.com/YOUR_USERNAME/omarchy-secure-boot-manager.git
cd omarchy-secure-boot-manager
./omarchy-secure-boot.sh --setup
# Follow any BIOS instructions shown, then:
./omarchy-secure-boot.sh --enroll  # (if keys were created)
```

## Features

✅ **Complete Setup** - Never gets stuck, always installs automation  
✅ **Error Resilient** - Continues even when individual steps fail  
✅ **Clean Installation** - Removes old versions before installing new  
✅ **Timeout Protection** - Commands can't hang indefinitely  
✅ **Self-Installing** - Copies itself to system during setup  
✅ **Self-Maintaining** - Creates pacman hook for automatic updates  
✅ **Comprehensive** - Handles packages, keys, signing, hashes  
✅ **User-Friendly** - Clear instructions and colored output  
✅ **Smart Recovery** - Provides guidance when things go wrong  

## Robust Design

### Setup Flow (v1.0)
1. **Install packages** (critical foundation)
2. **Install automation FIRST** (ensures updates work even if setup fails later)
3. **Create keys** (optional - setup continues regardless)
4. **Sign files** (always attempts)
5. **Verify status** (always reports current state)

### Error Handling
- Setup never exits early - automation always gets installed
- Clear error messages with recovery instructions
- Graceful handling of missing files or failed commands
- Smart status reporting based on actual system state
- Timeout protection prevents hanging on EFI operations

## Requirements

- **Arch Linux** with Limine bootloader
- **UKI (Unified Kernel Image)** setup  
- **UEFI system** with Secure Boot capability
- **Administrative access** for system configuration

## Technical Details

### EFI Files Managed
- `/boot/EFI/BOOT/BOOTX64.EFI`
- `/boot/EFI/limine/BOOTX64.EFI` 
- `/boot/EFI/limine/BOOTIA32.EFI`
- `/boot/EFI/Linux/<machine-id>_linux.efi`

### Integration
- Creates pacman hook: `/etc/pacman.d/hooks/99-omarchy-secure-boot.hook`
- Installs script: `/usr/local/bin/omarchy-secure-boot.sh`
- Updates BLAKE2B hashes in `/boot/limine.conf`
- Works alongside existing Omarchy hooks (proper execution order)

### Version History
- **v1.0** - Initial release with complete setup and robust error handling

## Troubleshooting

### Check Status
```bash
omarchy-secure-boot.sh --status
```

### Manual Signing
```bash
omarchy-secure-boot.sh --sign
```

### Verify Everything
```bash
sudo sbctl verify
sudo sbctl status
```

### Common Issues

**Setup stops at key creation**: v1.0 fixes this - automation always installs first

**Old script still running**: v1.0 does clean installation - removes old versions

**Keys won't enroll**: Check BIOS settings, ensure Setup Mode is enabled

**Commands hang**: v1.0 adds timeout protection to prevent hanging

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## Repository Structure

```
omarchy-secure-boot-manager/
├── omarchy-secure-boot.sh    # The bulletproof solution
└── README.md                 # This documentation
```

## License

MIT License - Feel free to use, modify, and distribute.

## Support

- **Issues**: Use GitHub Issues for bug reports
- **Discussions**: Use GitHub Discussions for questions
- **Documentation**: This README covers all functionality

---

**Secure boot should be simple and reliable. This tool makes it so.**
