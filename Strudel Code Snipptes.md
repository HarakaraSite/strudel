## 20250629

```javascript
setcps(120/60/2) //cycles per minute


$_:note("c3 c3 c4 c3").sound("supersaw")
$_:sound("glitch").loop(1).gain(0.2)
$_: n("<0 2 4 6>")
  .scale("d:minor")
  .s("supersaw")
  .dist(2).seg(8)
  .gain(0.4)
  .lpf(100).lpq(7)
  .clip(0.7)


$:stack(
  sound("bd!4?0.2").lpf("1200 | 500 | 900").lpq(5),
//  sound("- [rim [lt lt]]").gain(0.5),
  sound("{hh!3?0.3 cp!2?0.1 [hh!2]!2 hh}%4").hpf("1200 | 2000 | 2500").hpq(5),
  // sound("casio").struct("1?0.3 1 1?0.6").decay(0.3),
  // sound("metal*5").degradeBy(0.2).speed("0.2 | 0.1 | 0.3".fast(4)).gain(0.2)
//  sound("glitch").chop(4).loopAt(4)
)._punchcard()
```

```javascript
samples('github:tidalcycles/dirt-samples')
```

```javascript
setcps(120/60/2) //cycles per minute

//Probability
samples('github:tidalcycles/dirt-samples')
$:note("c3 c4").sound("supersaw").scope()
//$:note("c3").sound(wchoose(["sin",5],["tri",1],["sqr",1])).scope()
//let snd = chooseCycles("bd", "hh", "sd")
let snd = wchooseCycles(["bd",3], ["hh?",7], ["sd",1])
let snd2 = wchooseCycles(["ht",1], ["mt",3], ["lt",1])
$:stack(

//  chooseCycles("bd", "hh", "sd").sound().fast(4),
  sound(snd).fast(8),
  sound(snd2).fast(8).degradeBy(0.5).gain(tri.range(0.5,0.9).slow(2)),
//  sound("bd - bd -"),
//  sound("hh*8"),
  
).bank("RolandTR606")._punchcard()
```
