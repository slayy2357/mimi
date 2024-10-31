# Jailbreak detection

## Introduction
Supercell use a specific jailbreak detection protocol, that make your game crash if your device is jailbreaked.  
Some tweaks are made to bypass these security, (choicy, vnode bypass), but Supercell is still able to detect them.
However, as this detection happens on the client side, I will analyze it further.  

## Note
For now, I will focus on other points because a tool already exists to patch game executables and bypass jailbreak detection. Nonetheless, I will document the security aspects, as this patch works well to prevent crashes and evade most detections in Supercell games. However, from my analysis, it doesn’t seem to bypass every security measure, so Supercell might still be able to detect if your device is jailbroken. [The tool](https://github.com/risporce/Supercell-jailbreak)