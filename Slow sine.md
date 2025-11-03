```javascript
setcps(60/60/4)

$:stack(
 note("<c2 e2>").sound("sine"),
 note("<c3 e3>").sound("sine").gain(0.5),
 note("<g3 b3>").sound("sine").gain(0.25),
 note("<e4 gs4>").sound("sine").gain(0.15),
 note("<<c2 e3> <e3 c2>>".late(1)).fast(2).sound("sine").gain(0.0625),
).postgain(0.4).delay(0.75).room(0.3).gain(0.3).fast(2)
  .fm(1).fmh(2)
  .lpf(sine.range(100,500).seg(4).slow(4))
  .lpq(10) 

$:stack(
 note("<c2 e2>").sound("sine"),
 note("<c3 e3>").sound("sine").gain(0.5),
 note("<g3 b3>").sound("sine").gain(0.25),
 note("<e4 gs4>").sound("sine").gain(0.15),
 note("<<c2 e3> <e3 c2>>".late(1)).fast(2).sound("sine").gain(0.0625),
).postgain(0.2).delay(0.75).room(0.3).gain(0.3)
  .penv(2)
  .lpf(sine.range(60,200).seg(14).fast(4))
  .lpq(15)

$:stack(
 note("<c2 e2>").sound("sine"),
 note("<c3 e3>").sound("sine").gain(0.5),
 note("<g3 b3>").sound("sine").gain(0.25),
 note("<e4 gs4>").sound("sine").gain(0.15),
 note("<<c2 e3> <e3 c2>>".late(1)).fast(2).sound("sine").gain(0.0625),
).postgain(0.6).delay(0.75).room(0.3).gain(0.3)
  .lpenv(4)
  .lpf(sine.range(200,300).seg(10).fast(2))
  .lpq(15)

$:sound("pink").decay(0.1).hpf(800)
```
```javascript
setcps(60/60/4)
const base = stack(
 note("<c2 e2>").sound("sine"),
 note("<c3 e3>").sound("sine").gain(0.5),
 note("<g3 b3>").sound("sine").gain(0.25),
 note("<e4 gs4>").sound("sine").gain(0.15),
 note("<<c2 e3> <e3 c2>>".late(1)).fast(2).sound("sine").gain(0.0625),
).delay(0.75).room(0.3)

$_:base.postgain(sine.range(0.1,0.5).slow(3)).fast(4).jux(rev)
  .fm(2).fmh(2)
  .lpf(sine.range(100,600).seg(5).slow(2)).lpq(15)

$:base.postgain(0.2).fast(4).phaser(4)
  .penv(2)
  .lpf(tri.range(60,200).seg(14).fast(2)).lpq("<5 10 15 20 25>".fast(8))

$:base.postgain(0.2).fast(2).decay(16)
  .lpenv(2)
  .lpf(sine.range(200,400).seg(3).fast(8)).lpq(20)

$_:base.postgain(0.2).decay("<1 8>").fast(2)
  .lpf(perlin.range(400,900).seg(5).slow(3)).lpq(25)

all(x=>x._scope()
```
```javascript
setcps(60/60/4)
const base = stack(
 note("<c2 e2>").sound("tri"),
 note("<c3 e3>").sound("tri").gain(0.5),
 note("<g3 b3>").sound("tri").gain(0.25),
 note("<e4 gs4>").sound("tri").gain(0.15),
 note("<<c2 e3> <e3 c2>>".late(1)).fast(2).sound("tri").gain(0.0625),
).delay(0.75).room(0.3)

$:base.postgain(0.2).fast(4).decay(0.5).transpose("<0 7 0 12>")
  .fm(2).fmh(2)
  .lpf(sine.range(300,600).slow(2)).lpq(15)
$:base.postgain(0.2).fast(2).decay(1).jux(rev)
  .lpf(400).lpq("<5 10 15>").penv(1)
$:base.postgain(0.2).fast(4).decay(2).lpenv(2)
  .lpf(sine.range(700,900).fast(7)).lpq(15)
$:base.postgain(0.2).fast(1).decay(1)
  .lpf(perlin.range(100,800).fast(7)).lpq(20).phaser(2)
all(x=>x._scope()
```
```javascript
setcps(60/60/4)

const base = stack(
  stack(
  note("<a1 f1 c2 g1>").sound("saw"),
  note("<a2 f2 c3 g2>").sound("saw").gain(0.5),
  note("<e3 c3 g3 d3>").sound("saw").gain(0.25),
  note("<c4 a3 e4 b3>").sound("saw").gain(0.15)
  ).pan(sine.range(-0.4, 0.4).slow(8)),
  note("<[e5 c5] [c5 g5] [g5 d5] [d5 a5]>")
  .sometimes(x=>x.rev())
  .fast(1.5).sound("sqr")
  .late("<1 1.25 0.75 1.125>")  
  .early("<0 0.05 -0.05 0.02>") 
  .gain(perlin.range(0.15,0.5).slow(4)).pan("-0.8|0.8")
)

$:base
  .transpose("<0 2 7 5 12 0>".slow(2))
  .lpf(tri.range(300,400)).lpq(10)
  .sometimesBy(0.4,x=>x.mask("0"))
$:base.slow(2)
  .lpf(600).lpq(10).phaser(1.2).ply(4)
  .hpf(sine.range(80,120).slow(16)) 
  .sometimesBy(0.2,x=>x.mask("0"))
$:base.fast(1.5)
  .lpf(sine.range(200,300).slow(2)).lpq(15)
  .sometimesBy(0.3,x=>x.mask("0"))
$:base.fast(4)
  .lpf(perlin.range(300,400).fast(16)).lpq(15)
  .sometimesBy(0.3,x=>x.mask("0"))

all(x=>x.room(1/4).dist(0.05).postgain(1/4))

```

```javascript
setcps(120/60/4)

const melodyPatterns = [
  // クロマチック・アプローチ
  "[g4 g#4 a4] [e4 f4 g4] [f4 e4 f4] [f4 e4 b4]",
  // 7th、9thを強調
  "[b4 d5] [g4 c5] [a4 c5] [f4 a4 d5]",
  // シンコペーション + ブルーノート
  "[~ g4 bb4] [e4 ~ g4] [f4 ab4] [~ f4 b4]",
  // 3連符でスウィング感
  "[g4 b4 d5]/3 [e4 g4]/2 [f4 a4 c5]/3 [b4]/1",
  // ジャズ的アルペジオ
  "[g4 b4 d5 f#5] [e4 g4 c5] [f4 a4] [b4 d5 f4]",
  // 半音階的下降
  "[b4 bb4 a4] [g4 f#4 g4] [f4 e4 eb4] [f4 g4 b4]",
  // 複雑なリズム + 9th
  "[g4*2 b4] [e4 ~ g4 c5] [f4/2 a4/2 d5] [f4 b4]",
  // モダンジャズ風
  "~ [e4 bb4 d5] [f4 ~ a4] [b4 eb5 g4]"
]

const base = stack(
  stack(
  note("c1 a1 d1 g1").sound("sine"),
  note("c2 a2 d2 g2").sound("sine").gain(0.5),
  note("g3 e3 f3 b3").sound("sine").gain(0.25),
  note("b3 g3 a3 f3").sound("sine").gain(0.15)
  ),
  note(chooseCycles("0","1","2","3","4","5","6","7").inhabit(melodyPatterns))
  .fast(1.5).sound("sine")
  // ジャズ的なタイミング調整
  .sometimesBy(0.3, x=>x.early(1/32)) // 微妙な前ノリ
  .sometimesBy(0.2, x=>x.late(1/32))  // 微妙な後ノリ
).dist(0.05)

$:base
  .lpf(tri.range(600,900)) // 少し暗めの音色でジャズ感
  .sometimesBy(0.15,x=>x.mask("0"))
$:base.late(1/16)
  .lpf(tri.range(200,300)) 
  .sometimesBy(0.125,x=>x.rev())
  .someCyclesBy(0.20,x=>x.mask("0"))

all(x=>x.room(1/4).postgain(1/4))
```

```javascript
setcps(120/60/4)
const pattn = [
  "[e5 [g5 c5]] [b4 d5 g5 b5] [[c5 e5] [a5 e5]] [[f5 a5] c6 [a5 f5]]",
  "[g5 e5 c5] [d5 b4 g5] [a5 e5 c5] [c6 a5 f5]",
  "[[e5 g5] c6 e5] [g5 [b5 d5] g5] [[c5 a5] e5 a5] [[f5 c6] a5 f5]",
  "[c5 e5] [g5 b4] [e5 a5] [[f5 a5] c6]"
]
const base = stack(
  stack(
  note("<c2 g1 a1 f1>").sound("sqr|sin|saw"),
  note("<c3 g2 a2 f2>").sound("sin|saw|sqr").gain(0.5),
  note("<g3 d3 e3 c3>").sound("saw|sin|sqr").gain(0.25),
  note("<e4 b3 c4 a3>").sound("saw|sqr|sin").gain(0.125)
  ),
note("1|2|3|0".pick(pattn)).slow(2)
  .sound("sin|saw|sqr").gain(0.125)
).room(2/3).delay(1/3).sometimes(x=>x.room(3/4))

$:base.decay("1/4|5/4")
  .lpf(perlin.range(600,900).slow(4)).penv(0.5)
$_:base.decay("<1/3 4/3>").slow(4)
  .lpf(sine.range(200,2400).slow(6)).sometimes(x=>x.lpq(10))
$_:base.decay(1/4).slow(2).pan("<-0.3 0.3>")
  .lpf(tri.range(100,600).slow(2)).lpenv(1)
  .sometimesBy(0.2,x=>x.rev())
  .sometimesBy(0.3,x=>x.ply("2|4"))
$_:base.decay(1/4).fast(2).pan("<0.5 -0.5>")
  .transpose("<0 7 0 12 0 0 7 12>").sometimes(x=>x.late(1/16))
$:s("sbd*4").decay(0.5).gain(0.3)

all(x=>x.postgain(1/3))
```