# MaxOnLinux
### A collection of useful bits and pieces from the Max on Linux thread. 

Nikki A
  Feb 23, 2024, 11:45 PM

  - MAX 9
  - Arch
  - Steam or Proton.
    - If using steam, i first installed Max using regular Wine and then copy pasted Max files from it to directory of my choice.
    - You only may want to install wine ASIO drivers to proton prefix. Wineasio from aur will probably fail to build, so you need to download latest from github repo and build manually. And then just run wineasio-register on proton prefix.
    - If using steam: prefixes are here /home/{user}/.steam/steam/steamapps/compatdata/{id}/pfx
    - Before using you'll need to open winecfg on your prefix and set proper ui scaling for your resolution. I've set it one point higher than default for 1920x1080
    - To fix interface bugs like missing dropdown focus or unable to input any text to fields, in KDE i fixed it by setting Focus stealing prevention to: EXTREME. 


bob p
  Oct 26, 2025, 10:06 PM

  - Max 8 & 9
  - CachyOS (with KDE and Wayland)
  - LUTRIS
    - Installed MAX as a Windows game 
    - Everything "magically" run. 
    - Default CachyOS setting of windows management (no change in focus prevention etc.)


