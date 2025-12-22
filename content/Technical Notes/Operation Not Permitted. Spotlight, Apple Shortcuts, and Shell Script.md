---
share: true
date: 2025-11-10
tags:
  - macos
  - shortcuts
  - shell
---
Recently, I installed a new macOS 26 Tahoe and decided to try how can I now run my shortcuts right from the Spotlight. I created a simple shortcut that gets a file from Spotlight and runs a shell command `cat "$1"` to view the content of the currently selected file in Finder from my user's home directory.

However, when I ran the shortcut, I encountered an error message "Operation Not Permitted" in the output of the shell command action.

I did some research on this issue and there were common consensus that granting Full Disk Access either to the Shortcuts app or Finder would resolve the issue. However, this did not work for me.

It appears that **a shortcut runs in a sandboxed environment of an application that called the shortcut**. In my case, it was Spotlight, which does not have Full Disk Access permission. If the shortcut runs from the Shortcuts app, this app must have Full Disk Access. If you run the shortcut from Finder, the same applies to Finder's access rights, and so on.

Granting Full Disk Access permission to Spotlight in **System Settings** > **Privacy & Security** > **Full Disk Access** resolved the issue. The Spotlight can be found in the `/System/Library/CoreServices/Spotlight.app` path.
