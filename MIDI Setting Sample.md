## midi setting

```javascript
setcps(120/60/2)
$_: note("c2 |c#2| d2| d#2").fast(4)
  .midi().midichan(1) //drumrack

$:stack(
  note("c2").struct("1 0 1 0"),
  note("c#2").struct("0 1 0 1"),
  note("d2").struct("1 1 1 1").fast(2),
).midi().midichan(1) //drumrack

$:stack(
  note("c3 c#3 e3 [f3 e2]"),
  ccv(perlin.segment(16).fast(4)).ccn(2), //filter
  //ccv(sine.segment(16).slow(2)).ccn(3), //gain
  ccv(0.2).ccn(3), //gain
).midi().midichan(2) //synth
```

```javascript
setcps(120/60/2)
$: note("c2 |c#2| d2| d#2").fast(4)
  .midi().midichan(1) //drumrack

$_:stack(
  note("c2(3,4)"),
  note("c#2(3,4,1)").degradeBy(0.2),
  note("d2*8").degradeBy(0.3),
).midi().midichan(1) //drumrack

$_:stack(
  note("c3 c#3 e3 f3 e2 c3"),
  ccv(perlin.segment(16).fast(4)).ccn(2), //filter
  //ccv(sine.segment(16).slow(2)).ccn(3), //gain
  ccv(0.2).ccn(3), //gain
).midi().midichan(2) //synth

$:note("c2 a2 f2 e2")
  .control([74, sine.slow(4)])
  .midi().midichan(2)
```
```
////////////////
//strudel as midi controller
//xln life
////////////////
setcps(140/60/4)

//Kick
$: note("35").midi().struct("1 0 1 0 1 0 1 0") 

//sliced samples
$: note("40 39 [38|36|41] 40 39 40 40 39").midi()
  .struct("[1 1 1 1 1 1 1 1]") .degradeBy(0.15)
  .someCyclesBy(0.1,x=>x.rev())
$_: note("37").midi()
  //.struct("0 0 0 1 1 0 1 0".fast(2))
  .euclid(1,8).fast(4)
$_: note("42").midi()
  .struct("0 0 0 0 1 1? 1 1".fast(2))
  .sometimes(x=>x.ply(2))  
$_: note("43").midi()
  .struct("0 1 1? 1? 1 0 0 0".fast(2)) 

// macro control
$: note("10").midi()
  .ccv(perlin.seg(64).slow(22)) //cc value
  .ccn(3) //cc number
$: note("10").midi()
  .ccv(berlin.seg(64).slow(18)) //cc value
  .ccn(4) //cc number

```javascript
