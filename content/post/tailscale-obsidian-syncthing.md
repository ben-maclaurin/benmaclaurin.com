+++
title = "Tailscale, Obsidian and Syncthing"
date = "2022-08-01T20:58:33+01:00"

#
# description is optional
#
# description = "An optional description for SEO. If not provided, an automatically created summary will be used."

tags = ["development","networking",]
+++

I'll preface this by mentioning I'm no expert in any of the three tools referenced. Apologies in advance for any oversight.

## Background

Last month I setup a copy of [TiddlyWiki](https://tiddlywiki.com/) (a self-hosted knowledge management tool) on a small Linode instance. TiddlyWiki comes with batteries included and a painless setup. With some further tweaks and the help of [Tailscale](https://tailscale.com/) (a simple VPN solution), I had an access anytime/anywhere personal and private knowledgebase. TiddlyWiki displays well on mobile, too. 

But... I missed [Obsidian](https://obsidian.md/). Also, hosting a relatively lightweight TiddlyWiki on a 1GB VM felt a bit wasteful. A very convenient [PKM](https://en.wikipedia.org/wiki/Personal_knowledge_management) for the price of a coffee/mo. isn't bad, but I couldn't help shopping around for alternative setups.

## Obsidian Sync 

Obsidian offer [Obsidian Sync](https://obsidian.md/sync) for $10/mo. Whilst I understand the importance of supporting great products, a few limiations of Obsidian Sync stand out to me: 

> We keep version histories of markdown files for up to one year before we clean it up. Attachments are kept for two weeks.

I am sure the team at Obsidian take great care to keep their user data secure, but I always prefer self-hosted solutions where the compromise on convenience is mangageable.

> We keep data in your remote vaults, including version history, for one month after your subscription expires.

This would suggest that if my subscription expired for some fluke reason, I'd be out of luck. Sure, provided my devices were intact I could just restore the vaults locally, but I don't want to take unnecessary chances. Furthermore, I'm not keen on reliance to a service that could increase in price overtime. 

## Best of both worlds

Enter Tailscale and [Syncthing](https://syncthing.net/). Here is a brief description of Syncthing from their website:

> Syncthing is a continuous file synchronization program. It synchronizes files between two or more computers in real time, safely protected from prying eyes.

Syncthing, therefore, allows me to maintain Obsidian vault parity across multiple devices (4 in total). Tailscale is not strictly necessary for this setup, as Syncthing handles encrypted synchronisation, but I still use Tailscale for 2 reasons:

1. True P2P synchronisation. As described in [this post](https://console.dev/articles/private-p2p-encrypted-file-sync-syncthing-tailscale/), Tailscale enables you to leverage Syncthing without using their relay system or global discovery service. I don't understand the intracacies of how this works, so please check out the linked post for more info.

2. Better iOS support. Unforuntately not 100% hassle-free (see caveats below). 

## Important caveats

### Limited iOS support

At the time of writing, only [one (unofficial) app](https://apps.apple.com/gb/app/mobius/id1453744762) is available for Syncthing on iOS. To circumvent this problem, I use [Taildrop](https://tailscale.com/kb/1106/taildrop/), an alpha feature of Tailscale which allows for pretty seamless* file transfers between devices. This is a manual process, however, so definitely rough round the edges. Fingers crossed Syncthing will release an iOS app in the future.

(*I have only been using this feature for a month. So far I have no complaints.)

### Works best with always-on solutions

Both target and destination devices need to be connected to Syncthing and Tailscale for synchronisation to work. Fortunately for me, I have a spare Raspberry Pi that runs on my home network. This acts as an intermediary, allowing for resyncs between devices at anytime.

## Update 28/10/2022

I have since switched from Obsidian to GNU Emacs and org-mode.