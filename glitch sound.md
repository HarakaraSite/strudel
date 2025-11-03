```javascript
setcpm(136/4)

$:sound("bd*4".fast(2)).sometimesBy(0.6,x=>x.crush("4|2"))
$:sound("hh*8").swingBy(1/6,8).degradeBy(1/20)
$:sound("- - - sd?*2".fast(2)).decay("0.1|0.3")
  .sometimesBy(0.25,x=>x.dist("8:0.2"))
$_:sound("cp|cb".fast("<4 8 4 2>")).sometimesBy(0.22,x=>x.dist("8:0.2"))
  .sometimesBy(0.3,x=>x.lpf("600|1200".slow(2)).lpq("<5 20 10>"))
$_:note("c3 c3 c2 c2".fast("<2 8>"))
  .sometimesBy(0.7,x=>x.vowel("<a i a i a i>".fast(7)))
  .lpf(perlin.range(300,900).slow(3)).lpq("0|20")
  .sound("supersaw").vib(4).n(1).gain(0.6)

all(x=>x.postgain(1))
    
```