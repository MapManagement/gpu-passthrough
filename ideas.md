# Ideas
## MacOS VM
**WORK IN PROGRESS**

## Synergy
Some of you might already heard of [Synergy](https://symless.com/synergy).
[LTT made a video](https://www.youtube.com/watch?v=EozeSDeV3Vo) about some similar software, its called
[sharemouse](https://www.sharemouse.com/) but only lets you use one mouse and keyboard between Windows and Mac machines.
Since I'm also using Linux as my host operating system, I need a software that runs on Linux too. Synergy allows me
to do so but unfortunately it's not free like sharemouse. However, it allows me not only run multiple operating system,
it also provides me the opportunity to use multiple systems with only one keyboard and mouse. If installed correctly, I
could easily share files between them too. It would definitely improve my workflow when I have to work with Windows and
Linux at the same time.  
**Update:** I already created a distinct file about [mouse sharing](passthrough/extra/file_mouse_sharing.md). I'm not
using Synergy but a fork of it called [Barrier](https://github.com/debauchee/barrier).

## Looking-Glass
In the past I missunderstood the features and purpose of [looking-glass](https://looking-glass.io/). I thought that it's
simply just the free version of Synergy and I could use one mouse and keyboard for multiple VMs with their own graphics
card passed through. But obviously it's made for another use case: You can run KVMs with a passed GPU with only one
display, mouse and keyboard. The dedicated card has to be connected to any monitor or a monitor dummy (could also be
the monitor your host card is connected to) and then you're able to mirror that display to your so called 
looking-glass-client. Now you can access the VM screen on your host like it was a Window which runs in fullscreen mode
if desired. It really helps you if you only want to use one monitor for any reasons. I already tested it but had some
performance problems. You can find more about it [here](passthrough/extra/file_mouse_sharing.md).

## Dual Boot Guest
I recently heard about the possibility that you can easily boot into an image of your VM, similar to an actual dual boot.
So making changes wihtin the VM would also be applied when booting directly into it and the other way around. That fact
would really make the whole setup a bit more comfortable, since you're always able to start e.g. your Windows guest 
independently whether your VM has a problem or not. First time I heard about this in [a YT video](sources.md#Niteshade).
As far as I know now, I need to install the Windows VM either on its own drive or a separate partition on one of my drives.
Thus it should be possible to boot directly into the image. Games that would detect my VM should be playable if I do it
like this unless the hypervisor does not cause any other problems. Tests will probably provide the most accurate answers.