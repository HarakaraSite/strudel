```javascript
setcps(170/60/4)
const bd_pttn = [
  "1 1 0 1 1 0 0 1",
  "1 0 [1 1] 0 1 1 0 1",
  "1 [0 1] 0 1 1 0 1 0",
  "1 0 1 [1 0] 1 0 0 1",
]
$:stack(
  sound("bd:7").struct("<0 0 1 2 0 1 3 0>".pick(bd_pttn).rib(irand(7),2))
  .decay(0.7)
  .sometimesBy(1/8,x=>x.ply(2))
  .sometimesBy(1/16,x=>x.late(1/8))
  .sometimes(x=>x.rev()),
  sound("sd:4").struct("1 1 1 1 1 1 1 1").lpf(1800).decay(0.5)
  .sometimesBy(4/5,x=>x.mask(0)).ply("1|1|2|3")
  .sometimesBy(1/8,x=>x.late(1/8)),
  sound("hh:2*8").degradeBy(1/16)
)
$:note("c1 c1 c1 c1").sound("supersaw").lpf(berlin.range(100,300).slow(3))
  .gain(perlin.range(0.9,1.3).slow(5))
  
$:note("c2*4".fast("<2 4 3 8 4 8 4 8>")).sound("supersaw")
  .lpf(sine.range(200, 3000).fast("<7 9 11>")).lpq("15|20|5")
//  .someCyclesBy(1/16,x=>x.transpose(12))
//  .someCyclesBy(2/3,x=>x.mask("0"))

$:note("d3")
  .s("saw").fm(berlin.range(0.7,2.1).slow(5)).fmh(1.12)
  .lpf(sine.range(100, 8000).fast(3)) 
  .gain(perlin.range(0, 0.9).fast(3)).jux(rev).room(1/4)

all(x=>x.postgain(slider(0,0,1)))
```

```javascript
setcps(170/60/4)
const bd_pttn = [
  "1 1 0 1 1 0 0 1",
  "1 0 [1 1] 0 1 1 0 1",
  "1 [0 1] 0 1 1 0 1 0",
  "1 0 1 [1 0] 1 0 0 1",]

$:  sound("bd:7").struct("<0 0 1 2 0 1 3 0>".pick(bd_pttn).rib(irand(7),2))
  .decay(0.7)
  .sometimesBy(1/8,x=>x.ply(2))
  .sometimesBy(1/16,x=>x.late(1/8))
  .sometimes(x=>x.rev())
$:  sound("sd:4").struct("1 1 1 1 1 1 1 1").lpf(1800).decay(0.5)
  .sometimesBy(4/5,x=>x.mask(0)).ply("1|1|2|3")
  .sometimesBy(1/8,x=>x.late(1/8))
$:  sound("hh:2*8").degradeBy(1/16)
$:note("c2 c2 c2 c2").sound("supersaw").fm(1.13).fmh(1.12)
  .lpf(berlin.range(60,300).slow(3))
  .gain(perlin.range(1.1,1.5).slow(5))
  
$:note("c2*4".fast("<2 4 3 8 4 8 4 8>")).sound("supersaw")
  .lpf(sine.range(600, 3000).fast("<7 9 11>")).lpq("10|20|5")
//  .someCyclesBy(1/16,x=>x.transpose(12))
//  .someCyclesBy(2/3,x=>x.mask("0"))

$_:note("d3")
  .s("supersaw").fm(berlin.range(0.7,2.33).slow(5)).fmh(1.15).dist("3:0.3")
  .lpf(sine.range(100, 8000).fast(3)) .lpq("<0 0 5>".slow(2))
  .gain(perlin.range(0, 0.9).fast("<3 1.5>")).jux(rev).delay(1/8).room(1/4)
  .someCyclesBy(2/3,x=>x.mask("0"))

all(x=>x.postgain(slider(0.648,0,1)))
```

```javascript
setcps(170/60/4)
const bd_pttn = [
  "1 1 0 1 1 0 0 1",
  "1 0 [1 1] 0 1 1 0 1",
  "1 [0 1] 0 1 1 0 1 0",
  "1 0 1 [1 0] 1 0 0 1",]

$:stack(
  sound("bd:11").struct("<0 0 1 2 0 1 3 0>".pick(bd_pttn).rib(irand(7),2))
  .decay(0.7)
  .sometimes(x=>x.rev()),
  sound("sd:3").struct("1 1 1 1 1 1 1 1").lpf(1800).decay(0.5)
  .sometimesBy(4/7,x=>x.mask(0)).ply("1|1|1|2"),
  sound("white*8").decay(0.12).hpf(2000).degradeBy(1/16)
)._pianoroll()
$:note("c2 c2 c2 c2").sound("supersaw")
  .lpf(perlin.range(100,200).slow(7))
  .dist("3:0.2")
  
$:note("c2*4".fast("<2 4 3 8 4 3 4 2>")).sound("supersaw")
  .lpf(sine.range(600, 3000).fast("<7 9 11>")).lpq("10|20|5")
  .someCyclesBy(1/4,x=>x.mask("0"))

$:note("d3")
  .s("supersaw").fm(berlin.range(0.7,2.33).slow(5)).fmh(1.15).dist("3:0.15")
  .lpf(sine.range(100, 8000).fast(3)) .lpq("<0 0 5>".slow(2))
  .gain(perlin.range(0, 0.9).fast("<3 1.5>")).jux(rev).delay(1/8).room(1/4)
  .someCyclesBy(4/5,x=>x.mask("0"))

all(x=>x.postgain(slider(0,0,1)))
```

    





















