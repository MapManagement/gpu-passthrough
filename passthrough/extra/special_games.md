# "Special Games"
## Problem
Some anticheats of specific games do check whether you are running a game within a virtual machien or not. I don't know
how each of those anticheats detects that but here are some games that were either not playabale at all or only if you
put too much effort into it without knowing if the game will work properly at the end:
- Valorant because of its anticheat Vanguard
- Escape from Tarkov
- Genhsin Impact
- Rainbow Six Siege because of BattlEye
Vanguard could not be installed or started in a virtual machine and Rainbow Six Siege would kick or even ban you from
the game. I don't know much about the behavior of the other two games but I heard quite often that only a small amount
of people which were really into playing those games in a KVM managed to play them.  
Fortunately, there is a kind of a workaround for them since March 2021 and I already tested Valorant using it. I can say
that I was able to run Vanguard and play Valorant without getting kicked. Further tests are in the making and will be 
added to this file as soon as they are finished. Certainly I will also explain how this "workaround" works.

## Workaround
[Use these videos for now](../../sources.md#SomeOrdinaryGamers)  
Unfortunately, the workaround explained in the video above does not work anymore. It seems like Riot Games decided to
develop a patch against that method. It happened only 1 or 2 weeks after the first posts and videos about the workaround were publicly available. Eventhough you were not able to bypass Vanguard just by running Valorant in a virtual machine,
Riot obviously did not want players to use any kind of virtual machines and took action against the very popular
workaround. I really want to mention that the main reason, why players were able to play Valorant in a virtual machine, was
Windows' [nested virtualization](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/user-guide/nested-virtualization).