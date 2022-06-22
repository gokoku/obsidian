#js 

![](image-kn9mtwp6.png)

# 連想配列

オールド

```bash
var products = new Array();
// It looks like I'm adding items to this array,
// but I'm actually adding properties!
products['XBLK-01'] = 'Extra black car paint';
products['XBLK-02'] = 'Extra black outdoor paint';
products['CLK-01'] = 'Generic brand caulking';
```

ES6 からこんなソリューションがあるらしい。

```bash
const products = new Map();
products.set('XBLK-01', 'Extra black car paint');
products.set('XBLK-02', 'Extra black outdoor paint');
products.set('CLK-01', 'Generic brand caulking');
```

# 文字連結

```bash
var employeeDetail = 'Our team includes ' + firstName + ' ' +
 lastName + ' who works on the ' + team + 'team. They/'ve been a' + 
 ' team member since '  + hireDate + '!';
```

ES6からはテンプレート使いましょ。

```bash
const employeeDetail = `Our team includes ${firstName} ${lastName} who works on the ${team} team. They've been a team member since ${hireDate}!`;
```

# ロケール無しで文字比較

```bash
var a = "hello";
var b = "HELLO";
if (a.ToLowerCase() === b.ToLowerCase()) {
  // We end up here, because the lowercase versions of both
  // strings match.
}
```

もっとパワフルなロケールに特化した比較があるらしい。

```bash
const a = "hello";
const b = "HELLO";
if (a.localeCompare(b, undefined, {sensitivity: 'accent'}) === 0) {
  // We end up here, because the case-insensitive strings match.
}
```

undefined は使ってるパソコンのロケール。

accent は　a  á　みたいなやつ。これは違うということで。

# 文字オーバーイテレーション

オールドスクールな文字操作は indexOf()

アドバンスドなやり方は 

```bash
const searchText = 'I know not where I was born, save that the castle was infinitely old and infinitely horrible';
const matches = [...searchText.matchAll('infinitely')];
for (const match of matches) {
  console.log(`Found ${match[0]} at ${match.index}`)
}
```

別な例、split で配列にして、要素ごとに処理を加える例。

```bash
var animalList = 'horses, monkeys, goats, pandas, iguanas';
var animalArray = animalList.split(',');
for (var i = 0; i < animalArray.length; i++) {
  animalArray[i] = animalArray[i].trim();
}
```

こんなときは、map を使う。これは高階関数だね。

```bash
animalArray = animalArray.map(s => s.trim());
```

# Date を使わないタイミングパフォーマンス

使ってるハードウェアでサポートする正確な小数点をカウントするAPI があるらしい。

```bash

const startTime = window.performance.now();

const endTime = window.performance.now();

const elapsedMilliseconds = endTime - startTime;
```

# 正確なテストストリング

null のときや undefined や empty のときにスキップしたいのだが、こうすると、数字の0 のときも、false のときもスキップする。

```bash
if (unknownVariable) {
  /* We get here as long as:
     unknownVariable has been declared
     unknownVariable is not null
     unknownVariable is not the empty string ''
 */
}
```

正確に null か undefined か empty でスキップするには、

```bash
if (typeof unknownVariable === 'string' &&
    unknownVariable.length > 0) {
  // This is a genuine string with text in it
}
```

# 弱いランダム数字

Math.random() よりストロングなランダムが得たいときは Crypto API があるとのこと。

```bash
const randomBuffer = new Uint32Array(1);
window.crypto.getRandomValues(randomBuffer);
const randomFraction = randomBuffer[0] / (0xffffffff + 1);

// Get an integer from min to max.
randomInt = Math.floor(randomFraction*(max-min+1)) + min;
```