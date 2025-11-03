# Strudel Making Sound
# Samples

Samples are the most common way to make sound with tidal and strudel.
A sample is a (commonly short) piece of audio that is used as a basis for sound generation, undergoing various transformations.
Music that is based on samples can be thought of as a collage of sound. [Read more about Sampling](https://en.wikipedia.org/wiki/Sampling_%28music%29)

Strudel allows loading samples in the form of audio files of various formats (wav, mp3, ogg) from any publicly available URL.

# Default Samples

By default, strudel comes with a built-in âsample mapâ, providing a solid base to play with.

```
s("bd sd [~ bd] sd,hh*16, misc")
```

Here, we are using the `s` function to play back different default samples (`bd`, `sd`, `hh` and `misc`) to get a drum beat.

For drum sounds, strudel uses the comprehensive [tidal-drum-machines](https://github.com/ritchse/tidal-drum-machines) library, with the following naming convention:

| Drum | Abbreviation |
| --- | --- |
| Bass drum, Kick drum | bd |
| Snare drum | sd |
| Rimshot | rim |
| Clap | cp |
| Closed hi-hat | hh |
| Open hi-hat | oh |
| Crash | cr |
| Ride | rd |
| High tom | ht |
| Medium tom | mt |
| Low tom | lt |

More percussive sounds:

| Source | Abbreviation |
| --- | --- |
| Shakers (and maracas, cabasas, etc) | sh |
| Cowbell | cb |
| Tambourine | tb |
| Other percussions | perc |
| Miscellaneous samples | misc |
| Effects | fx |

Furthermore, strudel also loads instrument samples from [VCSL](https://github.com/sgossner/VCSL) by default.

To see which sample names are available, open the `sounds` tab in the [REPL](https://strudel.cc/).

Note that only the sample maps (mapping names to URLs) are loaded initially, while the audio samples themselves are not loaded until they are actually played.
This behaviour of loading things only when they are needed is also called `lazy loading`.
While it saves resources, it can also lead to sounds not being audible the first time they are triggered, because the sound is still loading.
[This might be fixed in the future](https://codeberg.org/uzu/strudel/issues/187)

# Sound Banks

If we open the `sounds` tab and then `drum-machines`, we can see that the drum samples are all prefixed with drum machine names: `RolandTR808_bd`, `RolandTR808_sd`, `RolandTR808_hh` etc..

We *could* use them like this:

```
s("RolandTR808_bd RolandTR808_sd,RolandTR808_hh*16")
```

â¦ but thats obviously a bit much to write. Using the `bank` function, we can shorten this to:

```
s("bd sd,hh*16").bank("RolandTR808")
```

You could even pattern the bank to switch between different drum machines:

```
s("bd sd,hh*16").bank("<RolandTR808 RolandTR909>")
```

Behind the scenes, `bank` will just prepend the drum machine name to the sample name with `_` to get the full name.
This of course only works because the name after `_` (`bd`, `sd` etc..) is standardized.
Also note that some banks wonât have samples for all sounds!

# Selecting Sounds

If we open the `sounds` tab again, followed by tab `drum machines`, there is also a number behind each name, indicating how many individual samples are available.
For example `RolandTR909_hh(4)` means there are 4 samples of a TR909 hihat available.
By default, `s` will play the first sample, but we can select the other ones using `n`, starting from 0:

```
s("hh*8").bank("RolandTR909").n("0 1 2 3")
```

Numbers that are too high will just wrap around to the beginning

```
s("hh*8").bank("RolandTR909").n("0 1 2 3 4 5 6 7")
```

Here, 0-3 will play the same sounds as 4-7, because `RolandTR909_hh` only has 4 sounds.

Selecting sounds also works inside the mini notation, using â`:`â like this:

```
s("bd*4,hh:0 hh:1 hh:2 hh:3 hh:4 hh:5 hh:6 hh:7")
.bank("RolandTR909")
```

# Loading Custom Samples

You can load a non-standard sample map using the `samples` function.

## Loading samples from file URLs

In this example we assign names `bassdrum`, `hihat` and `snaredrum` to specific audio files on a server:

```
samples({
bassdrum: 'bd/BT0AADA.wav',
hihat: 'hh27/000_hh27closedhh.wav',
snaredrum: ['sd/rytm-01-classic.wav', 'sd/rytm-00-hard.wav'],
}, 'https://raw.githubusercontent.com/tidalcycles/Dirt-Samples/master/');

s("bassdrum snaredrum:0 bassdrum snaredrum:1, hihat*16")
```

You can freely choose any combination of letters for each sample name. It is even possible to override the default sounds.
The names you pick will be made available in the `s` function.
Make sure that the URL and each sample path form a correct URL!

In the above example, `bassdrum` will load:

```
https://raw.githubusercontent.com/tidalcycles/Dirt-Samples/master/bd/BT0AADA.wav
|----------------------base path --------------------------------|--sample path-|
```

Note that we can either load a single file, like for `bassdrum` and `hihat`, or a list of files like for `snaredrum`!
As soon as you run the code, your chosen sample names will be listed in `sounds` -> `user`.

## Loading Samples from a strudel.json file

The above way to load samples might be tedious to write out / copy paste each time you write a new pattern.
To avoid that, you can simply pass a URL to a `strudel.json` file somewhere on the internet:

```
samples('https://raw.githubusercontent.com/tidalcycles/Dirt-Samples/master/strudel.json')
s("bd sd bd sd,hh*16")
```

The file is expected to define a sample map using JSON, in the same format as described above.
Additionally, the base path can be defined with the `_base` key.
The last section could be written as:

```
{
  "_base": "https://raw.githubusercontent.com/tidalcycles/Dirt-Samples/master/",
  "bassdrum": "bd/BT0AADA.wav",
  "snaredrum": "sd/rytm-01-classic.wav",
  "hihat": "hh27/000_hh27closedhh.wav"
}
```

## Github Shortcut

Because loading samples from github is common, there is a shortcut:

```
samples('github:tidalcycles/dirt-samples')
s("bd sd bd sd,hh*16")
```

The format is `samples('github:<user>/<repo>/<branch>')`. If you omit `branch` (like above), the `main` branch will be used.
It assumes a `strudel.json` file to be present at the root of the repository:

```
https://raw.githubusercontent.com/<user>/<repo>/<branch>/strudel.json
```

## From Disk via “Import Sounds Folder”

If you donât want to upload your samples to the internet, you can also load them from your local disk.
Go to the `sounds` tab in the REPL and open the `import-sounds` tab below the search bar.
Press the âimport sounds folderâ button and select a folder that contains audio files.
The folder you select can also contain subfolders with audio files.
Example:

```
└─ samples
   ├─ swoop
   │  ├─ swoopshort.wav
   │  ├─ swooplong.wav
   │  └─ swooptight.wav
   └─ smash
      ├─ smashhigh.wav
      ├─ smashlow.wav
      └─ smashmiddle.wav
```

In the above example the folder `samples` contains 2 subfolders `swoop` and `smash`, which contain audio files.
If you select that `samples` folder, the `user` tab (next to the `import-sounds` tab) will then contain 2 new sounds: `swoop(3) smash(3)`
The individual samples can the be played normally like `s("swoop:0 swoop:1 smash:2")`.
The samples within each sound use zero-based indexing in alphabetical order.

## From Disk via @strudel/sampler

Instead of loading your samples into your browser with the âimport sounds folderâ button, you can also serve the samples from a local file server.
The easiest way to do this is using [@strudel/sampler](https://www.npmjs.com/package/%40strudel/sampler):

```
cd samples
npx @strudel/sampler
```

Then you can load it via:

```
samples('http://localhost:5432/');

n("<0 1 2>").s("swoop smash")
```

The handy thing about `@strudel/sampler` is that it auto-generates the `strudel.json` file based on your folder structure.
You can see what it generated by going to `http://localhost:5432` with your browser.

Note: You need [NodeJS](https://nodejs.org/) installed on your system for this to work.

## Specifying Pitch

To make sure your samples are in tune when playing them with `note`, you can specify a base pitch like this:

```
samples({
'gtr': 'gtr/0001_cleanC.wav',
'moog': { 'g3': 'moog/005_Mighty%20Moog%20G3.wav' },
}, 'github:tidalcycles/dirt-samples');
note("g3 [bb3 c4] <g4 f4 eb4 f3>@2").s("gtr,moog").clip(1)
.gain(.5)
```

We can also declare different samples for different regions of the keyboard:

```
setcpm(60)
samples({
'moog': {
  'g2': 'moog/004_Mighty%20Moog%20G2.wav',
  'g3': 'moog/005_Mighty%20Moog%20G3.wav',
  'g4': 'moog/006_Mighty%20Moog%20G4.wav',
}}, 'github:tidalcycles/dirt-samples')

note("g2!2 <bb2 c3>!2, <c4@3 [<eb4 bb3> g4 f4]>")
.s('moog').clip(1)
.gain(.5)
```

The sampler will always pick the closest matching sample for the current note!

Note that this notation for pitched sounds also works inside a `strudel.json` file.

## Shabda

If you donât want to select samples by hand, there is also the wonderful tool called [shabda](https://shabda.ndre.gr/).
With it, you can enter any sample name(s) to query from [freesound.org](https://freesound.org/). Example:

```
samples('shabda:bass:4,hihat:4,rimshot:2')

$: n("0 1 2 3 0 1 2 3").s('bass')
$: n("0 1*2 2 3*2").s('hihat').clip(1)
$: n("~ 0 ~ 1 ~ 0 0 1").s('rimshot')
```

You can also generate artificial voice samples with any text, in multiple languages.
Note that the language code and the gender parameters are optional and default to `en-GB` and `f`

```
samples('shabda/speech:the_drum,forever')
samples('shabda/speech/fr-FR/m:magnifique')

$: s("the_drum*2").chop(16).speed(rand.range(0.85,1.1))
$: s("forever magnifique").slow(4).late(0.125)
```

# Sampler Effects

Sampler effects are functions that can be used to change the behaviour of sample playback.

### begin

a pattern of numbers from 0 to 1. Skips the beginning of each sample, e.g. `0.25` to cut off the first quarter from each sample.

* amount (number|Pattern): between 0 and 1, where 1 is the length of the sample

```
samples({ rave: 'rave/AREUREADY.wav' }, 'github:tidalcycles/dirt-samples')
s("rave").begin("<0 .25 .5 .75>").fast(2)
```

### end

The same as .begin, but cuts off the end off each sample.

* length (number|Pattern): 1 = whole sample, .5 = half sample, .25 = quarter sample etc..

```
s("bd*2,oh*4").end("<.1 .2 .5 1>").fast(2)
```

### loop

Loops the sample.
Note that the tempo of the loop is not synced with the cycle tempo.
To change the loop region, use loopBegin / loopEnd.

* on (number|Pattern): If 1, the sample is looped

```
s("casio").loop(1)
```

### loopBegin

Synonyms: `loopb`

Begin to loop at a specific point in the sample (inbetween `begin` and `end`).
Note that the loop point must be inbetween `begin` and `end`, and before `loopEnd`!
Note: Samples starting with wt\_ will automatically loop! (wt = wavetable)

* time (number|Pattern): between 0 and 1, where 1 is the length of the sample

```
s("space").loop(1)
.loopBegin("<0 .125 .25>")._scope()
```

### loopEnd

Synonyms: `loope`

End the looping section at a specific point in the sample (inbetween `begin` and `end`).
Note that the loop point must be inbetween `begin` and `end`, and after `loopBegin`!

* time (number|Pattern): between 0 and 1, where 1 is the length of the sample

```
s("space").loop(1)
.loopEnd("<1 .75 .5 .25>")._scope()
```

### cut

In the style of classic drum-machines, `cut` will stop a playing sample as soon as another samples with in same cutgroup is to be played. An example would be an open hi-hat followed by a closed one, essentially muting the open.

* group (number|Pattern): cut group number

```
s("[oh hh]*4").cut(1)
```

### clip

Synonyms: `legato`

Multiplies the duration with the given number. Also cuts samples off at the end if they exceed the duration.

* factor (number|Pattern):
  = 0

```
note("c a f e").s("piano").clip("<.5 1 2>")
```

### loopAt

Makes the sample fit the given number of cycles by changing the speed.

```
samples({ rhodes: 'https://cdn.freesound.org/previews/132/132051_316502-lq.mp3' })
s("rhodes").loopAt(2)
```

### fit

Makes the sample fit its event duration. Good for rhythmical loops like drum breaks.
Similar to `loopAt`.

```
samples({ rhodes: 'https://cdn.freesound.org/previews/132/132051_316502-lq.mp3' })
s("rhodes/2").fit()
```

### chop

Cuts each sample into the given number of parts, allowing you to explore a technique known as 'granular synthesis'.
It turns a pattern of samples into a pattern of parts of samples.

```
samples({ rhodes: 'https://cdn.freesound.org/previews/132/132051_316502-lq.mp3' })
s("rhodes")
 .chop(4)
 .rev() // reverse order of chops
 .loopAt(2) // fit sample into 2 cycles
```

### striate

Cuts each sample into the given number of parts, triggering progressive portions of each sample at each loop.

```
s("numbers:0 numbers:1 numbers:2").striate(6).slow(3)
```

### slice

Chops samples into the given number of slices, triggering those slices with a given pattern of slice numbers.
Instead of a number, it also accepts a list of numbers from 0 to 1 to slice at specific points.

```
samples('github:tidalcycles/dirt-samples')
s("breaks165").slice(8, "0 1 <2 2*2> 3 [4 0] 5 6 7".every(3, rev)).slow(0.75)
```

```
samples('github:tidalcycles/dirt-samples')
s("breaks125").fit().slice([0,.25,.5,.75], "0 1 1 <2 3>")
```

### splice

Works the same as slice, but changes the playback speed of each slice to match the duration of its step.

```
samples('github:tidalcycles/dirt-samples')
s("breaks165")
.splice(8,  "0 1 [2 3 0]@2 3 0@2 7")
```

### speed

Changes the speed of sample playback, i.e. a cheap way of changing pitch.

* speed (number|Pattern): inf to inf, negative numbers play the sample backwards.

```
s("bd*6").speed("1 2 4 1 -2 -4")
```

```
speed("1 1.5*2 [2 1.1]").s("piano").clip(1)
```

# Synths

In addition to the sampling engine, strudel comes with a synthesizer to create sounds on the fly.

## Basic Waveforms

The basic waveforms are `sine`, `sawtooth`, `square` and `triangle`, which can be selected via `sound` (or `s`):

```
note("c2 <eb2 <g2 g1>>".fast(2))
.sound("<sawtooth square triangle sine>")
._scope()
```

If you donât set a `sound` but a `note` the default value for `sound` is `triangle`!

## Noise

You can also use noise as a source by setting the waveform to: `white`,Â `pink` or `brown`. These are different
flavours of noise, here written from hard to soft.

```
sound("<white pink brown>")._scope()
```

Hereâs a more musical example of how to use noise for hihats:

```
sound("bd*2,<white pink brown>*8")
.decay(.04).sustain(0)._scope()
```

Some amount of pink noise can also be added to any oscillator by using the `noise` paremeter:

```
note("c3").noise("<0.1 0.25 0.5>")._scope()
```

You can also use the `crackle` type to play some subtle noise crackles. You can control noise amount by using the `density` parameter:

```
s("crackle*4").density("<0.01 0.04 0.2 0.5>".slow(2))._scope()
```

### Additive Synthesis

To tame the harsh sound of the basic waveforms, we can set the `n` control to limit the overtones of the waveform:

```
note("c2 <eb2 <g2 g1>>".fast(2))
.sound("sawtooth")
.n("<32 16 8 4>")
._scope()
```

When the `n` control is used on a basic waveform, it defines the number of harmonic partials the sound is getting.
You can also set `n` directly in mini notation with `sound`:

```
note("c2 <eb2 <g2 g1>>".fast(2))
.sound("sawtooth:<32 16 8 4>")
._scope()
```

Note for tidal users: `n` in tidal is synonymous to `note` for synths only.
In strudel, this is not the case, where `n` will always change timbre, be it though different samples or different waveforms.

## Vibrato

### vib

Synonyms: `vibrato, v`

Applies a vibrato to the frequency of the oscillator.

* frequency (number|Pattern): of the vibrato in hertz

```
note("a e")
.vib("<.5 1 2 4 8 16>")
._scope()
```

```
// change the modulation depth with ":"
note("a e")
.vib("<.5 1 2 4 8 16>:12")
._scope()
```

### vibmod

Synonyms: `vmod`

Sets the vibrato depth in semitones. Only has an effect if `vibrato` | `vib` | `v` is is also set

* depth (number|Pattern): of vibrato (in semitones)

```
note("a e").vib(4)
.vibmod("<.25 .5 1 2 12>")
._scope()
```

```
// change the vibrato frequency with ":"
note("a e")
.vibmod("<.25 .5 1 2 12>:8")
._scope()
```

## FM Synthesis

FM Synthesis is a technique that changes the frequency of a basic waveform rapidly to alter the timbre.

You can use fm with any of the above waveforms, although the below examples all use the default triangle wave.

### fm

Synonyms: `fmi`

Sets the Frequency Modulation of the synth.
Controls the modulation index, which defines the brightness of the sound.

* brightness (number|Pattern): modulation index

```
note("c e g b g e")
.fm("<0 1 2 8 32>")
._scope()
```

### fmh

Sets the Frequency Modulation Harmonicity Ratio.
Controls the timbre of the sound.
Whole numbers and simple ratios sound more natural,
while decimal numbers and complex ratios sound metallic.

* harmonicity (number|Pattern):

```
note("c e g b g e")
.fm(4)
.fmh("<1 2 1.5 1.61>")
._scope()
```

### fmattack

Attack time for the FM envelope: time it takes to reach maximum modulation

* time (number|Pattern): attack time

```
note("c e g b g e")
.fm(4)
.fmattack("<0 .05 .1 .2>")
._scope()
```

### fmdecay

Decay time for the FM envelope: seconds until the sustain level is reached after the attack phase.

* time (number|Pattern): decay time

```
note("c e g b g e")
.fm(4)
.fmdecay("<.01 .05 .1 .2>")
.fmsustain(.4)
._scope()
```

### fmsustain

Sustain level for the FM envelope: how much modulation is applied after the decay phase

* level (number|Pattern): sustain level

```
note("c e g b g e")
.fm(4)
.fmdecay(.1)
.fmsustain("<1 .75 .5 0>")
._scope()
```

### fmenv

Ramp type of fm envelope. Exp might be a bit broken..

* type (number|Pattern): lin | exp

```
note("c e g b g e")
.fm(4)
.fmdecay(.2)
.fmsustain(0)
.fmenv("<exp lin>")
._scope()
```

## Wavetable Synthesis

Strudel can also use the sampler to load custom waveforms as a replacement of the default waveforms used by WebAudio for the base synth. A default set of more than 1000 wavetables is accessible by default (coming from the [AKWF](https://www.adventurekid.se/akrt/waveforms/adventure-kid-waveforms/) set). You can also import/use your own. A wavetable is a one-cycle waveform, which is then repeated to create a sound at the desired frequency. It is a classic but very effective synthesis technique.

Any sample preceded by the `wt_` prefix will be loaded as a wavetable. This means that the `loop` argument will be set to `1` by default. You can scan over the wavetable by using `loopBegin` and `loopEnd` as well.

```
samples('bubo:waveforms');
note("<[g3,b3,e4]!2 [a3,c3,e4] [b3,d3,f#4]>")
.n("<1 2 3 4 5 6 7 8 9 10>/2").room(0.5).size(0.9)
.s('wt_flute').velocity(0.25).often(n => n.ply(2))
.release(0.125).decay("<0.1 0.25 0.3 0.4>").sustain(0)
.cutoff(2000).cutoff("<1000 2000 4000>").fast(4)
._scope()

```

## ZZFX

The Zuper Zmall Zound Zynthâ [ZZFX](https://github.com/KilledByAPixel/ZzFX) is also integrated in strudel.
Developed by [Frank Force](https://frankforce.com/), it is a synth and FX engine originally intended to be used for size coding games.

It has 20 parameters in total, here is a snippet that uses all:

```
note("c2 eb2 f2 g2") // also supports freq
.s("{z_sawtooth z_tan z_noise z_sine z_square}%4")
.zrand(0) // randomization
// zzfx envelope
.attack(0.001)
.decay(0.1)
.sustain(.8)
.release(.1)
// special zzfx params
.curve(1) // waveshape 1-3
.slide(0) // +/- pitch slide
.deltaSlide(0) // +/- pitch slide (?)
.noise(0) // make it dirty
.zmod(0) // fm speed
.zcrush(0) // bit crush 0 - 1
.zdelay(0) // simple delay
.pitchJump(0) // +/- pitch change after pitchJumpTime
.pitchJumpTime(0) // >0 time after pitchJump is applied
.lfo(0) // >0 resets slide + pitchJump + sets tremolo speed
.tremolo(0) // 0-1 lfo volume modulation amount
//.duration(.2) // overwrite strudel event duration
//.gain(1) // change volume
._scope() // vizualise waveform (not zzfx related)

```

# Audio Effects

Whether youâre using a synth or a sample, you can apply any of the following built-in audio effects.
As you might suspect, the effects can be chained together, and they accept a pattern string as their argument.

# Filters

Filters are an essential building block of [subtractive synthesis](https://en.wikipedia.org/wiki/Subtractive_synthesis).
Strudel comes with 3 types of filters:

* low-pass filter: low frequencies may *pass*, high frequencies are cut off
* high-pass filter: high frequencies may *pass*, low frequencies are cut off
* band-pass filters: only a frequency band may *pass*, low and high frequencies around are cut off

Each filter has 2 parameters:

* cutoff: the frequency at which the filter starts to work. e.g. a low-pass filter with a cutoff of 1000Hz allows frequencies below 1000Hz to pass.
* q-value: Controls the resonance of the filter. Higher values sound more aggressive. Also see [Q-Factor](https://en.wikipedia.org/wiki/Q_factor)

## lpf

Synonyms: `cutoff, ctf, lp`

Applies the cutoff frequency of the **l**ow-**p**ass **f**ilter.

When using mininotation, you can also optionally add the 'lpq' parameter, separated by ':'.

* frequency (number|Pattern): audible between 0 and 20000

```
s("bd sd [~ bd] sd,hh*6").lpf("<4000 2000 1000 500 200 100>")
```

```
s("bd*16").lpf("1000:0 1000:10 1000:20 1000:30")
```

## lpq

Synonyms: `resonance`

Controls the **l**ow-**p**ass **q**-value.

* q (number|Pattern): resonance factor between 0 and 50

```
s("bd sd [~ bd] sd,hh*8").lpf(2000).lpq("<0 10 20 30>")
```

## hpf

Synonyms: `hp, hcutoff`

Applies the cutoff frequency of the **h**igh-**p**ass **f**ilter.

When using mininotation, you can also optionally add the 'hpq' parameter, separated by ':'.

* frequency (number|Pattern): audible between 0 and 20000

```
s("bd sd [~ bd] sd,hh*8").hpf("<4000 2000 1000 500 200 100>")
```

```
s("bd sd [~ bd] sd,hh*8").hpf("<2000 2000:25>")
```

## hpq

Synonyms: `hresonance`

Controls the **h**igh-**p**ass **q**-value.

* q (number|Pattern): resonance factor between 0 and 50

```
s("bd sd [~ bd] sd,hh*8").hpf(2000).hpq("<0 10 20 30>")
```

## bpf

Synonyms: `bandf, bp`

Sets the center frequency of the **b**and-**p**ass **f**ilter. When using mininotation, you
can also optionally supply the 'bpq' parameter separated by ':'.

* frequency (number|Pattern): center frequency

```
s("bd sd [~ bd] sd,hh*6").bpf("<1000 2000 4000 8000>")
```

## bpq

Synonyms: `bandq`

Sets the **b**and-**p**ass **q**-factor (resonance).

* q (number|Pattern): q factor

```
s("bd sd [~ bd] sd").bpf(500).bpq("<0 1 2 3>")
```

## ftype

Sets the filter type. The ladder filter is more aggressive. More types might be added in the future.

* type (number|Pattern): 12db (0), ladder (1), or 24db (2)

```
note("{f g g c d a a#}%8").s("sawtooth").lpenv(4).lpf(500).ftype("<0 1 2>").lpq(1)
```

```
note("c f g g a c d4").fast(2)
.sound('sawtooth')
.lpf(200).fanchor(0)
.lpenv(3).lpq(1)
.ftype("<ladder 12db 24db>")
```

## vowel

Formant filter to make things sound like vowels.

* vowel (string|Pattern): You can use a e i o u ae aa oe ue y uh un en an on, corresponding to [a] [e] [i] [o] [u] [Ã¦] [É] [Ã¸] [y] [É¯] [Ê] [ÅÌ] [ÉÌ] [ÉÌ] [ÉÌ]. Aliases: aa = Ã¥ = É, oe = Ã¸ = Ã¶, y = Ä±, ae = Ã¦.

```
note("[c2 <eb2 <g2 g1>>]*2").s('sawtooth')
.vowel("<a e i <o u>>")
```

```
s("bd sd mt ht bd [~ cp] ht lt").vowel("[a|e|i|o|u]")
```

# Amplitude Envelope

The amplitude [envelope](https://en.wikipedia.org/wiki/Envelope_%28music%29) controls the dynamic contour of a sound.
Strudel uses ADSR envelopes, which are probably the most common way to describe an envelope:

![ADSR](https://upload.wikimedia.org/wikipedia/commons/thumb/e/ea/ADSR_parameter.svg/1920px-ADSR_parameter.svg.png)

[image link](https://commons.wikimedia.org/wiki/File%3AADSR_parameter.svg)

## attack

Synonyms: `att`

Amplitude envelope attack time: Specifies how long it takes for the sound to reach its peak value, relative to the onset.

* attack (number|Pattern): time in seconds.

```
note("c3 e3 f3 g3").attack("<0 .1 .5>")
```

## decay

Amplitude envelope decay time: the time it takes after the attack time to reach the sustain level.
Note that the decay is only audible if the sustain value is lower than 1.

* time (number|Pattern): decay time in seconds

```
note("c3 e3 f3 g3").decay("<.1 .2 .3 .4>").sustain(0)
```

## sustain

Synonyms: `sus`

Amplitude envelope sustain level: The level which is reached after attack / decay, being sustained until the offset.

* gain (number|Pattern): sustain level between 0 and 1

```
note("c3 e3 f3 g3").decay(.2).sustain("<0 .1 .4 .6 1>")
```

## release

Synonyms: `rel`

Amplitude envelope release time: The time it takes after the offset to go from sustain level to zero.

* time (number|Pattern): release time in seconds

```
note("c3 e3 g3 c4").release("<0 .1 .4 .6 1>/2")
```

## adsr

ADSR envelope: Combination of Attack, Decay, Sustain, and Release.

* time (number|Pattern): attack time in seconds
* time (number|Pattern): decay time in seconds
* gain (number|Pattern): sustain level (0 to 1)
* time (number|Pattern): release time in seconds

```
note("[c3 bb2 f3 eb3]*2").sound("sawtooth").lpf(600).adsr(".1:.1:.5:.2")
```

# Filter Envelope

Each filter can receive an additional filter envelope controlling the cutoff value dynamically. It uses an ADSR envelope similar to the one used for amplitude. There is an additional parameter to control the depth of the filter modulation: `lpenv`|`hpenv`|`bpenv`. This allows you to play subtle or huge filter modulations just the same by only increasing or decreasing the depth.

```
note("[c eb g <f bb>](3,8,<0 1>)".sub(12))
.s("<sawtooth>/64")
.lpf(sine.range(300,2000).slow(16))
.lpa(0.005)
.lpd(perlin.range(.02,.2))
.lps(perlin.range(0,.5).slow(3))
.lpq(sine.range(2,10).slow(32))
.release(.5)
.lpenv(perlin.range(1,8).slow(2))
.ftype('24db')
.room(1)
.juxBy(.5,rev)
.sometimes(add(note(12)))
.stack(s("bd*2").bank('RolandTR909'))
.gain(.5).fast(2)
```

There is one filter envelope for each filter type and thus one set of envelope filter parameters preceded either by `lp`, `hp` or `bp`:

* `lpattack`, `lpdecay`, `lpsustain`, `lprelease`, `lpenv`: filter envelope for the lowpass filter.
  + alternatively: `lpa`, `lpd`, `lps`, `lpr` and `lpe`.
* `hpattack`, `hpdecay`, `hpsustain`, `hprelease`, `hpenv`: filter envelope for the highpass filter.
  + alternatively: `hpa`, `hpd`, `hps`, `hpr` and `hpe`.
* `bpattack`, `bpdecay`, `bpsustain`, `bprelease`, `bpenv`: filter envelope for the bandpass filter.
  + alternatively: `bpa`, `bpd`, `bps`, `bpr` and `bpe`.

## lpattack

Synonyms: `lpa`

Sets the attack duration for the lowpass filter envelope.

* attack (number|Pattern): time of the filter envelope

```
note("c2 e2 f2 g2")
.sound('sawtooth')
.lpf(300)
.lpa("<.5 .25 .1 .01>/4")
.lpenv(4)
```

## lpdecay

Synonyms: `lpd`

Sets the decay duration for the lowpass filter envelope.

* decay (number|Pattern): time of the filter envelope

```
note("c2 e2 f2 g2")
.sound('sawtooth')
.lpf(300)
.lpd("<.5 .25 .1 0>/4")
.lpenv(4)
```

## lpsustain

Synonyms: `lps`

Sets the sustain amplitude for the lowpass filter envelope.

* sustain (number|Pattern): amplitude of the lowpass filter envelope

```
note("c2 e2 f2 g2")
.sound('sawtooth')
.lpf(300)
.lpd(.5)
.lps("<0 .25 .5 1>/4")
.lpenv(4)
```

## lprelease

Synonyms: `lpr`

Sets the release time for the lowpass filter envelope.

* release (number|Pattern): time of the filter envelope

```
note("c2 e2 f2 g2")
.sound('sawtooth')
.clip(.5)
.lpf(300)
.lpenv(4)
.lpr("<.5 .25 .1 0>/4")
.release(.5)
```

## lpenv

Synonyms: `lpe`

Sets the lowpass filter envelope modulation depth.

* modulation (number|Pattern): depth of the lowpass filter envelope between 0 and n

```
note("c2 e2 f2 g2")
.sound('sawtooth')
.lpf(300)
.lpa(.5)
.lpenv("<4 2 1 0 -1 -2 -4>/4")
```

# Pitch Envelope

You can also control the pitch with envelopes!
Pitch envelopes can breathe life into static sounds:

```
n("<-4,0 5 2 1>*<2!3 4>")
.scale("<C F>/8:pentatonic")
.s("gm_electric_guitar_jazz")
.penv("<.5 0 7 -2>*2").vib("4:.1")
.phaser(2).delay(.25).room(.3)
.size(4).fast(1.5)
```

You also create some lovely chiptune-style sounds:

```
n(run("<4 8>/16")).jux(rev)
.chord("<C^7 <Db^7 Fm7>>")
.dict('ireal')
.voicing().add(note("<0 1>/8"))
.dec(.1).room(.2)
.segment("<4 [2 8]>")
.penv("<0 <2 -2>>").patt(.02).fast(2)
```

Letâs break down all pitch envelope controls:

## pattack

Synonyms: `patt`

Attack time of pitch envelope.

* time (number|Pattern): time in seconds

```
note("c eb g bb").pattack("0 .1 .25 .5").slow(2)
```

## pdecay

Synonyms: `pdec`

Decay time of pitch envelope.

* time (number|Pattern): time in seconds

```
note("<c eb g bb>").pdecay("<0 .1 .25 .5>")
```

## prelease

Synonyms: `prel`

Release time of pitch envelope

* time (number|Pattern): time in seconds

```
note("<c eb g bb> ~")
.release(.5) // to hear the pitch release
.prelease("<0 .1 .25 .5>")
```

## penv

Amount of pitch envelope. Negative values will flip the envelope.
If you don't set other pitch envelope controls, `pattack:.2` will be the default.

* semitones (number|Pattern): change in semitones

```
note("c")
.penv("<12 7 1 .5 0 -1 -7 -12>")
```

## pcurve

Curve of envelope. Defaults to linear. exponential is good for kicks

* type (number|Pattern): 0 = linear, 1 = exponential

```
note("g1*4")
.s("sine").pdec(.5)
.penv(32)
.pcurve("<0 1>")
```

## panchor

Sets the range anchor of the envelope:

* anchor 0: range = [note, note + penv]
* anchor 1: range = [note - penv, note]
  If you don't set an anchor, the value will default to the psustain value.

* anchor (number|Pattern): anchor offset

```
note("c c4").penv(12).panchor("<0 .5 1 .5>")
```

# Dynamics

## gain

Controls the gain by an exponential amount.

* amount (number|Pattern): gain.

```
s("hh*8").gain(".4!2 1 .4!2 1 .4 1").fast(2)
```

## velocity

Sets the velocity from 0 to 1. Is multiplied together with gain.

```
s("hh*8")
.gain(".4!2 1 .4!2 1 .4 1")
.velocity(".4 1")
```

## compressor

Dynamics Compressor. The params are `compressor("threshold:ratio:knee:attack:release")`
More info [here](https://developer.mozilla.org/en-US/docs/Web/API/DynamicsCompressorNode?retiredLocale=de#instance_properties)

```
s("bd sd [~ bd] sd,hh*8")
.compressor("-20:20:10:.002:.02")
```

## postgain

Gain applied after all effects have been processed.

```
s("bd sd [~ bd] sd,hh*8")
.compressor("-20:20:10:.002:.02").postgain(1.5)
```

## xfade

Cross-fades between left and right from 0 to 1:

* 0 = (full left, no right)
* .5 = (both equal)
* 1 = (no left, full right)

```
xfade(s("bd*2"), "<0 .25 .5 .75 1>", s("hh*8"))
```

# Panning

## jux

The jux function creates strange stereo effects, by applying a function to a pattern, but only in the right-hand channel.

```
s("bd lt [~ ht] mt cp ~ bd hh").jux(rev)
```

```
s("bd lt [~ ht] mt cp ~ bd hh").jux(press)
```

```
s("bd lt [~ ht] mt cp ~ bd hh").jux(iter(4))
```

## juxBy

Synonyms: `juxby`

Jux with adjustable stereo width. 0 = mono, 1 = full stereo.

```
s("bd lt [~ ht] mt cp ~ bd hh").juxBy("<0 .5 1>/2", rev)
```

## pan

Sets position in stereo.

* pan (number|Pattern): between 0 and 1, from left to right (assuming stereo), once round a circle (assuming multichannel)

```
s("[bd hh]*2").pan("<.5 1 .5 0>")
```

```
s("bd rim sd rim bd ~ cp rim").pan(sine.slow(2))
```

# Waveshaping

## coarse

fake-resampling for lowering the sample rate. Caution: This effect seems to only work in chromium based browsers

* factor (number|Pattern): 1 for original 2 for half, 3 for a third and so on.

```
s("bd sd [~ bd] sd,hh*8").coarse("<1 4 8 16 32>")
```

## crush

bit crusher effect.

* depth (number|Pattern): between 1 (for drastic reduction in bit-depth) to 16 (for barely no reduction).

```
s("<bd sd>,hh*3").fast(2).crush("<16 8 7 6 5 4 3 2>")
```

## distort

Synonyms: `dist`

Wave shaping distortion. CAUTION: it can get loud.
Second option in optional array syntax (ex: ".9:.5") applies a postgain to the output.
Most useful values are usually between 0 and 10 (depending on source gain). If you are feeling adventurous, you can turn it up to 11 and beyond ;)

* distortion (number|Pattern):

```
s("bd sd [~ bd] sd,hh*8").distort("<0 2 3 10:.5>")
```

```
note("d1!8").s("sine").penv(36).pdecay(.12).decay(.23).distort("8:.4")
```

# Global Effects

## Local vs Global Effects

While the above listed âlocalâ effects will always create a separate effects chain for each event,
global effects use the same chain for all events of the same orbit:

## orbit

An `orbit` is a global parameter context for patterns. Patterns with the same orbit will share the same global effects.

* number (number|Pattern):

```
stack(
  s("hh*6").delay(.5).delaytime(.25).orbit(1),
  s("~ sd ~ sd").delay(.5).delaytime(.125).orbit(2)
)
```

## Delay

### delay

Sets the level of the delay signal.

When using mininotation, you can also optionally add the 'delaytime' and 'delayfeedback' parameter,
separated by ':'.

* level (number|Pattern): between 0 and 1

```
s("bd bd").delay("<0 .25 .5 1>")
```

```
s("bd bd").delay("0.65:0.25:0.9 0.65:0.125:0.7")
```

### delaytime

Synonyms: `delayt, dt`

Sets the time of the delay effect.

* seconds (number|Pattern): between 0 and Infinity

```
s("bd bd").delay(.25).delaytime("<.125 .25 .5 1>")
```

### delayfeedback

Synonyms: `delayfb, dfb`

Sets the level of the signal that is fed back into the delay.
Caution: Values >= 1 will result in a signal that gets louder and louder! Don't do it

* feedback (number|Pattern): between 0 and 1

```
s("bd").delay(.25).delayfeedback("<.25 .5 .75 1>")
```

## Reverb

### room

Sets the level of reverb.

When using mininotation, you can also optionally add the 'size' parameter, separated by ':'.

* level (number|Pattern): between 0 and 1

```
s("bd sd [~ bd] sd").room("<0 .2 .4 .6 .8 1>")
```

```
s("bd sd [~ bd] sd").room("<0.9:1 0.9:4>")
```

### roomsize

Synonyms: `rsize, sz, size`

Sets the room size of the reverb, see `room`.
When this property is changed, the reverb will be recaculated, so only change this sparsely..

* size (number|Pattern): between 0 and 10

```
s("bd sd [~ bd] sd").room(.8).rsize(1)
```

```
s("bd sd [~ bd] sd").room(.8).rsize(4)
```

### roomfade

Synonyms: `rfade`

Reverb fade time (in seconds).
When this property is changed, the reverb will be recaculated, so only change this sparsely..

* seconds (number): for the reverb to fade

```
s("bd sd [~ bd] sd").room(0.5).rlp(10000).rfade(0.5)
```

```
s("bd sd [~ bd] sd").room(0.5).rlp(5000).rfade(4)
```

### roomlp

Synonyms: `rlp`

Reverb lowpass starting frequency (in hertz).
When this property is changed, the reverb will be recaculated, so only change this sparsely..

* frequency (number): between 0 and 20000hz

```
s("bd sd [~ bd] sd").room(0.5).rlp(10000)
```

```
s("bd sd [~ bd] sd").room(0.5).rlp(5000)
```

### roomdim

Synonyms: `rdim`

Reverb lowpass frequency at -60dB (in hertz).
When this property is changed, the reverb will be recaculated, so only change this sparsely..

* frequency (number): between 0 and 20000hz

```
s("bd sd [~ bd] sd").room(0.5).rlp(10000).rdim(8000)
```

```
s("bd sd [~ bd] sd").room(0.5).rlp(5000).rdim(400)
```

### iresponse

Synonyms: `ir`

Sets the sample to use as an impulse response for the reverb.

* sample (string|Pattern): to use as an impulse response

```
s("bd sd [~ bd] sd").room(.8).ir("<shaker_large:0 shaker_large:2>")
```

## Phaser

### phaser

Synonyms: `ph`

Phaser audio effect that approximates popular guitar pedals.

* speed (number|Pattern): speed of modulation

```
n(run(8)).scale("D:pentatonic").s("sawtooth").release(0.5)
.phaser("<1 2 4 8>")
```

### phaserdepth

Synonyms: `phd`

The amount the signal is affected by the phaser effect. Defaults to 0.75

* depth (number|Pattern): number between 0 and 1

```
n(run(8)).scale("D:pentatonic").s("sawtooth").release(0.5)
.phaser(2).phaserdepth("<0 .5 .75 1>")
```

### phasercenter

Synonyms: `phc`

The center frequency of the phaser in HZ. Defaults to 1000

* centerfrequency (number|Pattern): in HZ

```
n(run(8)).scale("D:pentatonic").s("sawtooth").release(0.5)
.phaser(2).phasercenter("<800 2000 4000>")
```

### phasersweep

Synonyms: `phs`

The frequency sweep range of the lfo for the phaser effect. Defaults to 2000

* phasersweep (number|Pattern): most useful values are between 0 and 4000

```
n(run(8)).scale("D:pentatonic").s("sawtooth").release(0.5)
.phaser(2).phasersweep("<800 2000 4000>")
```

# MIDI, OSC and MQTT

Normally, Strudel is used to pattern sound, using its own â[web audio](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API)â-based synthesiser called [SuperDough](https://codeberg.org/uzu/strudel/src/branch/main/packages/superdough).

It is also possible to pattern other things with Strudel, such as software and hardware synthesisers with MIDI, other software using Open Sound Control/OSC (including the [SuperDirt](https://github.com/musikinformatik/SuperDirt/) synthesiser commonly used with Strudelâs sibling [TidalCycles](https://tidalcycles.org/)), or the MQTT âinternet of thingsâ protocol.

# MIDI

Strudel supports MIDI without any additional software (thanks to [webmidi](https://npmjs.com/package/webmidi)), just by adding methods to your pattern:

## midiin(inputName?)

MIDI input: Opens a MIDI input port to receive MIDI control change messages.

* input (string|number): MIDI device name or index defaulting to 0

```
let cc = await midin('IAC Driver Bus 1')
note("c a f e").lpf(cc(0).range(0, 1000)).lpq(cc(1).range(0, 10)).sound("sawtooth")
```

## midi(outputName?,options?)

Either connect a midi device or use the IAC Driver (Mac) or Midi Through Port (Linux) for internal midi messages.
If no outputName is given, it uses the first midi output it finds.

```

$: chord("<C^7 A7 Dm7 G7>").voicing().midi('IAC Driver')

```

In the console, you will see a log of the available MIDI devices as soon as you run the code,
e.g.

```
 `Midi connected! Using "Midi Through Port-0".`
```

The `.midi()` function accepts an options object with the following properties:

```
$: note("d e c a f").midi('IAC Driver', { isController: true, midimap: 'default'})

```

Available Options

| Option | Type | Default | Description |
| --- | --- | --- | --- |
| isController | boolean | false | When true, disables sending note messages. Useful for MIDI controllers |
| latencyMs | number | 34 | Latency in milliseconds to align MIDI with audio engine |
| noteOffsetMs | number | 10 | Offset in milliseconds for note-off messages to prevent glitching |
| midichannel | number | 1 | Default MIDI channel (1-16) |
| velocity | number | 0.9 | Default note velocity (0-1) |
| gain | number | 1 | Default gain multiplier for velocity (0-1) |
| midimap | string | âdefaultâ | Name of MIDI mapping to use for control changes |
| midiport | string/number | - | MIDI device name or index |

### midiport(outputName)

Selects the MIDI output device to use, pattern can be used to switch between devices.

```
$: midiport('IAC Driver');
$: note('c a f e').midiport('<0 1 2 3>').midi();
```

MIDI port: Sets the MIDI port for the event.

* port (number|Pattern): MIDI port

```
note("c a f e").midiport("<0 1 2 3>").midi()
```

## midichan(number)

Selects the MIDI channel to use. If not used, `.midi` will use channel 1 by default.

## midicmd(command)

`midicmd` sends MIDI system real-time messages to control timing and transport on MIDI devices.

It supports the following commands:

* `clock`/`midiClock` - Sends MIDI timing clock messages
* `start` - Sends MIDI start message
* `stop` - Sends MIDI stop message
* `continue` - Sends MIDI continue message

// You can control the clock with a pattern and ensure it starts in sync when the repl begins.
// Note: It might act unexpectedly if MIDI isnât set up initially.

```
$:stack(
midicmd("clock*48,<start stop>/2").midi('IAC Driver')
)
```

## control, ccn && ccv

* `control` sends MIDI control change messages to your MIDI device.
* `ccn` sets the cc number. Depends on your synths midi mapping
* `ccv` sets the cc value. normalized from 0 to 1.

```
note("c a f e").control([74, sine.slow(4)]).midi()
```

```
note("c a f e").ccn(74).ccv(sine.slow(4)).midi()
```

In the above snippet, `ccn` is set to 74, which is the filter cutoff for many synths. `ccv` is controlled by a saw pattern.
Having everything in one pattern, the `ccv` pattern will be aligned to the note pattern, because the structure comes from the left by default.
But you can also control cc messages separately like this:

```
$: note("c a f e").midi()
$: ccv(sine.segment(16).slow(4)).ccn(74).midi()
```

Instead of setting `ccn` and `ccv` directly, you can also create mappings with `midimaps`:

## midimaps

Adds midimaps to the registry. Inside each midimap, control names (e.g. lpf) are mapped to cc numbers.

```
midimaps({ mymap: { lpf: 74 } })
$: note("c a f e")
.lpf(sine.slow(4))
.midimap('mymap')
.midi()
```

```
midimaps({ mymap: {
  lpf: { ccn: 74, min: 0, max: 20000, exp: 0.5 }
}})
$: note("c a f e")
.lpf(sine.slow(2).range(400,2000))
.midimap('mymap')
.midi()
```

## defaultmidimap

configures the default midimap, which is used when no "midimap" port is set

```
defaultmidimap({ lpf: 74 })
$: note("c a f e").midi();
$: lpf(sine.slow(4).segment(16)).midi();
```

## progNum (Program Change)

`progNum` sends MIDI program change messages to switch between different presets/patches on your MIDI device.
Program change values should be numbers between 0 and 127.

```
// Switch between programs 0 and 1 every cycle
progNum("<0 1>").midi()

// Play notes while changing programs
note("c3 e3 g3").progNum("<0 1 2>").midi()
```

Program change messages are useful for switching between different instrument sounds or presets during a performance.
The exact sound that each program number maps to depends on your MIDI deviceâs configuration.

## sysex, sysexid && sysexdata (System Exclusive Message)

`sysex` sends MIDI System Exclusive (SysEx) messages to your MIDI device.
ysEx messages are device-specific commands that allow deeper control over synthesizer parameters.
The value should be an array of numbers between 0-255 representing the SysEx data bytes.

```
// Send a simple SysEx message
let id = 0x43; //Yamaha
//let id = "0x00:0x20:0x32"; //Behringer ID can be an array of numbers
let data = "0x79:0x09:0x11:0x0A:0x00:0x00"; // Set NSX-39 voice to say "Aa"
$: note("c a f e").sysex(id, data).midi();
$: note("c a f e").sysexid(id).sysexdata(data).midi();
```

The exact format of SysEx messages depends on your MIDI deviceâs specification.
Consult your deviceâs MIDI implementation guide for details on supported SysEx messages.

## midibend && miditouch

`midibend` sets MIDI pitch bend (-1 - 1)
`miditouch` sets MIDI key after touch (0-1)

```
note("c a f e").midibend(sine.slow(4).range(-0.4,0.4)).midi()
```

```
note("c a f e").miditouch(sine.slow(4).range(0,1)).midi()
```

# OSC/SuperDirt/StrudelDirt

In TidalCycles, sound is usually generated using [SuperDirt](https://github.com/musikinformatik/SuperDirt/), which runs inside SuperCollider. Strudel also supports using SuperDirt, although it requires installing some additional software.

There is also [StrudelDirt](https://github.com/daslyfe/StrudelDirt) which is SuperDirt with some optimisations for working with Strudel. (A longer term aim is to merge these optimisations back into mainline SuperDirt)

## Prequisites

To get SuperDirt to work with Strudel, you need to

1. install SuperCollider + sc3 plugins, see [Tidal Docs](https://tidalcycles.org/docs/) (Install Tidal) for more info.
2. install SuperDirt, or the [StrudelDirt](https://github.com/daslyfe/StrudelDirt) fork which is optimised for use with Strudel
3. install [node.js](https://nodejs.org/en/)
4. download [Strudel Repo](https://codeberg.org/uzu/strudel/) (or git clone, if you have git installed)
5. run `pnpm i` in the strudel directory
6. run `pnpm run osc` to start the osc server, which forwards OSC messages from Strudel REPL to SuperCollider

Now youâre all set!

## Usage

1. Start SuperCollider, either using SuperCollider IDE or by running `sclang` in a terminal
2. Open the [Strudel REPL](https://strudel.cc/#cygiYmQgc2QiKS5vc2MoKQ%3D%3D)

â¦or test it here:

If you now hear sound, congratulations! If not, you can get help on the [#strudel channel in the TidalCycles discord](https://discord.com/invite/HGEdXmRkzT).

Note: if you have the âAudio Engine Targetâ in settings set to âOSCâ, you do not need to add .osc() to the end of your pattern.

### Pattern.osc

Sends each hap as an OSC message, which can be picked up by SuperCollider or any other OSC-enabled software.
For more info, read [MIDI & OSC in the docs](https://strudel.cc/learn/input-output/)

## SuperDirt Params

Please refer to [Tidal Docs](https://tidalcycles.org/) for more info.

But can we use Strudel [offline](/learn/pwa/)?

# MQTT

MQTT is a lightweight network protocol, designed for âinternet of thingsâ devices. For use with strudel, you will
need access to an MQTT server known as a âbrokerâ configured to accept secure âwebsocketâ connections. You could
run one yourself (e.g. by running [mosquitto](https://mosquitto.org/)), although getting an SSL certificate that
your web browser will trust might be a bit tricky for those without systems administration experience.
Alternatively, you can use [a public broker](https://www.hivemq.com/mqtt/public-mqtt-broker/).

Strudel does not yet support receiving messages over MQTT, only sending them.

## Usage

The following example shows how to send a pattern to an MQTT broker:

Other software can then receive the messages. For example using the [mosquitto](https://mosquitto.org/) commandline client tools:

```

> mosquitto_sub -h mqtt.eclipseprojects.io -p 1883 -t "/strudel-pattern"
> hello
> world
> hello
> world
> ...

```

Control patterns will be encoded as JSON, for example:

Will send messages like the following:

```

{"s":"sax","speed":2}
{"s":"sax","speed":2}
{"s":"sax","speed":3}
{"s":"sax","speed":2}
...

```
