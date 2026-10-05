# Image-Based Linux Summit Berlin September 29th 2026
🖼️ 🐧 🗻 🥙

# Attendee's projects

* systemd
* mkosi
* SUSE: MicroOS/Tumbleweed
* Fedora: [Bootc](https://docs.fedoraproject.org/en-US/bootc/getting-started/), [CoreOS](https://fedoraproject.org/coreos/), [Atomic Desktops (Silverblue, Kinoite)](https://fedoraproject.org/atomic-desktops/)
* Microsoft: confidential containers, [Flatcar](https://www.flatcar.org/), Azure Boost
* NixOS: systemd-{vmspawn,machined,repart,stub,boot,cryptenroll}, [snix-store](https://snix.dev/about/), [Lanzaboote](https://github.com/nix-community/lanzaboote), UKIs, [NixOS Bootspec](https://github.com/NixOS/rfcs/blob/master/rfcs/0125-bootspec.md), [userborn](https://github.com/nikstur/userborn), [Sécurix](https://github.com/cloud-gouv/securix)
* Canonical: Ubuntu Core, snaps, foundations/bootloader/package management
* Freedesktop-sdk
* Buildstream
* GNOME OS
* Arch Linux: sbctl/go-uefi, signstar(os), mkinitcpio, arch-install-scripts, devtools, buildbtw / vmexec, voa
* Meta (image builders)
* IncusOS/HypervisorOS

## Summary of Last Year's Progress

* sysupdate + sysinstall
* Building amutable OS implementing everything for real and smashing against reality
* Tine for building systems
  * Amutable had image builder but not build system
  * Build tine to do both
* NixOS
  * pcrlock support added to Lanzaboote — Measured Boot is now easy to use on NixOS
  * systemd initramfs enabled by default
  * Removing all the patches in systemd (20+ in the past → down to 4)
  * Added support for account-utils
  * Nix-based dev environment for systemd
  * Nightly tests for newest commit from systemd master for our (80+) VM test suite
  * Enabled many more Varlink APIs
  * WIP: Adopt Varlink in the core parts of NixOS
    * WIP: used in switch-to-configuration (a poor man's soft reboot)
  * WIP: Firmware attributes management
    * Userspace API to manage vtd, iommu, etc, firmware features
    * Implemented by Lenovo/Dell/HP
* MicroOS
  * OCI images support for updating microos optional available
  * Full sysext integration in OBS and transactional-update
  * Images will be built with mkosi instead of KIWI
  * Moved to sd-boot only, removed grub-bls, SLES 16 will follow
  * GLib uapi config spec support for usr -> etc -> run (https://docs.gtk.org/glib/method.KeyFile.load_unix_configurations.html)
  * WIP: FIDO2 + TPM2 at the same time
  * WIP: improving sd-boot UX
  * WIP: integrating UKI with brtfs snapshots
  * MicroOS switched to pcrlock-only, dropping support of pcr-oracle (signed policy)
* Archlinux
  * Work on VOA adoption
    * WIP: [Web of Trust support for OpenPGP](https://gitlab.archlinux.org/archlinux/alpm/voa/-/work_items/20)
    * WIP: [Signify as crypto backend](https://gitlab.archlinux.org/archlinux/alpm/voa/-/work_items/24)
  * mkinitcpio [defaults to systemd initramfs](https://lists.archlinux.org/archives/list/arch-dev-public@lists.archlinux.org/message/S2G5NU4YD7OL7TIGLN4GCV2T6F4RUPBJ/) now
  * WIP: Boot standardization and bootchain package [RFC](https://gitlab.archlinux.org/archlinux/rfcs/-/merge_requests/66)
  * Further work on [Signstar OS](https://gitlab.archlinux.org/archlinux/signstar-os) (Arch Linux based, image-based OS, built using mkosi) based on [Signstar](https://gitlab.archlinux.org/archlinux/signstar) tooling
* Canonical
  * Hybrid images (UKI + normal rootfs) for UEFI
  * Stubble for UKI stub, for better arm64 consumer devices
  * Database of arm64 hwids also merged into sd-stub, kept in sync with stubble
  * Verifies root of trust (AMD/Intel) during installation to see if the system is in a sensible state (IOMMU/root of trust/TPM state)
* Flatcar
  * Switched to confexts, found some pain points (also for sysexts)
  * Sysupdate also found pain points
  * Onboarded to AKS (Azure Kubernetes), resulting in Azure Container Linux (Flatcar downstream)
  * Many patches shipped in rapid update cadence due to Ai security boom
  * WIP: moving to mkosi, adding gentoo support
  * Shipping signed sysexts by default
  * "sysext bakery" for 3rd party images builds and deployments
  * NVIDIA driver sysext
* GNOME OS
  * Added mutable confext for `/etc/` shipping default config in the os
  * "Safe mode" UKI profile that disables sysext
    * Lots of fun due to NVIDIA drivers support
  * WIP moving to mkosi:
    * bootable iso support upstreamed
    * Image boots, ship it!
  * zswap used through loopdev
    * There is now a new functionality that allows it without writeback to the fs
    * Loop device to avoid allocating the whole thing, create new empty thing on every boot and use loopdev on top of it
  * Brtfs replace used in installer implemented via repart
* systemd STF
  * Sysinstall work + ongoing development of new GNOME GUI installer
  * Integrating homed with GNOME: basic support mostly landed, secure lock support not far behind
  * Sparse allocation support to the kernel for block stuff
  * WIP for stable/nightly channels support for sysupdate
  * sysupdate: support for delta downloads, fetching only changed chunks
    * Erofs images without fragment flag (actual flags: `-E fragments,fragdedupe=inode`)
    * Algorithm does chunking in fixed sizes, works thanks to erofs padding
    * Experiments show 90% reuse
  * sysupdate: Redesign for pluggable download backends
  * sysupdate: Separated download from apply steps
  * sysupdate: Ongoing Varlink impl. Once done we'll drop sysupdated/dbus API
  * homed: Ongoing change to key hierarchy so that we can derive extra keys => will be plumbed into org.freedesktop.Secrets
  * Some plumbing for skipping recovery key prompt and redirecting boot somewhere else (GNOME OS will use this for a more featureful TPM recovery UI)
* Ubuntu
  * 512 packages that were using system users/groups manually via postinst useradd/groupadd converted to systemd-sysusers
    * Just missing Avahi/sane-utils in terms of default GNOME install to be fully hermetic-usr supported
  * [gitlab snippet with tracker](https://salsa.debian.org/-/snippets/863)
* IncusOS
  * New project, now has users/customers
  * Uses mkosi for images
  * Initrd wipes everything that it doesn't know on boot from the rootfs, to ensure it always boot to known good state
  * API for config mgmt demon
  * Factory reset support
  * Reinstall/Backup support
  * Recovery disk, including safety against downgrade attacks
  * UKI with multiple profile
  * Pain due to debian stable as base, old systemd (and everything else), might switch to upstream packages published on OBS
* Fwupd
  * There's a built-in hook for pcrlock to exclude HW PCRs at upgrade time
* KDELinux
  * New initiative with the idea to use reuse same base as GNOMEOS but in KDE world (freedesktop-sdk/BuildStream)
  * Idea is to share tech and infrastructure with GNOMEOS as much as possible (include systemd-sysupdate / security etc)

# Topics

* Maturity levels for systemd subsystems (e.g. nsresourced, etc.) for distribution adoption/promotion?
  * Unclear when things are "ready" for production
  * "Experimental" tag for components
    * D-Bus now has annotation for "experimental" API in the spec
    * Intention to do the same for Varlink
    * Manpages also supports tags for component or individual feature
    * Meson tag for individual component
  * API vs component stability
  * For backport we don't make this distinction, based on bug severity
  * All of these should provide a hint, but with the understanding that it is a hint, not a hard promise
* Alternative to PE as boot image container format, i.e. no EFI
  * Embedded linux has issues
  * U-boot uses FIT image (flattened image tree, a device-tree derivation)
  * But PE is so simple! Just do it! (™)
    * Authenticode is not bad, not great
    * Openssl does the heavy lifting, X.509 ecosystem is well supported if not pretty
  * Someone implemented reading [PE from some bootloader in MBR](https://github.com/nkraetzschmar/bootloader) and presented at ASG 2025
    * ASG talk: ["One Boot Config to Rule Them All: Bringing UAPI Boot Specification to Legacy BIOS"](https://www.youtube.com/watch?v=x5vKCc8fVJI)
  * Kernel in a UKI is callable in "classic bios" mode (i.e., avoid EFI stub/entry point)
  * U-boot has UEFI support, it's spaghetti all the way down
* BLI as UAPI spec? What for non-EFI systems. A file in the ESP? Extras in loader.conf?
  * s390x/ppc have completely different bootchains/bootloaders
  * Needs a champion to do the actual work
  * Not all variables should be part of the spec, as they are specific to sd-boot
    * Needs to be discussed var-by-var with s390x/ppc devs
    * Majority seems to be generic enough, but content also can be different
    * E.g.: what is an `ID`? Value not well defined, changes by implementation
    * Would be good to clarify in spec
  * Grub supports BLI, but has some issues on non-x86
  * systemd spec is not up to date, there are new vars that sd-boot/stub use
  * Device trees and EFI vars are very different, maybe needs to be a different spec
* Graphical systemd-boot?
  * Feedback for users when rollbacks need to be selected is that the GUI could use colors or emojis
  * Separate component that has fancy graphic?
  * Hi DPI screens have a lot of issues
  * Use EFI protocols to implement a terminal emulator
  * APIs for blessing boots available to make a better job from the OS of selecting rollbacks
  * Text stuff should really be improved regardless, as often it's hard to read, with the pager and so on
  * Link to [SUSE systemd-boot sub-menu PR](https://github.com/systemd/systemd/pull/43110)
  * [GNOME mockups](https://gitlab.gnome.org/Teams/Design/os-mockups/-/blob/master/boot/boot-menu.png?ref_type=heads)
  * Accessibility story? Non-existent
    * We beep!
    * Absent anything else, even Morse code would be better than nothing
  * GNOME OS has discussed "recovery" known-good partition that has functioning speech synthesis etc.
    * But it means this code gets out of date, security issues
  * Could GNOME show the boot menu?
    * If TPM fails to unlock redirect to another target to recover
    * Then users can select next boot from there
  * Kmscon also available
    * BUT NO EMOJIIS!!! 😭😭😿
    * No, it has emojis?? 🚀🎉
  * Conclusion: do it from the OS
* Signing of DDIs/Kernel keyring
* TOFU model for authenticating resources in sd-stub
* dm-verity keyring deep-dive: trust relationships and mechanisms for key injection
  * Amutable have their own key as usual but want to let users to enroll their own keys on top, e.g. for confexts
  * How to enroll? - i.e. how do trusted keys securely reach the node?
  * Kernel now has a separate dm-verity keyring that is filled at boot and then locked
    * Support for systemd WIP
    * Keyring set-up would be useful if it could be fully sealed (i.e. no new keys at all)
    * Current implementation allows new keys signed by existing keys
  * TOFU-style enrollment of keys
    * Users get golden image and modify it to add their keys
    * On first boot untrusted stuff is accepted
    * sd-stub looks for addons, those that don't authenticate are collected and stores their hashes in a protected EFI var (that can't be modified by userspace) - or a file on the ESP?
    * Make it opt-in via UKI profile metadata, consumed by userspace later
    * UKI metadata may also be used for kernel command line, DeviceTree and similar customisations (down the road, possible but not implemented)
  * Problem: key revocation is not part of that concept yet
  * GNOME OS builds everything first, then signs everything later
    * WIP patch to add new keyring called ".vendor" with keys that shim trusts, and UKIs can carry new keys in a new section that get added to this keyring
  * MOK manager, separate EFI binary to do enrollment of keys
    * Can we have a better GUI?
    * Maybe add new menu item creates new key and hands it to userspace, that would never leave the initrd, and then from the initrd it can be used to sign stuff to pass back to the UEFI mode
      * Key stored in boot-only EFI var, or EFI config table (ephemeral)
    * Goes back to the idea of having a fancy userspace that can do better UI/UX than a bootloader, and jump back to UEFI
    * Can TOFU substitute MOK?
    * Installer GUI selects to reboot into the special env that can access to the secret TOFU key, that env boots and enrolls via the HMAC, then reboot again
    * The special env needs to prompt the local user and do all the needed checks
    * Regenerate the key when it is used
  * UEFI measures every key (some key?) used via LoadImage
  * Conditional MOK variable: only add key if it is scoped to the "thing" being booted
    * E.g. enroll NVIDIA drivers only for Fedora OSes
* Learnings about TPM2 firmware quality at scale for measured boot adoption?
  * e.g. quirks hwdb about specific feature
  * Already has entry for TPMs
  * But things change (e.g. firmware updates)
  * Better to auto-detect problems at boot
  * Add journal specific for bug triaging
    * It's called… The journal!
  * We have the journal catalog too for fancy messages
  * Ensure the entry has all the needed data already so it's easy for users to extract and report
  * Test suite in the installer to verify how a system behaves as it is installed, to avoid installing a broken system
  * Tie it up in systemd-analyze or so, to have a well-formatted report that users can send easily without having to dig manually for parts
* sysexts: unified approach for OS overlays for immutable image-bases Linux
  * Root vs. individual directories (etc, opt, usr - what about var and others)
  * Balancing distro vendor vs operator needs
  * Node specific "ephemeral" overlays rendered at boot, snapshotting, etc.
  * Nixos has automatic provision of data on first boot (sql, etc.)
  * Customer want to ship an entire overlay in a single entity that covers the entire system
  * UAPI FS spec assigns very precise semantics to the parts of the hierarchy, and this wires into features like firstboot, factory-reset, etc
  * Ownership: sysext is owned by distro, confext is for operators
  * Expand Mstack feature to allow booting from it, which allows to do a full set of overlay layers, also supports bind mounts
  * confext is not sysext and is not varext and won't be rootext, do not want to lose well-defined semantics and atomicity
  * Use case provides OS plus a set of OS-vendor owned sysexts, customer adds more layer on top of that
  * Can also ship filesystems snapshots to provide pre-provisioned state
  * UAPI specs should be more verbose on the semantics of these components
* Sysupdate now supports multiplexing out to Varlink, can update sysext/confext and auto refresh them
  * Add also notification support via Varlink to sysext/confext themselves
  * But always prefer `StateDirectory=`, `RuntimeDirectory=` and friends
    * Currently if a unit is gone (service file deleted, systemd daemon-reload) we lose track, should be fixed so that a stub is kept behind for later cleanups
  * Currently `BindPaths=` does not support id-mapping, could be added
  * Could remove `/var/lib/private/` and use foreign UIDs range to protect content instead
* external UKI profiles?
  * Cmdline specific snapshot
  * Mix type1 and type2 to solve it
  * `bootctl link`/`bootctl unlink` creates type1 entry from type2 entry
    * Only combines references, does not create new config
    * Profile field added as supported part of this
    * Takes UKI plus secondary components as input
  * bootctl link-auto
    * Supports `.v` directory in usr|var with the kernel and the rest, picks up all content
    * Hooked up to Varlink notification of sysupdate
    * Opposed to kernel-install, which generates stuff locally and has plugins
* kernel-install future
  * See bootctl link as a replacement
* VOA all the things
  * Adoption and standardization of [dedicated configuration file format](https://voa.archlinux.page/config/voa.5.html) for policy enforcement
    * Proposal: once the 2nd consumer (in terms of a backend, e.g. X509), let's standardize something
  * Extending the specification: X509/CMS/PKCS#7, signify
  * Adding further backends to [reference implementation](https://voa.archlinux.page/) (X509/CMS/PKCS#7, signify)
  * Use of Rust-based reference implementation in C-based projects
  * OpenPGP can have many options
    * Can have multiple layers and trust anchors
    * can be pinned by fingerprints,
    * WoT: describe what the roots are, how many levels to allow
    * Need a policy enforcement on top of all of this, X.509 embeds this in the certs themselves
    * Metadata mostly specific to consumer of the data
    * Openpgp does not support embedded a lot of this into the format
    * End users want to customize some of these parameters
  * systemd VOA parser is generic
* pcrlock: support more than 8 variants (PolicyOR limitation)
  * Answer: yes please
* more granular UKI cmdline options
  * Use case: user has a problem, support tells user to enable X Y Z options
  * Answer: bootctl link to pick the right combination, pre-prepare the various individual components
  * Enhance sd-boot to select addons from available superset
    * And also to disable a specific addon
    * All ephemeral for the next boot
  * Figure out a way to safely do developer mode (as Android, etc)
    * Needs some hard thinking as it's not trivial problem in detail
    * Different developer modes? One that you can enter/exit freely, one that requires wipe-on-enter, one that requires wipe-on-exit? Turn off IPE or enroll self-signed keys? Turn off NNP? Shell access? etc etc etc
* NNP (NoNewPrivs) and containers: scoped, filtered by cgroup labels. BPF magic enforces on "good" (labelled) containers; "bad" containers that don't run with this can be excluded.
  * SUSE has a presentation on this at ASG this year
* WIP: pty broker PR
  * Get a pty and connect to a container
  * Can also open a shell in the background
  * Asks precise policy questions
  * run0 moved to this for the simple use cases
    * See also https://build.opensuse.org/package/show/openSUSE:Factory/fudo
* [Journald on mmc](https://github.com/systemd/systemd/issues/15292)
  * IOPS are the problem
  * Duplicate journal: full fat journal to `/run/`, filtered to `/var/`, even per-unit (via cgroup xattr or/and priority filtering)
  * Glob for log element in the cgroup xattr
  * Let the kernel coalesce more by having iouring do the writes instead of doing manually
  * Switch away from mmap writes
    * Journald uses mmap writing because the docs said it was undefined behaviour to mix mmap read and pwrite/iouring, but that is no longer the case
  * Log namespace already exists that runs a completely separate and independent journald instance, units can use this
* pcrlock: for A/B booted system (atomic updates)
  * Not very atomic (ESP + nv index)
    * Writes the policy first, then the NvPCR, as the first by itself has no functional implication
    * But if nv index is written and esp is not, also hard failure
  * If pcrlock runs without the ESP then booting fails
  * Could keep both old/new policies around as fallback
  * pcrlock locks to as many PCRs as possible, including PCR12 which is where creds are measured to, including the pcrlock policy itself
  * Special case it, and have sd-stub load it from the esp into an EFI var?
    * Special casing sucks
    * But it is a special case
  * Another atomicity hole: migrating from normal PCR -> pcrlock during a system update
  * Communicate which LUKS slot unlocked the disk
    * Lennart thinks we have logic to measure what unlock method is used ("TPM" vs "password" …), including the slot number itself
* sysinstall for other OSes?
  * Amutable uses it for headless installs
  * GNOMEOS for the GUI installer
  * What requirements do others have?
  * Nix needs installer for corp env, talking to some remote server
    * System is customized and provisioned depending on the user + hardware
    * Are other people interested in sharing these services/components?
    * Specifically
      * Enrollment
      * OIDC auth
      * Factory reset
      * FIDO configuration
    * Update client pulls updates (NixOS config) from the server, which keeps track of clients, has mutable and immutable use cases
  * Windows Autopilot does the same
    * Machine is authenticated via TPM
    * Log in with OIDC
    * Customized based on user/org settings
  * What Amutable does currently
    * Machine boots empty
    * Machine does IMDS
    * Machine sends reports (contains TPM, CoCo signatures, etc) (Enrolls w/ Orchestrator)
    * Orchestrator pushes down configuration (including maybe features in sysupdate)
    * (Some features should be controlled locally: NVIDIA hardware, detect cloud => install agent, etc)
    * Report contains PCRs and quotes, confext locks against them, so machine can only use them if the machine is in the same state
* no brute force make-policy / predict (slow)
  * Algo is bruteforcing to do all the possible combinations which is slow
  * Predicts what is going to fail and skips it
  * Combinatorial explosion even with only 3 snapshots
  * Calls make-policy twice
  * Tries hard to keep all PCRs locked
  * 1 minute enrolment in a constrained VM
  * PCR15 extended with hash of volume key, so a service can abort switchroot
    * PCR15 can be wrong as for example a new device can be added
    * User faces scary alert
    * Need a way to bypass and continue boot after manually checking
    * In a user friendly way
  * PCR15 should not measure all keys, but the identity of the system, which is why it measures the rootfs volume key

# Action Items

* Add notification support via Varlink to systemd-sysext for merging / un-merging sysext/confext. Currently, only systemd-sysupdate supports this.
* Handle StateDirectory= RuntimeDirectory= and friends for unit files that "disappear" (service file deleted, systemd daemon-reload). Currently if a unit is gone we lose track, should be fixed so that a stub is kept behind for later cleanups.
* pcrlock: support more than 8 variants (PolicyOR limitation)
* Think about what knobs should exist for a "developer mode", what guarantees can we give afterwards, etc
