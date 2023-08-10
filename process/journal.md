# Process Journal

## Thursday, 27 July 2023

Somewhat hopelessly I'm writing this weeks after the original act of making the game, which happened on the 13th of July. I went on vacation semi-soon after than and was busy getting ready etc. I'm so, so sorry.

Anyway, I have some faith I can still explain the game because it is not very complex.

Essentially I grabbed a Super Mario Bros. ROM file – and let's say that I have the cartridge at home and so that was totaly fine – and then I messed around with it to try to turn it into nothing.

I started by just opening it in a text editor and deleting everything in the file, and then tried to run it in an emulator. It didn't run - the file was described as corrupted. Which spurred me into realizing I did want the game to actually run, while being nothing... or at least least a nothing-ing of Super Mario Bros.

So then I tried deleting everything except the NES (text) at the leading end of the file. But that was also broken.

So then I went and actually looked up the NES rom format specifications here: <https://www.nesdev.org/wiki/INES> -- or rather I suppose this is the format the emulators use. Which is a thing in itself - so really this project is about the erasure of an emulator-oriented copy of the game. Does that matter? Well it would if I wanted to run it on a cartridge - then I'd have to figure out how to do the format for a cartridge instead of an emulator, but that's not what I'm doing right now. (Though I would kiiind of like to.)

So with that bit I understood that there are some more elements that matter, most obviously that it seems like you have to have *data* for the whole program even if there's no information - i.e. you need to zero out all the bytes rather than delete them...

And so I ended up with a version where I kept the first 7 bytes of the header data from the ROM but zero-ed out everything else. And that runs in OpenEmu, launching to just a green screen which I take to be some kind of default. Is that a default for the emulator or a default for the machine? I couldn't say. I should really try to run it in multiple emulators to get a sense of that - there's a way in which (like the Nothings Suite) this is about seeing an underlayer of what a game "can be" on a platform.

However I've just been poking at it again and can get away with 0 bytes for everything except the NES leading header bit (literally the characters N E S and a carriage return) - this will let the ROM run okay.

Essentially that's just having the header indicate the ROMs program length and "CHR ROM" are both 0.

But that makes me think... shouldn't I be able to create a smaller ROM file in that case, that's essentially just the header and nothing else? Or do I actually have to have all the other 0 data in there for it to count?

So I'm currently trying to create a hex file like that, with some difficulty... let me try again.

Okay well yes, I created a file from scratch called `test.rom` with the Mac hex editor [Hex Fiend](https://hexfiend.com/) and added only the 16 bytes of the header, and with that only the NES leader and then 00s. And it ran in OpenEmu. When I subtracted anything from this it was corrupt because presumably the header is mandatory. But it seems like it works in the sense that you don't need the zero'ed out data if your header specifies a zero-length program and data.

**HOWEVER** now I am at a crossroads. I could

1. Stick with the version I have in the repo right now, which is the original ROM of Super Mario Bros. with the data zero'ed out.

2. Do that, but delete all the zero'ed data (and update the header accordingly).

Which one of those "makes the most sense" in terms of the project? Some of this is totally academic, okay all of it is, but I suppose the question is whether "nothing-ing" a game like this is better represented as 0s where the data was (so there's a record of it in some sense - a trace of where the data for the game was) or better represented as the deletion of that data entirely from the program.

Is there even an answer to that? Should there be both versions?

Obviously this is ultra process stuff I'm talking about here, none of which particularly matters except that it's a way to talk about file formats, the idea of the "trace" of a program after it's gone...

And on traces, too, there's the thing that I don't really want to have the original ROM (with SMB in it) in the repo - that seems sketchy of course - but that means there isn't the historical trace of "here's the ROM with the game, now here it is erased" - so there's a kind of faith thing going on, where I should be trusted to have had the original data, the real thing, and then erased that. Rather than having created a new empty ROM which would essentially just represent ANY NES game that has then been zero-ed out. Now there's no difference at all except that I'm saying I zeroed/deleted/nothing-ed a specific game - that I had that game's data as my starting point. Which I did. And which seems to matter to me.

In amongst this we're also talking about [Cory Archangel's *Super Mario Clouds*](https://whitney.org/collection/works/20588) which does a "deleting everything except" kind of idea (though see [Patrick Lemieux's video about this](https://vimeo.com/241966869)).

And then there's [Rauschenberg's *Erased de Kooning Drawing*](https://www.sfmoma.org/artwork/98.298/) in which Robert Rauschberg took a de Kooning pencil drawing and rubbed it out, and then called that a new artwork. I think there are some of the same questions/ideas active - except that in that more analog setting I wonder if there's a more interesting sense of history and trace involved in "this is the actual piece of paper" that digital files just don't have. Maybe I should have done it on "the blockchain" or something.

Anyway, I suppose I'm leaning toward providing both versions - the zeroed version and the deleted version. I'll do that now.

Well I had the nice realization that I can follow a kind of convention from NES roms online which is having a bracketed letter after the game name to tell you about what version or the game it is (e.g. E for Europe, U for US, J for Japan I think?). This gives me some nice room for the two versions idea because I can have `Super Mario Nothing (Z).nes` for the zero-ed version and `Super Mario Nothing (D).nes` for the deleted version.

Making the deleted version from scratch just now was easy with Hex Fiend obviously - just grabbed the original, deleted everything except the header, changed the header to reflect that.

Making the zeroed version has actually been really time consuming in a way I think I like? I'm 0ing each byte individually. Which I can do by holding down the 0 key (after the header obviously). It proceeds at a rate specific to my computer's key repeat rate (I assume? I could speed that up then?) and has at least some relationship to the manual nature of erasing something manually rather than the "instantaneous" nature of a lot of computer operations. So I'll do it this way.

The other thing about the Rauschenberg, which is brutally obvious and really important, is that it's a singular object. There's only one of the drawing and when it's erased it's gone. Not true for the ROM. So that's not really part of it, but I do think that there's "aura stuff" and there's the idea of "Oh, so that's what you see when the game is gone" a bit too.

I did increase the key repeat. This puppy's gonna take a while though.

<video width="100%" controls>
<source src="videos/zeroing-super-mario-bros.mp4" type="video/mp4">
Uh oh. There's meant to be a video of zero-ing out the game here.
</video>

## Zero-ing -- Monday, 31 July 2023

So as it turns out I'm *still* zero-ing out the (Z) version of the game. Which I actually find quite fun and funny. Some thoughts:

- For the commit I'm about to make (with this entry) I specifically added the file to the `.gitignore` to avoid having any of the original game's data in the repo. In my mind this functions as a (not necessarily very credible, but something) proof that this file is being manually zero-ed and did indeed contain the original data (and parts thereof).
- I did spend time thinking about *how* to zero the file. There are at least two key ways: (1) automate it somehow to just replace all the data except the header with 0s - something that could take like 15 seconds to work out the regular express (or whatever) and then done. (2) manually zero out all the data by editing the file directly.
- I'm going with (2) because (1) feels somehow too close to just generating an empty file of zeroes and because it goes in what appears to be a single moment (not really true at the level of computation and maybe I'm just totally kidding myself on this).
- Also I think I just like how manual (2) is - it feels durational even though I'm holding down the 0 key and relying on the key repeat of the mac (which I sped up to max) to actually fill in the zeros. But still, I feel like it's more like the gradually erasing of a drawing than just it being deleted. Again here there's a "weird" thing about the fact that while I'm incrementally zeroing it
  - (a) is in a buffer so it's not actually being committed to disk
  - (b) ... something else?

So yeah I'm happy to be slowly zero-ing out this file "by my own hands" and it will be done when it's done. Doing a bit a day. It's also kind of meditative to do because I can't use my computer for anything else so there's a kind of attention/inattention thing - I can relax while I zero.

## Continued zero-ing (2023-08-02)

It's actually quite relaxing. Makes me think a bit of meditation, which is also fun in terms of "clear your mind" corresponding to the gradual deletion of data in a file. Makes me think a bit of a game/app where that's the representation/motivation to consider yourself meditating - the various forms of waiting that OSes provide to us. Progress bars, most obviously. Spinning beach balls.

Anyway, zeroing.

## Zero-ing, the different between code and image data? (2023-08-10)

Good to be zero-ing a bit more. Felix did a few lines, so both he and Chip have contributed some zeroes.

One thing that's noticeable (I assume/think) is that lower down in the file it all looks more rhythmic and patterns - repeated hex codes etc. - which I imagine is the image data/sprite stuff for the game. So it *feels* like I'm deleting something different in that moment.

Aaaand... it's done. Let me make sure it still runs. It does. Mission accomplished.
