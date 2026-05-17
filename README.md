# MaxOnLinux
### A collection of useful tips and tricks from the Max on Linux thread. 
(no code only tips and tricks)

***

Nikki A
  Feb 23, 2024, 11:45 PM

  - Max 9
  - Arch
  - Steam or Proton.
    - If using steam, i first installed Max using regular Wine and then copy pasted Max files from it to directory of my choice.
    - You only may want to install wine ASIO drivers to proton prefix. Wineasio from aur will probably fail to build, so you need to download latest from github repo and build manually. And then just run wineasio-register on proton prefix.
    - If using steam: prefixes are here /home/{user}/.steam/steam/steamapps/compatdata/{id}/pfx
    - Before using you'll need to open winecfg on your prefix and set proper ui scaling for your resolution. I've set it one point higher than default for 1920x1080
    - To fix interface bugs like missing dropdown focus or unable to input any text to fields, in KDE i fixed it by setting Focus stealing prevention to: EXTREME. 

***

bob p
  Oct 26, 2025, 10:06 PM

  - Max 8 & 9
  - CachyOS (with KDE and Wayland)
  - LUTRIS
    - Installed MAX as a Windows game 
    - Everything "magically" run. 
    - Default CachyOS setting of windows management (no change in focus prevention etc.)

**

Hannes B

Dec 2, 2025, 6:40 AM

I can confirm that Max9 indeed works under Cachy OS. I also tried several different distros, including Debian, Arch, Fedora, but without success. So something is obviously different with the wine packaging under Cachy (I used the "wine" package, not the "wine-cachy" one).
I would love to be able to run Max under a more stable Linux distro, so if anyone can figure out what the Cachy folks did to make it work, I would be very curious to know.
Hannes Brugger's icon

Hannes B

Dec 10, 2025, 7:30 PM

turns out that a major part of the problem was a missing font. after installing ttf-bitstream-vera, Max9 starts also on debian. looks promising, will do some further testing and report here (autocompletion bug is still present but still solvable as mentioned here before)

***

Th8a

Dec 18, 2025, 10:43 AM

There is some detailed guides here in this thread, but some of the info is a bit outdated. It's probably easier now than it used to be.

The important thing is to make sure that you have all of your video drivers installed correctly and in a way that WINE recognizes it via Vulkan/DXVK/ etc. (that's for NVIDIA cards. Not sure about AMD, though they may work out of the box.)

Then, there's a few ways to do it, but I would go ahead and just install Lutris and use that to create a wineprefix and install MAX into it.

The trick to getting it to work with very little to no tinkering from this point on is to choose the wine version well. I have found that newer versions of proton or GE proton with esync/fsync enabled is the best choice as of this post. Wine staging has had mixed results from version to version in my experience.

You'll probably want to open winetricks through lutris and install the microsoft redistributables as well. (vcrun2022 etc.) In fact, I think that is necessary to run the installer if I'm not mistaken...

Now you should be able to just launch MAX through lutris and have it work for the most part.

If you get the glitch where creating objects doesn't let you use the autocomplete function, then you'll need to set your window manager to prevent focus stealing from MAX windows. This is easy in KDE by going to the window/app options in kwin. Someone managed to accomplish this in gnome as well, but you'll have to find that in this thread cause I don't remember how they did it.

That's pretty much it. If it doesn't work after all of that, then make sure all of the prefix and launch options in Lutris are configured correctly, make sure that the prefix is a 64 bit prefix and not a 32 bit prefix, and try different versions of WINE until you get one that works.

Optionally, if you plan to do a lot of audio intensive stuff, you may want to look into installing wineasio. I have found that to help reduce latency and pipe audio through the jack/pipewire environment without too much trouble.

***

woyteg
Dec 28, 2025, 7:57 PM

I finally got max 9 to work on i3, a tiling window manager. (At least some) externals also work, gen~ works(I feel like I had problems with these in the past but maybe i confuse it with FAUST compiled externals).

    I installed max via lutris.

    In Lutris i had to install lutris's own wine (vie preferences->runners->wine), wine-ge-8-26-x86_64 which was the only choice there.

    I played around with 'Esync' and 'Fsync' settings which didnt seem to make a difference.

    At this point max seemed to run fine but couldn't be managed by i3. In the wineconfig, if i switched on the setting that would allow my wm to manage wine windows i would get the focus stealing bug, if i would switch it off max would run but always sit on top of everything else. Additionally, focus would sometimes drop to backround windows and instead of typing in a new object, i would b writing into a browser window that was sitting behind max. Both of these options seemed unusable.

    After tons of trial and error, installing awesome wm and KDE plasma to see if max would run there as described in this thread and thereby messing up my existing i3 a little i finally looked into i3s documentation. In the i3 config, adding just

    no_focus [class="max.exe"]

    Finally solved everything. Audio worked out of the box. Have not tried external audio interfaces yet, am not interested in jitter. Maybe this helps someone. (Max on i3 is quite something.)

For the sake of completeness, my i3 config also has the following lines but I am pretty sure they don't matter (they come from previous tests and i am currently not interested in further tests)

focus_follows_mouse yes
focus_on_window_activation smart

System infos

Linux version 6.1.159-1-MANJARO (builduser@runnervmoqczp) (gcc (GCC) 15.2.1 20251112, GNU ld (GNU Binutils) 2.45.1) #1 SMP PREEMPT_DYNAMIC Sun Dec  7 08:24:53 UTC 2025

i3 version 4.24 

 license manager doesn't work in wine/lutris?

***

woyteg
Dec 29, 2025, 5:37 PM

So, I got the license manager to work under lutris using GE-Proton. I had to install protonup-qt via pacman, use it to install ge-proton. I then in lutris configured the runner for max to use Ge-Proton10-27. Also i am currently using these arguments for max in lutris (but not sure if its important):

--disable-web-security --ignore-certificate-errors --user-data-dir Documents

The other proton version that was available there didn't work with the licesne manager.

When starting max i get a bunch of node for max errors. I didn't have them under the wine runner but i don't really care about them at the moment as everything else seems to work.

Again, writing this for myself and others as some sort of protocol, sorry for the noise.

***

Th8a

Dec 30, 2025, 5:17 PM

Nice work.

From what I have found, GE proton runs Max the best, including the licencing stack.

Previously, I've had to install wininet and a few other network based dependencies in winetricks to get the license stuff working. GE proton seems to have it working out of the box

Any WM should work as long as you can figure out how to set the focus stealing settings per app.

Any distro of Linux also seems to work as long as you can get wine set up and running well, as the setup of MAX seems to hinge more on the wine version and wineprefix configuration than any distro specific libraries or drivers etc.

I'm not a big jitter user myself, but I have tested it and yes it does work as long as you have all of your GPU drivers working properly and set up to jibe with DXVK/VKD3D etc.

It's a whole lot like how you would get any given PC games running, except that the chosen version of wine appears to have more importance than winetricks dependencies beyond vcredistributables or tweaking wincfg settings.

***

bob p
Jan 21, 2026, 11:44 AM

Hi all,

I think to have succesfully closed my long trip to get MAX running on Linux, I will use it from now on.

Windows finally not anymore mandatory (even if I kept a dualboot possibility...)

A brief sumup maybe useful for who is interested.

    OS: CachyOS (the only I found with autocomplete running, but surely other possibility)

    DE: KDE Plasma

    install cachyos-gaming-meta

    Installation of MAX with Wine

    Load the exe as Windows local games in Lutris

    No action required like focus steal etc. , it runs out of the box

    Run MAX inside Lutrix, not from desktop icon or system app launcher

    install all wanted MAX extra packages

    license activation simply from MAX Help - User Account & License- usrn & passw, as for WIN or MAC

Everything runs without any tweaking, including the damned Autocomplete!

***

Vincent F
Jan 25, 2026, 1:21 PM

Hi all ! I’ve been looking at most of the pages of the forum, what a fascinating journey this has been ! Looks like Wine 11 is out since Jan. 13, can some of you tell if this is an big step forward for Max on Linux ? https://www.gamingonlinux.com/2026/01/windows-compatibility-layer-wine-11-arrives-bringing-masses-of-improvements-to-linux/

I’ll be trying to setup a LattePanda Mu to run Max 9 today, with Lutris/GE-Proton/KDE in KUbuntu LTS.


***Update on this, after a long coding day :
- Max 9.1.2 works on my LattePanda Mu (8GB RAM, Lite Carrier). I switched from KUbuntu to CachyOS (like Bob Pell suggested, thanks!)
- Running inside Lutris under ge-proton 8-26.
- WineASIO never worked with this setup, I use ad_directsound with a basic 10-year old Komplete Audio 6 mk1 interface and MIDI controler, and the latency is pretty low !

The license and package managers unfortunately doesn't work though. 

***

Merlin E
Feb 20, 2026, 7:21 PM

Max 9 on Linux (CachyOS + KDE/Wayland + Lutris/Wine) – stable incl. audio (license not working yet)

Hi everyone,

Quick status report from a fresh Linux setup. I’m not sharing personal identifiers, but enough technical detail to reproduce.

System

    OS: CachyOS (x86_64, Arch-based)

    DE/WM: KDE Plasma 6 (Wayland / KWin)

    Kernel: 6.19.x

    CPU/GPU: AMD Ryzen 7 7735U + Radeon 680M (integrated)

    Audio: PipeWire (PulseAudio compatibility layer)

Software

    Max: Max 9 (Windows build)

    Lutris: 0.5.19

    Wine runner: wine-staging 11.2 (x86_64) via Lutris

    64-bit Wine prefix created automatically by Lutris

Installation
Installed Max via Lutris (“Install a Windows game from an executable” → selected the .msi installer).

After reboot I initially got:

    MissingGameExecutableError: The game doesn't have an executable

Fix: manually set the executable in Lutris to:
drive_c/Program Files/Cycling '74/Max 9/Max.exe
(not the .msi).

After that, Max launches reliably when started from Lutris.
What works

    Stable startup from Lutris

    UI usable, no redraw glitches in this session

    Object creation and autocomplete functional (no focus-stealing issue on this setup)

    DSP runs correctly

    Audio output works via PipeWire-Pulse

    Tested with Bluetooth headset (A2DP profile) — higher latency as expected, but stable output

Test patch used:
Multiple cycle~ oscillators (100–1000 Hz), each scaled with *~ 0.1, summed into live.gain~ → ezdac~.
DSP toggled repeatedly, no crashes or instability.

General impression: audio engine and UI were stable during testing.
What does NOT work (yet)

    License activation / License Manager: not working in this setup.

    Package Manager not tested.

    Jitter / heavy jit.gl visuals not tested.

    External audio interface + wineasio not tested.

So this is not a fully validated production setup yet — but as far as basic patching + audio output go, it behaves solid.

***

Rui P
Feb 22, 2026, 9:30 PM

Manjaro + Crossover 26.0 + Max 9.1 = All working in Linux, including license, package manager, autocomplete and jitter

Steps:

0) I used Manjaro, nothing especial about this version, except that is provided by the brand that sells the Linux laptop that I use, so it has the laptop drivers all installed and ready to go. But I guess any stock Manjaro would work.

1) Install crossover 26.0 (I installed it from the distros own repositories, using the standard package manager UI, when you install it's a full functional trial for 15 days, so no risk for anyone to try if they want.)

2) Install Max 9.1 or Max 8.6.5, windows version in crossover 26.0 using the suggested / default settings

3) Instead of using the auto graphics setting use the DXVK graphics setting. If not, at least in my hardware, the UI refresh is broken. See second screenshot for where that setting is. Takes 10 seconds to change.

4) That's it, everything works, as it should. Except for two things I noticed so far.

i) when the autocomplete windows pops, you can scroll with the mouse wheel, but trying to drag the scroll bar closes the window.

ii) the package manager has a slight offset, so you need to click a few pixels bellow the buttons. No such thing happens on the max UI itself. I think its only on webviews (like the license auth as well, but there the buttons are so large you don't really notice.

As for the actual usage:


No crashes, no nothing, it just works fine. Audio just worked with pipewire, no issues, no configuration. Connecting an external USB-C midi keyboard, also no fiddling, all good out of the box. Logging in, no issues, authorized my demo license just fine. Package manager has that UI issue mentioned but I was able to install and use "ease" in about 1 minute. So I would say it works without any real issues, just a slight annoyance. As I said, this is just in the manager, in Max itself the UI is crisp and responsive. No weird font issues either. Audio interface also no issue, connected a Zoom L6, just took 10s to change the output/input devices in the settings and also worked with no issues or troubleshooting.

Jitter seems to run pretty OK, I'm not sure what is the gold standard for testing jitter, but I searched for example jitter patches, and found a site with a bunch of them, in the screenshot you can see one of them, particles_box.maxpat running at about 50fps. Maybe not amazing, but clearly working in my point of view.

***

