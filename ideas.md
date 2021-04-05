# Ideas
## MacOS VM
As soon as I set up everything to create a new stable and working virtual machine for windows, it is not that hard to also create a MacOS vm. For now, I do not use any software that is only for usable within MacOS but it cannot hurt at all if I got MacOS installed too.

## Synergy
Some of you might already heard of [Synergy](https://symless.com/synergy).
[LTT made a video](https://www.youtube.com/watch?v=EozeSDeV3Vo) about some similar software, its called
[sharemouse](https://www.sharemouse.com/) but only lets you use one mouse and keyboard between Windows and Mac machines.
Since I'm also using Linux as my host operating system, I need a software that runs on Linux too. Synergy allows me
to do so but unfortunately it's not free like sharemouse. However, it allows me not only run multiple operating system, it also offers me the opportunity to use multiple systems with only one keyboard and mouse. If installed correctly, I
could easily share files between them too. It would definitely improve my workflow when I have to work with Windows and Linux at the same time. 

## Looking-Glass
In the past UI missunderstood the features and purpose of [looking-glass](https://looking-glass.io/). I thouht that it's
simply just the free version of Synergy and I could use one mouse and keyboard for multiple VMs with their own graphics
card passed through. But obviously it's made for another use case: You can run KVM with a passed GPU with only one
display, mouse and keyboard. The dedicated card has to be connected to any monitor or a monitor dummy (could also be
the monitor your host card is connected to) and then you're able to mirror that disaply to your so called 
looking-glass-client. Now you can access the VM screen on your host like it was a Window which runs in fullscreen mode
if desired. It really helps you if you only want to use one monitor for any reasons. I already tested it but had some
performance problems. Maybe I'll create an extra file for my experiences with it.

## Dual Boot Guest
I recently heard about the possibility that you can easily boot into an image of your VM directly,
similar to an actual dual boot. So making changes wihtin the VM would also be applied when booting directly
into it and the other way around. That fact would really make the whole setup a bit more comfortable
since you're always able to start e.g. your Windows independently whether your VM has a problem or not. I heard
about this the first in [this video](sources.md#Niteshade). So as far as I know now, I need to install the Windows VM
either on its own drive or a separate parition on one of my drives. Thus it should be possible to boot directly into
the image. Games that would detect my VM should be playable if I do it like this unless the hypervisor does not cause
any other problems. Tests will probably provide the most accurate answers.