---
title: 'swap, zram, zswap, random linux stuffs'
thumbnail: https://raw.githubusercontent.com/audacioustux/paste/master/Screenshot_20200204_013440.png
date: 2019-12-30 10:29:56
comments: true
categories:
- linux
tags:
- zswap
- zram
- config
- tools
- tips
---

#### **context**: `Linux Bangladesh` facebook group

---

1. #### **swap, swapfile, zswap, zram**
র‍্যাম যতই হোক, almost-always-add-swap-space - https://haydenjames.io/linux-performance-almost-always-add-swap-space/

##### **কিন্তু কত GB?**
* hibernate (suspend on disk) ফিচার চাইলে র‍্যামের সাইজ + ১ জিবি
* যদি হাইবারনেট করার চিন্তা ভাবনা না থাকে:
    - hdd হইলে: দরকার নাই (পরে কাহিনী খুলে বলতেছি)
    - ssd হইলে: 1 GB

##### **ক্যামনে বানাবো?**
```
$ sudo su
$ dd if=/dev/zero of=/swapfile1 bs=1G count=<swap_size>
$ chown root:root /swapfile1; chmod 0600 /swapfile1
$ mkswap /swapfile1; swapon /swapfile1
$ echo "/swapfile1 none swap sw 0 0" >> /etc/fstab
```
explanation: 1) switch to root user, 2) হিউজ একখান ফাইল, যাতে কোনো হোল নাই -_- replace `<swap_size>` with যত জিবি সোয়াপ চান (ex: 1,2,3...) 3) set permission for the file 4) file টাকে সোয়াপ হিসাবে ব্যবহার উপযোগী করার জন্যে 5) swapfile টা প্রতি স্টার্টাপে অটোম্যাটিকালি সোয়াপ হিসাবে মাউন্ট করার জন্যে...

##### zswap, zram কিতা?
সোয়াপ আসলে কি হয়? 

    "memory pages which are hardly ever used" 

তো সেই মেমোরি swapfile এ না রেখে মেমোরীতেই কম্প্রেস কইরা রাখলেই তো হয় 🤔 সুবিধা? hdd/ssd র‍্যামের থেকে অনেকটা স্লো। হার্ডলি এভার ইউজ্ড মানেতো এই না যে কখনোই ব্যবহার হবেনা? আর যদি র‍্যাম অনেকটা ব্যবহার হয়ে যায়, তখন যাস্ট কিছুটা কম ব্যবহার হইতেছে এমন মেমোরীও সোয়াপ হয়ে যাইতে পারে। কম্প্রেস করে র‍্যামেই রেখে দিলে ram এর একটা অংশ সোয়াপ স্টোরেজের মত কাজ করতেছে 😌 zswap, zram এর কাহিনী ইহাই :) 
zswap কম্প্রেস করে মেমোরিতেই রাখতেছে, কিন্তু যখন মেমোরিও ফুল হয়ে যাওয়া শুরু করবে, মেমোরিতে কম্প্রেস হওয়া ডাটা আসল সোয়াপ স্টোরেজে মুভ করা শুরু করবে... 
zram এর ক্ষেত্রে মুভের কোনো কাহিনী নাই :) বাকিসব zswap এর মতই। তো zram এর সাথে swapfile/partition রেখে লাভ নাই।

##### **তো কখন কোনটা? zram or zwap?**
hdd হইলে যাস্ট zram. ssd হইলে zswap 👌
এর জন্যেই একেবারে প্রথমে বলছিলাম hdd তে swap এর দরকার নাই।

##### **কিন্তু ক্যামনে?**
একেক ডিস্ট্রোতে একেকভাবে zram, zwap enable করে। manjaro, pop_os এই দুইটার টা কমেন্টে দিতেছি

2. #### **personal preference:**
**distro**: manjaro kde
**package manager**: snap -> flatpak -> pacman -> yay
**terminal emulator**: অনেক অনেক ঘুরাঘুরির পর kitty ( https://github.com/kovidgoyal/kitty ) তে শান্তি পাইলাম.
**config**: https://gist.github.com/audacioustux/4967b2c6d25a004c394455a95f676508 (follow the README file)
**shell**: zsh with starship config: ^
**terminal** editor: emacs with spacemacs
**config**: https://gist.github.com/audacioustux/1d4aea266047e3efec5f4796441ae1af

3. #### **tools:**
**ripgrep (like grep)**: https://github.com/BurntSushi/ripgrep
**bat (like cat)**: https://github.com/sharkdp/bat
**fkill**: https://github.com/sindresorhus/fkill-cli
**fd (like find)**: https://github.com/sharkdp/fd
**ag(like ack)**: https://github.com/ggreer/the_silver_searcher