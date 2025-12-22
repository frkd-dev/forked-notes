---
date: 2020-12-24
tags:
  - macos
  - gpu
share: true
---
_How to get 4 Gb instead of 1.5 Gb of video RAM for integrated Intel GPU on MacBook 2015 models._

Once upon a time, I was very frustrated with the amount of RAM used by integrated Intel GPU on my MacBook Pro. I always was low on memory while working with graphics-intensive apps which were using Metal acceleration for computing. I believed that GPU is sharing a memory with CPU so the limits are software. After some googling and researching I've managed out how to increase 1.5GB to 4GB.

Here are step-by-step instructions how to do that. Use on your own risk.

1. Reboot your Mac into Recovery Mode by restarting your computer and holding down <kbd>⌘</kbd> + <kbd>R</kbd> until the Apple logo appears on your screen.
2. Click **Utilities** > **Terminal**.
3. Type and run:
	```sh
	csrutil disable
	```
	This disables [System integrity protection](https://developer.apple.com/documentation/security/disabling_and_enabling_system_integrity_protection) as we need to modify/patch system files.
4. Reboot your Mac.
5. If you have macOS 10.15 (Catalina) and above, open Terminal and run:
	```sh
	sudo mount -uw /
	```
6. Get Intel graphics platform ID:
	```sh
	ioreg -l | grep ig-platform-id
	```
	you'll get something like: `"AAPL,ig-platform-id" = <0700260d>`
	
	Remember your ID.
7. Find a "Intel framebuffer" kext in use:
	```sh
	kextstat | grep Framebuffer
	```
	the output will be like `com.apple.driver.AppleIntelFramebufferAzul`.
8. Open respective framebuffer kext file in hex editor: `/System/Library/Extensions/AppleIntelFramebufferAzul.kext/Contents/MacOS/AppleIntelFramebufferAzul`
9. Find your Intel ID from step 6 (mine was 0700260d) and you'll see something like:
	`0700260d 01030403 00000004 00002002 00005001 00000060`
10. Replace `00000060` (1.5Gb) with `FFFFFFFF` (4Gb):
	`0700260d 01030403 00000004 00002002 00005001 FFFFFFFF`
11. Refresh the kext cache and reboot:
	```sh
	sudo kextcache -i /sudo reboot
	```

Done. You have 4GB of video RAM.

