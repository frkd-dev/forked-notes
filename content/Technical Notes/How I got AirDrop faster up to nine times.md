---
share: true
date: 2024-11-23
tags:
  - macos
  - iphone
  - airdrop
  - wifi
---
*A small story of how disconnecting from a Wi-Fi access point helped me significantly boost AirDrop transfer speeds.*

One day, I was streaming Spotify via AirPlay to my audio/video receiver from my iPhone 13 Pro. At the same time, I decided to transfer a significant 10 GB of over 190 videos and images that I had captured during our recent family vacation from my iPhone to my MacBook Pro M2.

As soon as I started the transfer, I quickly noticed that it was remarkably slow, even though both devices were in close proximity. I opened the system's Monitor app to check the network statistics.

![How I got AirDrop faster up to nine times.png](how-i-got-airdrop-faster-up-to-nine-times.png)

A mere 10 MB/sec is quite disappointing, especially considering that AirDrop uses peer-to-peer communication, which bypasses routers. Both devices should theoretically be able to achieve much faster speeds with their 2x2 MIMO Wi-Fi 6 chips. So, what could be the problem?

Based on my experience in optimizing home Wi-Fi routers using OpenWRT, I know how sensitive Wi-Fi can be. Even slight misconfigurations of Wi-Fi hardware or radio channels can drastically reduce your transfer speed. It's possible that the ongoing AirPlay stream was the culprit, consuming bandwidth. After I disabled AirPlay, the AirDrop transfer speed tripled.

![How I got AirDrop faster up to nine times-1.png](how-i-got-airdrop-faster-up-to-nine-times-1.png)

This confirms that my assumptions were correct. Since we didn't achieve the full potential of 2x2 MIMO Wi-Fi 6, what should we do next?

As I mentioned earlier, the AirDrop connection is peer-to-peer, which means that the MacBook needs to maintain two connections simultaneously: one to my iPhone via AirDrop and another to the Wi-Fi router for regular network communication. What if I disconnect the unnecessary connection for the transfer? Let's disconnect both the iPhone and MacBook from the router while keeping Wi-Fi enabled on both devices.

![How I got AirDrop faster up to nine times-2.png](how-i-got-airdrop-faster-up-to-nine-times-2.png)

Wow! The entire Wi-Fi bandwidth is now dedicated to the AirDrop transfer, and it has reached the theoretical limit of 2x2 MIMO Wi-Fi 6.

That's what I was looking for. ∎