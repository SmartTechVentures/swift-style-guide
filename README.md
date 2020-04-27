# Swiftコーディング規約

## バージョン
v4.0.0  
Swift 5 対応版

## 目次
- [はじめに](##はじめに)
- [命名規則](##命名規則)
- [ファイルフォーマット](##ファイルフォーマット)
- [アクセス制御](##アクセス制御)
- [クラス](##クラス)
- [プロパティ](##プロパティ)
- [変数](##変数)
- [型](##型)
- [制御構文](##制御構文)
- [エラーハンドリング・オプショナル](##エラーハンドリング・オプショナル)
- [クロージャー](##クロージャー)
- [ジェネリクス](##ジェネリクス)
- [メモリ管理](##メモリ管理)
- [規約外とした項目](##規約外とした項目)

## はじめに
このコーディング規約は以下を方針として策定しています。
- 一貫性
- 可読性
- 冗長性の排除

## 命名規則
### クラス・構造体・列挙体・プロトコル・ジェネリクスの型パラメータ
キャメルケースとし、先頭を大文字とすること。  
(UpperCamelCase)

**理由**

API Design Guidelinesに従う。

<table>
<tr><th>良い例</th><th>悪い例</th></tr>
<tr>
<td><pre lang=swift>
class SomeClass<T> {
    ...
}

enum SomeEnum {
    ...
}

struct SomeStruct {
    typealias Element = String

    ...
}

protocol SomeProtocol {
    associatedtype Parameter

    ...
}

</pre></td>
<td><pre lang=swift>
class someClass<t> {
    ...
}

enum someEnum {
    ...
}

struct someStruct {
    typealias element = String

    ...
}

protocol someProtocol {
    associatedtype parameter

    ...
}
</pre></td>
</tr>
</table>

### 変数・メソッド・enumのcase
キャメルケースとし、先頭は小文字とすること。  
(lowerCamelCase)
また、副作用に応じた命名とすること。

**理由**

API Design Guidelinesに従う。

<table>
<tr><th>良い例</th><th>悪い例</th></tr>
<tr>
<td><pre lang=swift>
struct SomeStruct {

    let someNumber: Int
    let sortedNumber: Int

    func someMethod() -> String {
        ...
    }

    // 副作用がない場合は同士として命名
    func sort() {
        ...
    }

    // 副作用がある場合は名詞として命名
    func sortedNumber -> Int {
        ...
    }
}

enum SomeEnum {

    case red
    case green
    case blue

    ...
}
</pre></td>
<td><pre lang=swift>
struct SomeStruct {

    let SomeNumber: Int
    let int: Int

    func SomeMethod() -> String {
        ...
    }

    func sortedNumber() {
        ...
    }

    func sort() -> Int {
        ...
    }
}

enum SomeEnum {

    case Red
    case Green
    case Blue

    ...
}
</pre></td>
</tr>
</table>

### クラス名
クラス名にプレフィックスは付けない。

**理由**

Swiftでは名前空間が存在するためクラス名は衝突せず、ただ可読性を下げてしまうため。  

<table>
<tr><th>良い例</th><th>悪い例</th></tr>
<tr>
<td><pre lang=swift>
class SomeClass {

    ...
}
</pre></td>
<td><pre lang=swift>
class STVSomeClass {

    ...
}
</pre></td>
</tr>
</table>


### urlとdateの命名
URL型の場合○○URLを使い、String型であれば○○URLStringとする。  
同様にDate型の場合○○Dateを使い、String型であれば○○DateStringとする。

**理由**

混同しやすいため。

<table>
<tr><th>良い例</th><th>悪い例</th></tr>
<tr>
<td><pre lang=swift>
var thumbnailURL: URL
var imageURLString: String
var lastUpdateDate: Date
var birthdayString: String
</pre></td>
<td><pre lang=swift>
var thumbnailURL: String
var lastUpdateDate: String
</pre></td>
</tr>
</table>

### extensionのファイル名
extensionのみのファイル名は**UIView+○○(機能名).swift**とすること。  
加えて、extensionは機能単位でグルーピングすること。

**理由**

extensionであることを明確にするため。  
機能ごとにextensionを作り、可読性をあげるため。

<table>
<tr><th>良い例</th><th>悪い例</th></tr>
<tr>
<td><pre lang=swift>
extension UIView {

    @IBInspectable
    var cornerRadius: CGFloat {
        get {
            return layer.cornerRadius
        }
        set {
            layer.cornerRadius = newValue
            layer.masksToBounds = newValue > 0
        }
    }

    @IBInspectable
    var borderWidth: CGFloat {
        get {
            return layer.borderWidth
        }
        set {
            layer.borderWidth = newValue
        }
    }

    @IBInspectable
    var borderColor: UIColor? {
        get {
            return layer.borderColor.map { UIColor(cgColor: $0) }
        }
        set {
            layer.borderColor = newValue?.cgColor
        }
    }
}
</pre></td>
<td><pre lang=swift>
extension UIView {

    @IBInspectable
    var borderColor: UIColor? {
        get {
            return layer.borderColor.map { UIColor(cgColor: $0) }
        }
        set {
            layer.borderColor = newValue?.cgColor
        }
    }

    // ↓↓関係ない
    var hasSubview: Bool {
        return !subviews.isEmpty
    }
}
</pre></td>
</tr>
</table>


### コメント
宣言には**ドキュメントとしてコメント**を記載すること。 
ただし、コードを読めばわかる内容であればこの限りでない。

**理由**

可読性をあげるため。


### その他
その他命名に関しては、[API Design Guidelines](https://swift.org/documentation/api-design-guidelines/)を参考にすること。  
以下は*Fundamentals*からの引用です。

> ・**利用の観点から、明確さ**が最大の目標である。  
> メソッドやプロパティなどのエンティティは、一度だけ定義され、何度も使われるものである。  
> これらを利用する際、明確かつ簡潔であるようにAPIを設計しなければならない。  
> APIの評価をする際は、定義を読むだけでは不十分である。  
> コンテキスト上で明確であるよう、APIを使っている例も含めて評価しなければならない。

> ・**明確さは簡潔さよりも重要である。**  
> Swiftのコードは短く書くことができるが、できるだけ少ない文字数で  
> 可能な限り最小のコードを実現することが目標ではない。  
> Swiftにおける簡潔さは  
> 強力な型システムと自然にボイラープレートコードを減らすことができる性質から生まれた、  
> 副次的なものである。



## ファイルフォーマット

### ファイル内の順序
以下の順に書くこと。  
また、以下のそれぞれをグルーピングし、セクション見出しを挿入すること。
- Property
  - Public
  - Internal
  - Private
- IBOutlet
- IBAction
- Method
  - LifeCycle
  - Public
  - Internal
  - Private
- 各protcolを継承する箇所

セクション見出し
```
// MARK: - ~
```

```swift
// 1.import
import UIKit

// 2.protocol
protocol ViewControllerDelegate {

}

// 3.class・struct・enum
final class ViewController: UIViewController {

    // 4.typealias
    // 5.Inner class, enum & struct

    // 6.property
    //
    // open → public → internal → fileprivate → private
    //
    // 6-1.static member
    // |
    // | 6-1-1.let
    // |
    // | 6-1-2.var
    // |
    // | 6-1-3.computed var
    //
    // 6-2.instance member
    // |
    // | 6-2-1.@~
    // |
    // | 6-2-2.let
    // |
    // | 6-2-3.var
    // |
    // | 6-2-3.computed var

    // 7.method
    //
    // open → public → internal → fileprivate → private
    //
    // 7-1.static member
    //
    // 7-2.instance member
    // |
    // | 7-2-1.init
    // |
    // | 7-2-2.life cycle
    // |
    // | 7-2-3.@~
    // |
    // | 7-2-3.others

}

// 8.extension
extension ViewController: UITableViewDelegate {

}
```

**理由**

書く順序を統一することで、参照したい情報へのアクセスを効率的にする。
また、Source Navigatorを使用した検索が容易になるため。

### 変数宣言時の修飾子の順序
以下の順に書くこと。

@~  
↓  
アクセス制御  
↓  
static, class  
↓  
dynamic, lazy, weak  
↓  
let, var

**理由**

統一のため。

### import文の順序
以下の順に書くこと。

フレームワーク  
↓  
ライブラリ

また、同系列内ではアルファベット順に書くこと。

**理由**

コンフリクトを防止するため。  
順序を揃えることで可読性を上げるため。

### スペース
- ブラケットの前後には半角スペースを1つおくこと。
- メソッドの戻り値の->の前後に半角スペースを1つおくこと。
- コロンの前はスペースを入れず、コロンの後ろに1つスペースをいれること。(三項演算子を除く)
- カンマの前にはスペースを入れず、カンマの後ろに1つスペースをいれること。
- 演算子の実装の際は演算子の後ろに1つスペースをおくこと。
- インデントは4スペース
- 行末に空白を残さない

**理由**

一貫性を持たせるため。

<table>
<tr><th>良い例</th><th>悪い例</th></tr>
<tr>
<td><pre lang=swift>
func something() -> Int {
    let some: Int = 0
}

func <| (lhs: Int, rhs: Int) -> Int
func <|< <A>(lhs: A, rhs: A) -> A
</pre></td>
<td><pre lang=swift>
func something()->Int{
    let some :Int = 0
}

func <|(lhs: Int , rhs: Int)->Int
func <|<<A>(lhs: A,rhs: A)->A
</pre></td>
</tr>
</table>

### 改行
- ファイル終端は空行とすること。
- メソッドとメソッドの間は空行とすること。
- 一行に180字を超える場合は適切な位置で改行すること。

**理由**
エラーを防ぐため。一貫性を持たせるため。

<table>
<tr><th>良い例</th><th>悪い例</th></tr>
<tr>
<td><pre lang=swift>
final class ViewController: UIViewController {
    override func viewDidLoad() {
        // ...
    }

    override func viewWillAppear(animated: Bool) {
        // ...
    }
}

</pre></td>
<td><pre lang=swift>
final class ViewController: UIViewController {
    override func viewDidLoad() {
        // ...
    }
    override func viewWillAppear(animated: Bool) {
        // ...
    }
}
</pre></td>
</tr>
</table>

### classとstructの使い分け
下記のいずれかに当てはまる場合は`struct`で定義する。
- 複数個の値型のデータをひとまとめにして扱う場合
- 継承する必要がない場合
- メソッドを持っているが、プロパティの簡単な操作を行うだけの処理である場合

一方で、下記のいずれかに当てはまる場合は`class`で定義する。
- propertyに参照型が含まれる場合
- 継承する必要がある場合
- 一意性の必要がある場合
- deinitializerする必要がある場合

**理由**

値型(`class`)と比較して参照型(`struct`)の方がシンプルなため。
代入のコストが小さいため。

### プロトコルの実装
①ひとつのextensionで実装するプロトコルは１つとする。  
②可能な限り継承よりもプロトコルを使用すること。

**理由**

①関連メソッドをグルーピングすることで可読性を上げるため。  
②継承は構造体で利用できないため。また、継承可能なのは１つのクラスのみであるのに対し、プロトコルは複数適用可能であるため。  

<table>
<tr><th>良い例</th><th>悪い例</th></tr>
<tr>
<td><pre lang=swift>
class ViewController: UIViewController {
    ...
}

extension ViewController: UITableViewDataSource {
    ...
}

extension ViewController: UITableViewDelegate {
    ...
}
</pre></td>
<td><pre lang=swift>
class ViewController: UIViewController, UITableViewDataSource {
    ...
}
</pre>
<pre lang=swift>
extension ViewController: UITableViewDataSource, UITableViewDelegate {
    ...
}
</pre></td>
</tr>
</table>

### enumの場所
広く使われるものであれば独立したファイルに宣言すること。  
ある構造に特有のものであればその構造内で宣言すること。

**理由**

enumのある場所を統一するため。

### セミコロンの使用禁止
行末のセミコロンの使用を禁止する。

**理由**

必要ないため。

### 条件文の()の禁止
条件文を()で囲わないこと。

**理由**

必要ないため。

<table>
<tr><th>良い例</th><th>悪い例</th></tr>
<tr>
<td><pre lang=swift>
if names.isEmpty {
    ...
}
</pre></td>
<td><pre lang=swift>
if (names.isEmpty) {
    ...
}
</pre></td>
</tr>
</table>

### todo / fixmeの記載
未対応の機能には以下を記述すること。

```swift
// TODO: 〜
```

修正が必要な場合は以下を記述すること。

```swift
// FIXME: 〜
```

**理由**

実装漏れを防止するため。

## アクセス制御
### アクセス制御
アクセス制御は可能な限りprivateを指定すること。

**理由**

そのクラス内、メソッド内…ということが担保でき、実装変更時の影響範囲を小さくできるため。

## クラス
### final宣言
クラスはfinalをデフォルトとし、継承、オーバーライドをしなければならない正当な必要性がある場合にのみ、変更する。

**理由**

継承、オーバーライド不可なことを明示的に示すことで設計意図を示せるため。  
また、副次的にパフォーマンス向上にも繋がるため。

## プロパティ

### self.でのアクセス
必要な時にのみ使用すること。  
(引数と同じ名前のプロパティ、クロージャ内等)

**理由**

統一し、また冗長性を排除するため。

<table>
<tr><th>良い例</th><th>悪い例</th></tr>
<tr>
<td><pre lang=swift>
struct Person {

  let name: String

  init(name: String) {
      self.name = name
  }

  func printName() {
      print(name)
  }

  let callback: () -> String = {
      return self.name
  }
}
</pre></td>
<td><pre lang=swift>
struct Person {

  let name: String

  func printName() {
      print(self.name)
  }
}
</pre></td>
</tr>
</table>

### getクロージャの省略
算出型プロパティにおいて、ゲッターのみ定義する場合はgetのクロージャを省略すること。

**理由**

不要なため。

<table>
<tr><th>良い例</th><th>悪い例</th></tr>
<tr>
<td><pre lang=swift>
class Person {

    let first: String
    let family: String

    var full: String = {
        return first + family
    }
}
</pre></td>
<td><pre lang=swift>
class Person {

    let first: String
    let family: String

    var full: String = {
        get {
            return first + family
        }
    }
}
</pre></td>
</tr>
</table>

## 変数
### 定数・変数の宣言
var宣言を使用するのは、その値が変わり得る等、明確な理由があるときのみとする。  
それ以外の場合はlet宣言を使用する。

**理由**

より安全なコードにするため。  
意図しない値の変更を防ぐため。

letを使用することでプログラマーが値が変わらないことを確認することができる。  
逆に、varによって宣言された変数が変更されることを予期できる。

## 型

### 型推論
最大限利用すること。

**理由**

冗長性を排除し、シンプルなコードにするため。

<table>
<tr><th>良い例</th><th>悪い例</th></tr>
<tr>
<td><pre lang=swift>
let name = "sample"

let image = UIImage(named: "test")

view.backgroundColor = .blue

let view = UIView(frame: .zero)

enum CustomResult {
    case Success
    case Error
}
var result: CustomResult?
result = .Success

let selector = #selector(viewWillAppear)

※数値を扱う場合は注意
let int = 1            // Int型
let double = 1.0       // Double型
let float:Float = 1.0  // Float型にしたい場合は明記する
</pre></td>
<td><pre lang=swift>
let name: String = "sample"

let image: UIImage = UIImage(named: "test")!

view.backgroundColor = UIColor.blue

let view = UIView(frame: CGRect.zero)

enum CustomResult {
    case Success
    case Error
}
var result: CustomResult?
result = CustomResult.Success

let selector = #selector(ViewController.viewWillAppear)
</pre></td>
</tr>
</table>

### 空配列・空辞書の初期化
空配列・空辞書の初期化の際は、型推論を利用した形で書くこと。

**理由**

統一のため。

<table>
<tr><th>良い例</th><th>悪い例</th></tr>
<tr>
<td><pre lang=swift>
var names = [String]()
var jsonDic = [String: Any]()
</pre></td>
<td><pre lang=swift>
var names: [String] = []
var jsonDic: [String: Any] = [:]
</pre></td>
</tr>
</table>

### 糖衣構文の使用
糖衣構文がある場合はそちらを使用すること。  
Voidに関しては、可読性を理由に例外を認める。

**理由**

統一のため。

<table>
<tr><th>良い例</th><th>悪い例</th></tr>
<tr>
<td><pre lang=swift>
var names: [String]
var jsonDic: [String: Any]
var title: String?
var someClosure: () -> ()
</pre></td>
<td><pre lang=swift>
var names: Array<String>
var jsonDic: Dictionary<String, Any>
var title: Optional<String>
var someClosure: Void -> Void
</pre></td>
</tr>
</table>

### Objective-Cクラスの扱い
文字列はNSString型でなく、String型を使用すること。  
また、文字列長判定にはcharacters.countを使用すること。

NSNumber、NSArray、NSDictionary型も同様に、  
Swiftネイティブな型を利用すること。

**理由**

swiftの型で統一するため。

## 制御構文

### 早期Return
guard文を利用し、例外の場合に早めに制御を返すこと。

**理由**

guardにより制御が返ることを明示し、可読性を上げるため。  
また、ネストを減らすことで書きやすさ読みやすさを上げるため。

<table>
<tr><th>良い例</th><th>悪い例</th></tr>
<tr>
<td><pre lang=swift>
guard hogehoge else {
    return
}
</pre></td>
<td><pre lang=swift>
if hogehoge {

} else {
    return
}
</pre></td>
</tr>
</table>

## エラーハンドリング・オプショナル

### Implicitly Unwrapped 型
利用をIBOutletに限定する。

**理由**

nilアクセスの可能性をできるだけ下げるため。

### アンラップ処理

!(forced unwrap)を使わず、optional chaining, optional binding, nil coalescingを使用すること。  
また、optional bindingの際には、同じ変数名で変数宣言すること。

**理由**

nilアクセスの可能性をできるだけ下げるため。  
アンラップ前の変数を使ってしまうバグを防ぐため。

<table>
<tr><th>良い例</th><th>悪い例</th></tr>
<tr>
<td><pre lang=swift>
final class ViewController: UIViewController {
    override func viewDidLoad() {
        super.viewDidLoad()
        let navigationItem = navigationController?.navigationItem
    }
}
</pre>
<pre lang=swift>
let response: Any = [:]
if let json = response as? [String: Any] {
    ...
}
</pre>
<pre lang=swift>
let someNumber: Int? = 10
let otherNumber: Int = someNumber ?? 0
</pre>
<pre lang=swift>
if let some = some {
    ...
}
</pre></td>
<td><pre lang=swift>
final class ViewController: UIViewController {
    override func viewDidLoad() {
        super.viewDidLoad()
        let navigationItem = navigationController!.navigationItem
    }
}
</pre>
<pre lang=swift>
let response: Any = [:]
let json = response as! [String: Any]
</pre>
<pre lang=swift>
let someNumber: Int? = 10
let otherNumber: Int = someNumber!
</pre>
<pre lang=swift>
if let bindedSome = some {
    ...
}
</pre>
</td>
</tr>
</table>

### try~catch
catchで例外を処理しない場合はtry!ではなく、try?を使用する。

**理由**

nilアクセスの可能性をできるだけ下げるため。

<table>
<tr><th>良い例</th><th>悪い例</th></tr>
<tr>
<td><pre lang=swift>
let someInt = try? someErrorThrowMethod() ?? 0
</pre></td>
<td><pre lang=swift>
let someInt = try! someErrorThrowMethod()
</pre></td>
</tr>
</table>

## クロージャー

### クロージャーの表記

①関数の最後の引数として関数にクロージャを渡す必要があって、かつクロージャが１つの場合には後置クロージャを使用する。  
　またクロージャ内のパラメータは明確に意味がわかるようにする。（一行で記述する場合に限り、引数名を省略すること。）  
②クロージャが関数やメソッドの唯一の引数の場合は、関数名やメソッド名の後ろの（）を省略する。  
③後置クロージャを持つメソッドでメソッドチェーンで記述する場合は、スペースを入れる or 改行するで統一する。  

**理由**

①複数のクロージャを引数にもつ場合に後置クロージャを使用すると可読性が落ちるため。  
　複数行のクロージャ内で引数名を省略した場合に可読性が落ちるため。  
　複数のクロージャを引数にするメソッドの呼び出しでtrailingクロージャの省略形を使うと、外部引数名が省略され、  
　可読性が落ちるため。  
②省略可能なため。  
③実装者によって記述が異なることを防ぐため。  

<table>
<tr><th>良い例</th><th>悪い例</th></tr>
<tr>
<td><pre lang=swift>
let someClosure: String -> Int? = { Int($0) }

let someClosure: String -> Int? = { string in

    let intValue = Int(string)
    return intValue
}

UIView.animate(withDuration: 10.0) {
    print("animate")
}

UIView.animate(withDuration: 10.0,
               animations: {
                print("animate")
},
               completion: { _ in
                print("completed")
})

let value = numbers.map {$0 * 2}.filter {$0 > 50}.map {$0 + 10}
</pre></td>
<td><pre lang=swift>
let someClosure: String -> Int? = {

    let intValue = Int($0)
    return intValue
}

UIView.animate(withDuration: 10.0, animations:{
    print("animate")  
})

UIView.animate(withDuration: 10.0,
               animations: {
                print("animate")
}) { _ in
    print("competed")
}
</pre></td>
</tr>
</table>

## ジェネリクス

### ジェネリクスの活用
可能な限りジェネリクスを活用すること  

**理由**

汎用性が高まるため、またコード量を削減できるため。  

Tupleを作成するメソッド  
<table>
<tr><th>良い例</th><th>悪い例</th></tr>
<tr>
<td><pre lang=swift>
func makeTuple<T> (a: T, b: T) -> (T, T) {
    return (a, b)
}
</pre></td>
<td><pre lang=swift>
// Intの場合
func makeTuple (a: Int, b: Int) -> (Int, Int) {
    return (a, b)
}

// Stringの場合
func makeTuple (a: String, b: String) -> (String, String) {
    return (a, b)
}
</pre></td>
</tr>
</table>

## メモリ管理

### 弱参照
弱参照をする際にはunownedではなくweakを使用すること。

**理由**

nilアクセスの可能性をできるだけ下げるため。

[weak self]・・・弱参照先がメモリ解放されている場合にnilになる（Optional型と同様にアンラップが必要）  
[unowned self]・・・Optional型ではないため、弱参照先がメモリ解放されている場合にクラッシュする  

<table>
<tr><th>良い例</th><th>悪い例</th></tr>
<tr>
<td><pre lang=swift>
someFunc { [weak self] in
    guard let weakSelf = self {
      return
    }
    let sampleTuple = weakSelf.makeTuple(a: "A", b: "B")
    print(sampleTuple)
    ...
}

someFunc { [weak self] in
    if let weakSelf = self {
      let sampleTuple = weakSelf.makeTuple(a: "A", b: "B")
      print(sampleTuple)
    } else {
      print("Error")
    }
    ...
}
</pre></td>
<td><pre lang=swift>
someFunc { [weak self] in
    let sampleTuple = self?.makeTuple(a: "A", b: "B")
    print(sampleTuple!)
    ...
}

someFunc { [unowned self] in
    ...
}
</pre></td>
</tr>
</table>

## 規約外とした項目

### warningを解消する
warningを可能な限り解消すること。

**却下理由**

Xcodeに関する事項であり、コーディングの規約としては不適切と判断した。

### 関数型のメソッドに関する制約
関数型の使用を制限する。

**却下理由**

プロジェクトごとに判断されるべきものであり、コーディング規約としては不適切と判断した。

### AutoLayoutに関する制約
AutoLayoutを使用する際に関する規約。

**却下理由**

各プロジェクトに裁量を任せることとし、コーディング規約の対象外とした。

### 頭文字語に関する制約
頭文字語はすべて大文字またはすべて小文字とすること。

**却下理由**

変数名やクラス名等の規約と重複しているため、不要と判断した。

### 不要なコードの削除
テンプレートで実装されているメソッドやコメントで不要なコードは削除する.

**却下理由**
`override func didReceiveMemoryWarning()`を削除することを規定していたが、Xcoode10からはテンプレートに含まれなくなったため、不要と判断した。

### 関数の宣言
エディター上で折り返したら改行すること。  
また、改行をいれたらすべての引数の前で改行すること。

**却下理由**

改行に関わる規約を追記したため、不要と判断した。