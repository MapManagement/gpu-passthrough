# Games

## Overall
Since I started this whole project and was able to fully use my gaming guest, I played quite some games.
Most games worked immediately as if I played on a typical computer. None of them crashed just because I
played within a virtual machine and I couldn't recognize any difference from playing games on a native
Windows build. It is safe to say than you can play nearly all of your games perfectly fine within a guest
that uses a dedicated GPU. Only if you want to use some more special input devices like a controller for
example you have to invest more time.

## Specific Games

### Rage 2
I tested [Rage 2](https://bethesda.net/en/game/rage2) in the first Windows VM that I created and had no
big issues at all. At some parts of the game, I noticed some weird flickering but I don't know what caused it.
Either it is bug of the game, it was meant as a feature (the locations and scenes would have matched to
that flickering) or it is because of my VM. Either way, I'm really happy about the result and cannot complain
about the performance. More detailed test are already planned and will be added to this sectionb aswell.

### Overwatch
[Overwatch](https://playoverwatch.com/en-us/) was the second game I tested. Compared to Rage 2 it's
not a big deal for mid to high-end machines at all. Getting constant 60 FPS should not be a problem
for most modern pcs but since I'm using a 144Hz monitor for all of my games, I needed to reach those
144 FPS. Everything I can say so far is, that Overwatch runs flawlessly as well. The virtual machine
has enough  power to run the game at the constant 144 FPS I need and I couldn't recognize any other
issues like weird flickering or somehting similar. Apart from playing Overwatch most of the time
when I played somehting at that time, I chose that game because I've never heard of any probelems
with its anti-cheat software. It also runs under Linux if you're using [Proton](https://www.protondb.com/)
, therefore I had no worries about getting banned, kicked or whatever. To sum it all up, I can
only tell about good experiences with Overwatch in a virtual machine.

### Chivalry 2
Recently I started playing [Chivalry 2](https://chivalry2.com/) and I really enjoyed playing it within
my Windows guest. I didn't experience any bugsor crashes and played with almost the same FPS like on my
native installation. I was also able to use my host keyboard because of
[Barrier](passthrough/extra/mouse-sharing.md#Barrier). But be aware, you can't use your host mouse if you
don't want to play with very weird mouse movements. It only happened to me when I was playing a game in
fullscreen but I'm fine with using another mouse for games as long as I'm able to use only mouse for desktop
stuff.

## Exceptions

### Valorant
[Valorant](https://playvalorant.com/en-us/) does not run within a Windows guest, at least not without trying
to surpass their anticheat called "Vanguard". Vanguard needs to be loaded at boot time and cannot be initialized
in a virtual machine. Some people were able to play Valorant but only by modifying their guests in a very specific
way which also lead to bans in some cases when Vanguard detects these workarounds. For a short period a workaround
that worked flawlessly was available. You were able to play Valorant respectively load Vanguard. Cheating wasn't
simplified by this techniques, nonetheless Riot decided to introduce a patch that kinda killed the workaround. If
you want to know more about it, watch [this video](https://www.youtube.com/watch?v=L1JCCdo1bG4).

### Escape From Tarkov
[Escape From Takov](https://www.escapefromtarkov.com/) or in short "EFT" uses [BattlEye](https://www.battleye.com/)
as its anticheat. In mid 2020 an update was released which probably lead to a better VM detection of BattlEye for
EFT. As a result many player using a KVM for playing EFT, weren't able to join a server anymore. Since then EFT is
one of the games which cannot be played within a virtual machine.

### Tom Clancy's Rainbow Six Siege
For resons of simplification I'll call the game [Tom Clancy's Rainbow Six Siege](https://www.ubisoft.com/en-us/game/rainbow-six/siege)
only "Rainbow Six". So Rainbow Six also uses BattlEye and cannot be played within a VM for a similar reason. Of
course there are some workaround to hide anything that reveal the virtual machine. By the way, the same workaround
which worked for Valorant one or two weeks also worked for Rainbow Six. In this case it seems a bit easier to run
Rainbow Six in a VM nowadays but be careful when trying so, you always risk a game ban.

### Genshin Impact
Another game that cannot be run that easy within a virtual machine is [Genshin Impact](https://genshin.mihoyo.com/en).
The current workaround seems be not very difficult but you have to adjust your guest config in order to play the game.
Some workarounds in [this Reddit thread](https://www.reddit.com/r/Genshin_Impact/comments/j0blm4/genshin_impact_and_virtual_machines/?utm_source=share&utm_medium=web2x&context=3).

