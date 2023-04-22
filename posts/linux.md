# Jumping into the Rabbit Hole That Is Linux

This blog is a collection of stories, updates, and in the worst case, rants about my Linux experiment.

---

## I Purchased Thinkpad Laptop
> Date created: April 21, 2023

Before starting this experiement, my one and only machine was a HP Spectre x360. I don't know the specific model by heart, but it's safe to say that it was definitely built for Windows. This Linux experiment isn't my first; around two summers ago I decided to try Linux out and after some major close-calls (i.e. breaking everything), I had dual-boot Ubuntu and Windows 10 on the Spectre. 

But unfortunately, there were a lot of issues. A lot of the hardware didn't have drivers available on Ubuntu. Camera, mic, and speakers were a no go. Coupled that with my lack of Linux knowledge, I had no idea what I was doing and I ended up switching back to Windows-only. Then a couple months later, I attempted to install Linux again on the Spectre. No luck either.

**But now, I have a THINKPAD.** I was in the market for a new laptop mainly because I was getting bored of the Windows and Spectre (ah, first-world problems). I wanted to give Linux another shot, so given my past experience, I was in the market for a Linux laptop. I initially had my wallet set on System76 but got sucked into the rabbit hole of consumer electronics and Reddit, so I spent a good two days just browsing the internet for what laptop I should get. It was either a $1000+ brand new laptop with the latest specs or a cheap (but still popping) Thinkpad. The key questions were "what will I do with the laptop", "can I upgrade it", and "is it worth the money".

Long story short and skipping over a lot of details, I won a bid on Ebay and paid ~$200 for Thinkpad T480 with a 8th generation i7-8650U Intel Core CPU, 8GB RAM, and 256GB SSD. Despite the bad specs, it's a pretty darn good price and I can always upgrade the laptop if I need to.

## "Deep" Cleansing and PopOS! Installation
>  Date created: April 21, 2023

# Plan

-[X] Clean the laptop
    - [official guide](https://pcsupport.lenovo.com/us/en/products/laptops-and-netbooks/thinkpad-t-series-laptops/thinkpad-t480-type-20l5-20l6/20l5/solutions/ht035676-keeping-your-computer-clean-notebooks-all-in-one-desktops-tiny-in-one-and-monitors)
    - [remove the keyboard](https://www.youtube.com/watch?v=0v-WO9VANyE)
        - snap from the black clip
-[ ] Specs check (as-is)
    - [video](https://www.youtube.com/watch?v=3hFmIVqpG6s&list=LL&index=2)
    - thunderbolt firmware: 20.0 
    - install lenvono vantage
-[ ] Install Linux!
    - Decide if I want dual boot or not.. (nah, linux all the way)
    - Live disk creation by flashing onto usb


very weird issue with balena
- destroys the partition of the usb drive
- makes it so that the system can't find the drive any more, disk is not allocated to the drive
- had to allocate volume, give it a name with the disk manager and minitool
- finally got the volume and formatted to fat32
- changing to rufus

- gpt format [video](https://www.youtube.com/watch?v=s80HI2krsNA)
- the drive works after running those commands with diskpart
- balena etcher has to run as admin so that it can write the usb
- now I can boot up through the usb

how to change password
```
sudo passwd brainuser5705
```

figure out what the ssh thing is with github
can i edit files remotely????


