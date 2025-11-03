```javascript
setcps(140/60/4)

$:sound("sbd*4").decay(0.5)
$:sound("hh*8").gain(berlin.range(0.6,1.0).slow(7))
  .sometimesBy(0.9,x=>x.off(1/16,y=>y.gain(0.1)))
$:sound("rim").sometimesBy(1/20,x=>x.delay(1/8)).degradeBy(0.1)
$:note("d2*4").sound("supersaw").lpf(160)
$:note("c2*4").sound("sine").fm(sine.range(1.1,2.05).slow(5)).fmh(1.51)
  .lpf(berlin.range(240,2400).slow(7))
$:sound("- white".slow(2)).att("0.6|1.5").decay("<0.6 0.3>")
  .lpf(sine.range(900,1800)).lpq(10)
  .degradeBy(0.45).room(1/3)
$:note("c3").sound("tri").lpf("<300 500 700>")
  .att(1.2).degradeBy(0.55).delay(1/2).room(2/3).pan(sine.range(-0.7,0.7).slow(4))
$:note("c4").sound("sine").att(1.1).rel(1).echo(2,1/4,1/5)
  .pan(sine.range(0.3,-0.3).slow(3))
  .dist("2:0.3").degradeBy(0.7)
$:note("c2").sound("saw").fm(1.11).fmh(sine.range(1.71,2.33).slow(5)).degradeBy(0.85)
  .att(1).decay(0.3).rel(1.2).lpf(perlin.range(800,1200).slow(3))
  .lpenv(1).room(4/5).rlp(5000)

all(x=>x.postgain(slider(0,0,1)._scope()))
  


    






















```