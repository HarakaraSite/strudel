```javascript
setcpm(125/4)

$:stack(
  sound("bd*4"),
  sound("- hh").off(1/8, x => x.pan(sine.slow(6).range(-0.8, 0.8))),
  sound("- - - sd").lpf(2400).degradeBy(0.2).gain(0.7)
).bank("tr606")

$:note("c2 - c2 -").sound("saw").lpf(200).adsr(".1:.9:.2:.4").gain(0.7)

$:chord("0|1|2|3".pick(["- Am7 - -","- Dm7 - -"," Am7 - - -","- Dbm - -"])) 
    .voicing().slow(2).sound("supersaw")
    .adsr(".02:.3:.0:.2")
    .delay(3/8).delayfeedback(0.65).room(0.7)
    .pan(sine.slow(7).range(-0.5, 0.5))
    .lpf(sine.range(500, 2000).slow(8)).lpq(8)
    .gain(0.4)

$_:chord("3|2|1|0".pick(["- Am7 -"," Em7b5 - - "," Fm7 - - ","- - Bbm7"])) 
    .voicing().slow(1).sound("gm_synth_bass_1")
    .adsr(".1:.4:.0:.3")
    .delay(3/4).delayfeedback(0.4).room(0.8)
    .lpf(800)
    .gain(0.3)

$_:chord("0|2|1|3".pick(["Am7 - Am7 -","Dm7 - Dm7 -","Am7 - Am7 -","Dbm - Dbm -"])) 
    .voicing().slow(0.5).sound("gm_fx_sci_fi").transpose(12)
    .adsr(".01:.4:.0:.05")
    .delay(9/16).delayfeedback(0.8).room(0.4) 
    .lpf(sine.range(1200, 1800).slow(4))
    .gain(0.4)
    .pan(sine.slow(4).range(-0.3, 0.3))
    .every(8, x => x.rev()).degradeBy(1/20)

all(x=>x.postgain(1))

```