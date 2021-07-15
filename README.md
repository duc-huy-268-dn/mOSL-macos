# macOS Lockdown (mOSL)
Bash script to audit and fix macOS security settings

- This script will **only ever** support macOS release  
- This script requires your **password** to invoke some commands with `sudo`  

This repository is fork from mOSL(https://github.com/0xmachos/mOSL) and Open-Audit(https://github.com/Opmantek/open-audit/blob/master/other/audit_osx.sh)

## `brew`

tap: [`est-rouge/er-homebrew-mosl`](https://bitbucket.org/est-rouge/er-homebrew-mosl)

To install mOSL via `brew` execute:

```
brew tap est-rouge/er-homebrew-mosl https://bitbucket.org/est-rouge/er-homebrew-mosl.git
brew install mosl
```

mOSL will then be available as:

```
Lockdown
```

## Threat Model(ish)

The main goal is to enforce already secure defaults and apply more strict non-default options.

It aims to reduce attack surface but it is pragmatic in this pursuit. The author utilises Bluetooth for services such as Handoff so it is left enabled.

There is **no specific focus** on enhancing privacy.

Finally, mOSL will not protect you from the [FSB](https://en.wikipedia.org/wiki/Federal_Security_Service), [MSS](https://en.wikipedia.org/wiki/Ministry_of_State_Security_(China)), [DGSE](https://en.wikipedia.org/wiki/Directorate-General_for_External_Security), or [FSM](https://en.wikipedia.org/wiki/Flying_Spaghetti_Monster).

## SHA1 command to Verify File Integrity
```
Lockdown v1.0:
$ shasum $(which Lockdown)
a843fb857fa761b9f734968a5c0389513c358e19

```
## Usage

```
$ ./Lockdown

  Audit or Fix macOS security settings🔒🍎

  Usage: ./Lockdown [list | audit {setting_index} | fix {setting_index} | debug]

    list         - List settings that can be audited/ fixed
    audit        - Audit the status of all or chosen setting(s) (Does NOT change settings)
    fullaudit    - Audit the status of all or chosen setting(s) and Dump information entire MacOS system (Does NOT change settings)
    fix          - Attempt to fix all or chosen setting(s) (Does change settings)

    fix-force    - Same as 'fix' however bypasses user confirmation prompt
                   (Can be used to invoke Lockdown from other scripts)
    debug        - Print debug info for troubleshooting

```

Settings that can be audited/ fixed:
```

  [0] Disable Automatic System Updates
  [1] Disable Automatic App Store Updates
  [2] Enable Firewall
  [3] Require an administrator password to access system-wide preferences
  [4] 5 Minutes to lock and sleep computer
  [5] Enable Terminal.app secure keyboard entry
  [6] Enable FileVault
  [7] Disable Remote Login
  [8] Delete a firmware password

```
