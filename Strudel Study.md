Strudelにおける確率に関する関数、ステートメント、およびオペレータは、パターンにランダムな振る舞いや確率的な要素を導入するために使用されます。これらは、音の選択、イベントの発生、パラメータの変調など、様々な側面でランダム性を加えることができます。

以下に、関連する機能とその詳細を整理して説明します。

### 1. ランダムな値の選択 (Random Value Selection)

これらの関数は、指定されたリストや範囲からランダムな値を選択するために使用されます。

-   **`choose(xs)`**
    -   与えられた要素のリストからランダムに1つを選択します。
    -   `xs`は選択肢となる値やパターンのリストです。
    -   例: `note("c2 g2!2 d2 f1").s(choose("sine", "triangle", "bd:6"))`
-   **`wchoose(pairs)`**
    -   与えられた要素のリストから、各要素に設定された**確率（重み）**に基づいてランダムに1つを選択します。
    -   `pairs`は値と重みの配列です。
    -   例: `note("c2 g2!2 d2 f1").s(wchoose(["sine",10], ["triangle",1], ["bd:6",1]))`
-   **`chooseCycles(items)`** (別名: `randcat`)
    -   **各サイクル**において、与えられた要素の中からランダムに1つを選びます。
    -   例: `chooseCycles("bd", "hh", "sd").s().fast(8)`
-   **`wchooseCycles(pairs)`** (別名: `wrandcat`)
    -   **各サイクル**において、各要素に設定された**確率（重み）**に基づいてランダムに1つを選びます。確率はそれ自体がパターンであることも可能です。
    -   例: `wchooseCycles(["bd",10], ["hh",1], ["sd",1]).s().fast(8)`

### 2. 確率的なイベントの制御 (Probabilistic Event Control)

これらの関数は、パターン内のイベントの発生や、関数が適用されるかどうかを確率的に制御します。

-   **`degradeBy(amount)`**
    -   パターンからイベントを**ランダムに削除**します。
    -   `amount`は0から1の間の数値で、削除される確率を決定します。0は0%の削除、1は100%の削除を意味します。
    -   例: `s("hh*8").degradeBy(0.2)`
-   **`degrade()`**
    -   `degradeBy(0.5)`の省略形であり、イベントの50%をランダムに削除します。
-   **`undegradeBy(amount)`**
    -   `degradeBy`の**逆**の機能です。0は100%の削除（`degradeBy`で削除されるイベントは通過する）、1は0%の削除を意味します。
    -   例: `s("hh*8").undegradeBy(0.2)`
-   **`undegrade()`**
    -   `undegradeBy(0.5)`の省略形であり、イベントの50%をランダムに通過させます。
-   **`sometimesBy(probability, function)`**
    -   与えられた関数を**指定された確率**でランダムに適用します。
    -   `probability`は0から1の数値またはパターンです。
    -   例: `s("hh*8").sometimesBy(.4, x=>x.speed("0.5"))`
-   **`sometimes(function)`**
    -   `sometimesBy(0.5, fn)`の省略形であり、50%の確率で関数を適用します。
-   **`someCyclesBy(probability, function)`**
    -   `sometimesBy`に似ていますが、関数を**サイクルごと**にランダムに適用します。
-   **`someCycles(function)`**
    -   `someCyclesBy(0.5, fn)`の省略形です。
-   **`often(function)`**
    -   `sometimesBy(0.75, fn)`の省略形です。
-   **`rarely(function)`**
    -   `sometimesBy(0.25, fn)`の省略形です。
    -   例: `chords.segment(4).clip(rand.range(.4,.8)).rarely(ply("2"))`
-   **`almostNever(function)`**
    -   `sometimesBy(0.1, fn)`の省略形です。
-   **`almostAlways(function)`**
    -   `sometimesBy(0.9, fn)`の省略形です。
-   **`never(function)`**
    -   `sometimesBy(0, fn)`の省略形であり、関数は**決して**呼び出されません。
-   **`always(function)`**
    -   `sometimesBy(1, fn)`の省略形であり、関数は**常に**呼び出されます。

### 3. 連続的なランダム信号 (Continuous Random Signals)

これらの信号は、連続的なランダムな数値ストリームを提供し、パラメータの動的な変調によく使用されます。

-   **`rand`**
    -   0から1の間のランダムな数を生成する**連続的なパターン**です。
    -   例: `s("bd*4,hh*8").cutoff(rand.range(500,8000))`
-   **`perlin`**
    -   0から1の範囲の**パーリンノイズ**を生成する連続的なパターンです。より滑らかなランダムな動きに適しています。
    -   例: `s("bd*4,hh*8").cutoff(perlin.range(500,8000))`
-   **`irand(n)`**
    -   0から`n-1`までのランダムな**整数**を生成する連続的なパターンです。
    -   `n`は排他的な最大値です。
    -   例: `n(irand(8)).struct("x x*2 x x*3").scale("C:minor")`
-   **`brand`**
    -   0または1の値を生成する**バイナリランダム**な連続パターンです。
    -   例: `s("hh*10").pan(brand)`
-   **`brandBy(probability)`**
    -   0または1の値を生成するバイナリランダムな連続パターンですが、値が1になる**確率**を指定できます。
    -   `probability`は0から1の数値です。
    -   例: `s("hh*10").pan(brandBy(0.2))`

### 4. Mini-Notationのオペレータ (Mini-Notation Operators)

-   **`?` オペレータ**
    -   Mini-Notation内で`degradeBy`の短縮形として使用されます。
    -   例: `s("[hh?0.2]*8")` は `s("hh*8").degradeBy(0.2)` と同じ意味です。`s("[hh?]*8")` は `s("hh*8").degrade()` と同じです。

これらの機能を使うことで、Strudelのパターンに複雑で予測不能な、しかし制御されたランダム性を加えることができ、より表現豊かな音楽の可能性が広がります。