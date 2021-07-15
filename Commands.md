# Commands

Easy to read, non one-liner, version of all the `audit` and `fix` commands used by ert-mOSL.   


## Disable Automatic System Updates

### Audit
```
if ! defaults read "/Library/Preferences/com.apple.SoftwareUpdate.plist" "AutomaticallyInstallMacOSUpdates" >/dev/null 2>&1; then
	exit 1
	# This key isnt present if the user has not manually interacted with System Preferences > Software Update before
fi
defaults read "/Library/Preferences/com.apple.SoftwareUpdate.plist" "AutomaticallyInstallMacOSUpdates" | grep -q "0"

```

### Fix
```
declare -a keys

keys=(AutomaticDownload AutomaticallyInstallMacOSUpdates)

for key in "${keys[@]}"; do
	defaults write "/Library/Preferences/com.apple.SoftwareUpdate.plist" "${key}" -bool false
done
```

## Disable Automatic App Store Updates

### Audit
```
if ! defaults read "/Library/Preferences/com.apple.commerce.plist" "AutoUpdate" >/dev/null 2>&1; then
	exit 1
fi
defaults read "/Library/Preferences/com.apple.commerce.plist" "AutoUpdate" >/dev/null 2>&1 | grep -q '0';
```

### Fix
```
defaults write "/Library/Preferences/com.apple.commerce.plist" "AutoUpdate" -bool false
```

## Enable Firewall

### Audit
```
sudo /usr/libexec/ApplicationFirewall/socketfilterfw  --getglobalstate | grep -q 'enabled'
```

### Fix
```
sudo /usr/libexec/ApplicationFirewall/socketfilterfw --setglobalstate on
```

## Require an administrator password to access system-wide preferences

### Audit
```
security -q authorizationdb read system.preferences | grep -A1 'shared' | grep -q 'false'
```

### Fix
```
security -q authorizationdb read system.preferences > /tmp/system.preferences.plist
/usr/libexec/PlistBuddy -c 'Set :shared false' /tmp/system.preferences.plist
sudo security -q authorizationdb write system.preferences < /tmp/system.preferences.plist
rm '/tmp/system.preferences.plist'
```

## Set 5 minutes time lock and sleep computer

### Audit
```
sudo systemsetup -getcomputersleep | grep -q '5 minutes'
```

### Fix
```
sudo systemsetup -setcomputersleep 5 ;sudo systemsetup -setdisplaysleep 3;$(osascript -e 'tell application "System Events" to set require password to wake of security preferences to true')
```

## Enable `Terminal.app` secure keyboard entry

### Audit
```
defaults read com.apple.Terminal SecureKeyboardEntry | grep -q '1'
```

### Fix
```
defaults write com.apple.Terminal SecureKeyboardEntry -bool true
```

## Enable FileVault

### Audit
```
fdesetup status | grep -q 'On'
```

### Fix
```
sudo fdesetup enable -user $USER > $HOME/FileVault_recovery_key.txt
```

## Disable Remote Login

### Audit
```
sudo systemsetup -getremotelogin | grep -q 'Remote Login: Off'
```

### Fix
```
sudo systemsetup -f -setremotelogin off
```

## Disable Safari Auto Open 'safe' Files

### Audit
```
defaults read com.apple.Safari AutoOpenSafeDownloads | grep -q '0'
```

### Fix
```
defaults write com.apple.Safari AutoOpenSafeDownloads -bool false
```

## Delete a firmware password

### Audit
```
sudo firmwarepasswd -check | grep -q 'No'
```

### Fix
```
sudo firmwarepasswd -delete
```
