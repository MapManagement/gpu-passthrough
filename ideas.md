# Ideas
## MacOS VM
As soon as I set up everything to create a new stable and working virtual machine for windows, it is not that hard to also create a MacOS vm. For now, I do not use any software that is only for usable within MacOS but it cannot hurt at all if I got MacOS installed too.

## Looking Glass
Some of you might already heard of that software, [LTT already made a video about it](https://www.youtube.com/watch?v=EozeSDeV3Vo). It allows me not only run multiple operating system, it also allows me to use both systems with only one keyboard and mouse and also to easily share files between them if configured so. It would definitely improve my workflow when I have to work with Windows and Linux at the same time.

## Dual Boot Guest
I recently heard about the possibility that you can easily boot into an image of your VM directly,
similar to an actual dual boot. So making changes wihtin the VM would also be applied when booting directly
into it and the other way around. That fact would really make the whole setup a bit more comfortable
since you're always able to start e.g. your Windows independently whether your VM has a problem or not. I heard
about this the first in [this video](sources.md#Niteshade). So as far as I know now, I need to install the Windows VM
either on its own drive or a separate parition on one of my drives. Thus it should be possible to boot directly into
the image. Games that would detect my VM should be playable if I do it like this unless the hypervisor does not cause
any other problems. Tests will probably provide the most accurate answers.