```javascript
setcps(60/60/4)
const pattn = [
  "[e4 e4 e4 -] [e4 e4 e4 -] [e4 g4 c4 d4] [e4 - - -]", 
  "[g4 - a4] [g4 - e4] [g4 - a4] [g4 - e4]",
  "[c4 d4 e4 f4] [g4 a4 b4 c5] [d5 - b4 -] [c5 - - -]",
  "[[c5 g4] [c5 g4]] [[d5 b4] [d5 b4]] [[e5 c5] [e5 c5]] [f5 - g5 -]",
  "[c4 - g4 -] [e4 - c5 -] [a4 - e5 -] [f4 - c5 -]",
  "[c5 b4 a4 g4] [f4 g4 a4 b4] [c5 - - -] [g5 - - -]",
  "[c5 - b4 -] [a4 - g4 -] [f4 - e4 -] [d4 - c4 -]", 
  "[g4 - f4 -] [e4 - d4 -] [c4 - - -] [- - - -]"
]
const base = stack( 
  note((rand.range(0,7).seg(8).slow(8)).pick(pattn)).gain(0.5),
  note("<c3 g2 a2 f2>,<c4 g3 a3 f3>".fast(2)).gain(0.25),
)
$:base.s("gm_tubular_bells|gm_vibraphone").slow("<16 8 16>")
  .delay(1/2).dt(1/4).dfb(1/2).room(1/4)
  .lpf(1600)
$_:base.s("gm_percussive_organ|gm_epiano1").slow("<2 4 2>").late("<1/2 -1/2>")
  .lpf(900).room(1/4)
  .sometimes(x=>x.rev())
$_:base.s("gm_kalimba|gm_synth_strings_2").slow("<0.5 1 0.5>")
  .someCycles(x=>x.transpose(7))
  .decay(1)
  .lpf(600)

$:s("rd*8").gain(sine.range(0.5,0.25).slow(6))
  .dec(1/4).delay(1/2)
  .sometimesBy(0.2, x=>x.ply(2)) 
//gm_tubular_bells|gm_vibraphone
//gm_percussive_organ|gm_epiano1
//gm_kalimba|gm_synth_strings_2
all(x=>x.postgain(1.1))

```
