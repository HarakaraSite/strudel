```javascript
setcps(120/60/4)

$: note(`[c3 e4 g4 b4 d5 b4 g4 e4]
         |[a3 c#4 e4 g4 a4 g4 e4 c#4]
         |[d3 f#4 a4 c5 d5 c5 a4 f#4]
         |[g3 b4 d5 f5 g5 f5 d5 b4]`)
   .sound("gm_piano").gain(0.3).sometimesBy(0.15,x=>x.early(1/30))
   .someCyclesBy(0.25,x=>x.fast("1.5|0.5"))
   .pianoroll({ labels: 1 })

$: note("<d2 f2 [eb2 e2] f2 [g2 a2 b2 c3] [ab2 a2] bb2 c3 e3 [d3 db3] c3>*4")
   .someCyclesBy(0.10,x=>x.rev())
   .someCyclesBy(0.05,x=>x.fast(2))
   .sound("gm_acoustic_bass").lpf(600).gain(0.9).decay(0.5)
   .swing(2)

$: sound("bd*2 [~ sd] [~ bd] sd").gain(0.8)
   .swing(2)

$: sound("[hh hh]*4").gain("[0.5 0.3]*4")
   .swing(2)

all(x=>x.postgain(slider(0,0,1)))
```