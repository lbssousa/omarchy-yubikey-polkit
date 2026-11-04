# YubiKey Polkit Agent

A theme-aware Polkit agent that shows a touch prompt while PAM waits for a security key,
with fingerprint and password prompts when those methods are requested.

**Version 0.2.0.** This standalone plugin comes from
[Omarchy YubiKey](https://github.com/marselnenaj/omarchy-yubikey), which also provides guided key enrollment and PAM
configuration. See the suite release notes for validation and compatibility.

## Requirements

Omarchy 4 with its Quickshell plugin host, `qs.Commons`, `qs.Ui`, the PAM/Polkit
modules and authentication-clone support. The source environment used Omarchy
4.0.6. Other shell versions are unqualified. This is trusted authentication
code that runs inside the shell; review it before enabling it.

The lock needs the shell's existing `omarchy-lock-password` PAM service. For key
unlock it also needs `omarchy-lock-fido2` and a central enrollment mapping.
Polkit needs the matching key authentication rule in `/etc/pam.d/polkit-1`.
[Guided setup and recovery](https://github.com/marselnenaj/omarchy-yubikey/blob/v0.2.0/docs/install.md) configures those
pieces with backups. Installing this plugin alone does not alter PAM or enroll
a key, and it does not install the optional Keyring helper.

## Install

Use the suite's guided installer if you want the full setup. To install just
this shell plugin after configuring PAM:

```sh
omarchy plugin add https://github.com/marselnenaj/omarchy-yubikey-polkit.git
omarchy plugin enable yubikey.polkit
```

Enable it while the desktop is unlocked. Disable any other custom replacement
for `omarchy.polkit` first; keep a recovery terminal available. The host uses
`omarchy.clonedFrom` to replace the stock service. Do not mix a Git-managed copy
with a directory already copied by the suite installer under the same ID.
Test key and password authentication before relying on it. Plugin commands
follow the repository's default branch; use the suite release for a fixed source
snapshot. [PAM instructions](https://github.com/marselnenaj/omarchy-yubikey/blob/v0.2.0/docs/pam-setup.md) and
[security model](https://github.com/marselnenaj/omarchy-yubikey/blob/v0.2.0/docs/security-model.md) describe PIN mode,
explicit password fallback and limitations. Updating plugin files alone does not
update PAM policy. Re-run reviewed suite setup to enable PIN/explicit fallback.

## Update and remove

While unlocked:

```sh
omarchy plugin update yubikey.polkit
```

To remove it:

```sh
omarchy plugin disable yubikey.polkit
omarchy plugin remove yubikey.polkit
```

On the supported host, disabling the clone restores the stock service. If you
previously used another custom clone, re-enable it yourself. Removal does not
change PAM or delete enrollment data. Follow the suite's rollback instructions
if you also want to undo that setup.

## Source and license

This is a distribution repository. Its maintained source is
[`plugins/yubikey.polkit`](https://github.com/marselnenaj/omarchy-yubikey/tree/v0.2.0/plugins/yubikey.polkit) in the suite.
`SOURCE.json` records the source ref and hashes of the exported files. Report
bugs and send changes to [the suite](https://github.com/marselnenaj/omarchy-yubikey/issues).

MIT; QML derives from the Omarchy shell. See `THIRD_PARTY_NOTICES.md`.
Independent community software, unaffiliated with Yubico, GNOME or Omarchy.
