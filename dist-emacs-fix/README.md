# AeroSpace — Emacs child frame fix (arm64)

Patched build of [AeroSpace](https://github.com/nikitabobko/AeroSpace) based on the SNAPSHOT branch, with the fix from [PR #2036](https://github.com/nikitabobko/AeroSpace/pull/2036) applied.

**What it fixes:** AeroSpace steals focus from Emacs when a child frame closes (corfu, eldoc-box, posframe completion popups). After this patch, child frames are treated as transient popups and ignored by the window manager. Tracked in [issue #776](https://github.com/nikitabobko/AeroSpace/issues/776).

## Requirements

- Apple Silicon Mac (arm64)
- macOS 13 Ventura or later

## Install

1. Copy `AeroSpace.app` to `/Applications`:
   ```
   cp -R AeroSpace.app /Applications/
   ```

2. Copy the `aerospace` CLI to your PATH:
   ```
   cp aerospace /opt/homebrew/bin/aerospace
   ```

3. Strip the quarantine flag (required because this build is not notarized):
   ```
   xattr -dr com.apple.quarantine /Applications/AeroSpace.app
   xattr -dr com.apple.quarantine /opt/homebrew/bin/aerospace
   ```

4. Launch AeroSpace from `/Applications` or add it to Login Items.

## Notes

- This is an ad-hoc signed build. Step 3 is mandatory or macOS will block it.
- The fix detects Emacs child frames by the absence of window chrome buttons and `AXMain == false`. It matches both `Emacs.app` (bundle ID `org.gnu.Emacs`) and emacs daemons launched via nix-darwin (nil bundle ID, AXTitle "emacs").
- Once PR #2036 is merged into the official AeroSpace release, you can switch back to the Homebrew formula and uninstall this build.
