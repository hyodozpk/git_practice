# りあくと第３版vo2まとめ
## 第 5章 JSX で UI を表現する 
### 5-1. なぜ React はJSXを使うのか
#### JSXの本質を理解する 
- JSXはBabelやtscによってコンパイルされることを前提としたシンタックスシュガー
- JSXはTSにおいて`ReactElement`というオブジェクト生成するためのJSの拡張リテラル

#### なぜReactは見た目とロジックを混在させるのか 
- 機能単位で分割された独立性の高いパーツ(コンポーネント)を組み合わせることでアプリケーションを構築しようというのがReactの開発思想
    - コンポーネントごとに見た目とロジックを閉じ込める必要がある
#### なぜReactはテンプレートを使わないのか 
- 一般的なプログラミングにおける1ファイルは最大100 行、理想は50 行以内という目安
- JSXが純粋な式だからこそのメリットは、静的解析や型推論に適していること
- TSのコンパイルオプション`jsx`を`react`に指定するとtsc自身がJSXを`React.createElement()`の関数コールに変換してくれる
#### なぜReactはViewをタグツリーで表現するのか 
- Reactのレンダラー例
    - React DOM…… HTML DOM（公式標準パッケージ）
    - react-test-renderer …… JavaScriptオブジェクト（公式標準パッケージ）
    - React ART……HTML5 CanvasやSVG などのベクターグラフィック（公式標準パッケージ）
    - React Native…… iOS および Androidのネイティブアプリケーション
    - React Native for Windows + macOS…… Windows および macOSのネイティブアプリケーション
    - React 360 …… ブラウザ上で動く VR アプリケーション
    - React-pdf…… PDFドキュメント
    - react-three-fiber…… WebGL による3Dグラフィック
    - React Figma……Figma プラグイン
- React とはWeb にとどまらずアプリケーションやドキュメントを包括的に抽象化するものであり、それを各プラットフォームに合わせて具現化するためのものがレンダラー

### 5-2. JSXの書き方
#### JSXの基本的な文法 
- Create React App で作成したプロジェクトにはESLintというコードの静的解析ツールとその共有設定であるeslint-config-react-app28 が最初から組み込まれていて、React における構文の簡単なまちがいを指摘してくれるようになってる
- JSXの構文を書く前にReactのインポートが必要
- JSXをTSベースにするときは拡張子を`tsx`にする
- 式の埋め込みには`{}`を使う
    - `if`や`for`など値を返さない制御文はかけない
    - `null` `undefined` `true``false`は何も出力されない
- `//`は閉じカッコもコメントアウトしてしまうので1行にまとめたいときは`/* */`
- JSXでは複数の要素が含まれるときトップレベルが一つの要素である必要がある
    - `<div>`で囲っても良いが余計なノード階層ができるのでフラグメントを使う
    ``` JSX
        const elems = (
    <>
    <div>foo</div>
    <div>bar</div>
    <div>baz</div>
    </>
    );
    ```    

#### JSXとコンポーネントの関係 
- propsとは『Properties（プロパティ）』の略で、コンポーネントを関数として考えたとき、その引数に相当するもの
- `children`はJSXにおいて属性値ではなく子要素として記述され呼ばれた側のコンポーネントでは暗黙のpropsとして渡される
- propsに値を渡す際は`{}`で囲むが、文字列は`""`でも渡せる
- JSXの属性値の記述は`true`の場合は記述を省略可能

#### Reactの組み込みコンポーネント 
- Reactのコンポーネントにはユーザー定義コンポーネントと組み込みコンポーネントの2種類がある
- ユーザー定義の場合は命名規則があり1文字目を必ず大文字にする
- 小文字は組み込みコンポーネントと認識される
- JSの予約語とかぶったもの
    - `className`
    - `htmlFor`
- HTMLタグ床となり値が真偽値のもの
    - `checked`
    - `disabled`
    - `selected`
- Reactでは`<textarea>`や`<select>`も`value`を持てる
- HTMLには存在せず組み込みコンポーネントにのみ存在する属性
    - `ref` リアルDOMへの参照が可能
    - `key` 繰り返し処理で同階層に同じ要素のリストを表示させる際、Reactはユニークなkey 属性値を必要とする
        - 文字列か数値が利用可能
    - `key`属性はReactが差分検出に使うのでインデックスを`key`に使うのは非推奨、ユニークIDなどが良い

## 第 6章 Linter とフォーマッタでコード美人に
### 6-1. ESLint 
#### JavaScript、TypeScriptにおける Linterの歴史 
- Linterはコードを静的解析してコンパイルではじかれない潜在的なバグを警告するも
の
- コードフォーマッタはインデントや改行などのスタイルを一律に自動整形してくれる
#### ESLint の環境を作る 
- TypeScript ESLintの設定ファイルにおける各プロパティ
    - env…… プログラムの実行環境をESLint に教える。個別の環境ごとにglobals の値がプリセットされている
    - extends…… 共有設定を適用する。共有設定はESLint に標準で含まれているものか別途インストールしたもの、またはインストール済みプラグインのパッケージに含まれているものを指定する。なおここに記述した共有設定間でルール設定が重複している場合、リストの後ろに記述されたほうが優先される
    - parser…… ESLint が使用するパーサを指定する
    - parserOptions…… パーサの各種実行オプションを設定する
    - plugins…… 任意の（インストール済みの）プラグインを組み込む
    - rules…… 適用する個々のルールと、エラーレベルや例外などその設定値を記述する

#### ESLint の適用ルールをカスタマイズする
```JavaScript
extends: [
'plugin:react/recommended',
'airbnb',
+ 'airbnb/hooks',
+ 'plugin:import/errors',
+ 'plugin:import/warnings',
+ 'plugin:import/typescript',
+ 'plugin:@typescript-eslint/eslint-recommended',
+ 'plugin:@typescript-eslint/recommended',
+ 'plugin:@typescript-eslint/recommended-requiring-type-checking',
],
︙
plugins: [
'@typescript-eslint',
+ 'import',
+ 'jsx-a11y',
'react',
+ 'react-hooks',
],
```
- ESLintプラグインはインストールするだけでなくルールを適用しないと有効にならない
- 各プラグイン推奨の共有設定を`extends`に記述
- 共有設定感ではあとに記述されたものが上書きする
- 他のルールセットと依存の順番がある場合はそれぞれドキュメントで言及してるはずなので、新しくプラグインや拡張ルールセットをインストールするときは、npm のページくらいにはざっと目を通す

``` JavaScript
parserOptions: {
ecmaFeatures: {
jsx: true
},
- ecmaVersion: 12,
+ ecmaVersion: 2020, //、@typescript-eslint/parserの最新版4.0.1で動作させるため
+ project: './tsconfig.eslint.json',　//型情報が要求されるルールに必要
sourceType: 'module',
+ tsconfigRootDir: __dirname,
},
︙
+ root: true,
```
- `tsconfig.eslint.json`という別ファイルを用意してチェックするファイルを限定する
- この設定をしないとパーサがnpmパッケージまでパースしてしまいパフォーマンスが落ちる
- `root`オプションでESLintが親ディレクトリの設定ファイルの読み込みを無効にする
- `rules`オプションで`extends`で読み込んだ共有設定の値を書き換える
``` JavaScript
rules: {
'lines-between-class-members': [
'error',
'always',
{
exceptAfterSingleLine: true
}
],
'no-void': 'off',
'padding-line-between-statements': [
'error',
{
blankLine: 'always',
prev: '*',
next: 'return',
},
],
'@typescript-eslint/no-unused-vars': [
'error',
{
'vars': 'all',
'args': 'after-used',
'argsIgnorePattern': '_',
'ignoreRestSiblings': false,
'varsIgnorePattern': '_',
},
],
'import/extensions': [
'error',
'ignorePackages',
{
js: 'never',
jsx: 'never',
ts: 'never',
tsx: 'never',
}
],
'react/jsx-filename-extension': [
'error',
{
extensions: ['.jsx', '.tsx']
}
],
'react/jsx-props-no-spreading': [
'error',
{
html: 'enforce',
custom: 'enforce',
explicitSpread: 'ignore',
},
],
},
overrides: [
{
'files': ['*.tsx'],
'rules': {
'react/prop-types': 'off',
},
},
],
settings: {
    'import/resolver': {
node: {
paths: ['src'],
},
},
},
};
```
- `lines-between-class-members`…… クラスメンバーの定義の間に空行を入れるかどうかを定義するルール。`eslint-config-airbnb` で常に空行を入れるように設定されていたのを、ここでは1 行記述のメンバーのときは空行を入れなくていいようにしている
- `no-void`…… void 演算子の（式としての）使用を禁ずるルール。Effect Hook57 内で非同期処理を記述する際、`@typescript-eslint/no-floating-promises`ルールに抵触してしまうのを回避するのにvoid文を記述する必要があるため、このルールを無効にしている。ESLint バージョン7.0 以降はルール全体を無効にするのではな`allowAsStatement` オプションで文としての使用のみを許可すること
- `padding-line-between-statements`…… 任意の構文の間に区切りの空行を入れるかどうかを定義するルール。ここではreturn 文の前に常に空行を入れるよう設定している
- `@typescript-eslint/no-unused-vars`…… 使用していない変数の定義を許さないルール。ここでは変数名を_ にしたときのみ許容するように設定
- `import/extensions`* …… インポートの際のファイル拡張子を記述するか定義するルール。npm パッケージ以外のファイルについて.js、.jsx、.ts、.tsx のファイルのみ拡張子を省略し、他のファイルは拡張子を記述させるように
- `react/jsx-filename-extension`* …… JSX のファイル拡張子を制限するルール。`eslint-config-airbnb`で.jsx のみに限定されているので、`.tsx` を追加
- `react/jsx-props-no-spreading`…… JSX でコンポーネントを呼ぶときのprops の記述にスプレッド構文を許さないルール。`eslint-config-airbnb` にてすべて禁止されているが、`<Foo {...{ bar, baz } /}>` のように個々のprops を明記する書き方のみ許容するように設定
- `react/prop-types`* …… コンポーネントのprops に型チェックを行うためのpropTypes プロパティ58 の定義を強制するルール。eslint-config-airbnb で設定されているが、TypeScript の場合は不要なのでファイル拡張子が.tsx の場合に無効化するよう設定を上書き
- `overrides`は任意のglob パターン59 にマッチするファイルのみ、ルールの適用を上書きできるプロパティ

- *はTypeScriptを使った開発で必須の設定
- ESLintの組み込みルールについては公式ドキュメントにルール一覧があるのでそれを参照する
- `@typescript-eslint/eslint-plugin`はGitHubプロジェクトページのREADME に同様のフォーマットで一覧がある
- `.eslintignore`ファイルを作ってESLint チェックの対象外となるファイルを定義
- VSCodeにESLintの拡張機能いれる
    - VSCodeの`settings.json`に追記して自動で動くように設定
- ESLintを一時的に黙らせたいときは無効化コメントを使う
    - `eslint-disable`
    - `eslint-disable-next-line`
### 6-2. Prettier 
#### 特別なコードフォーマッタ Prettier 
- 『コードフォーマッタ』とは、その名のとおりインデントや改行などの記述スタイルのフォーマットを統一して整形してくれるツール

#### ESLint のプラグインとして Prettierをインストール
- PrettierはESLintとバッティングすることがあるのでESLintの`--fix`オプションとして提供されているものを使う
- `eslint-config-prettier`系の共有設定を書くのは一番最後にする

### 6-3. stylelint 
- stylelintはESLintのCSS版
### 6-4. さらに進んだ設定 
- 不要な拡張ルールセットやプラグインは導入せず、カスタマイズは最小限にとどめて、常に
自分が中身を把握しておけるよう設定はできるだけシンプルにする
- Huskeyとlint-stagedでGitにコミットした際にLintが走る設定をする

## 第 7章 React をめぐるフロントエンドの歴史
### 7-1. React 登場前夜 
#### すべては Google マップショックから始まった 
#### フロントエンド第2世代技術の興隆
### 7-2. Web Componentsが夢見たもの
### 7-3. React の誕生 
### 7-4. React を読み解く6つのキーワード 
#### 公式サイトトップに掲げられている三大コンセプト 
#### Declarative（宣言的） 
- 『宣言型プログラミング（Declarative Programming）』という用語があって、出力の性質やあるべき状態を記述してプログラムを構成することと定義されてる
- 対義語は『命令型プログラミング（Imperative Programming）』で、最終的な出力を得るために、時系列に沿って直前の状態に依存しながら命令を順番に書いていく手法

#### Component-Based（コンポーネントベース）、Just The UI（UIにしか関知しない）
- コンポーネントベースのアーキテクチャというのは、見た目と機能がカプセル化されたコンポーネントというアプリケーションの部品を作成し、それらを組み合わせることで複雑なUI を構築するという設計思想のこと 

#### Virtual DOM（仮想 DOM） 
- 仮想DOM というのは、メモリに展開されたこのイミュータブルな要素ツリーのこと

#### One-Way Dataflow（単方向データフロー） 
- React における単方向データフローとは、データがコンポーネントツリーを親コ
ンポーネントから子コンポーネントに対して一方向に、props という形をとって流れ落ちることをいう
#### Learn Once, Write Anywhere（ひとたび習得すれば、あらゆるプラットフォームで開発できる
### 7-5. 他のフレームワークとの比較 
## 第3世代のフレームワーク情勢 
#### Angular 
#### Vue.js
#### LitElement、そしてWeb Components

## 第 8章 何はなくともコンポーネント 
### 8-1. コンポーネントのメンタルモデル 
- 仮想DOMを構成するのがReact Elements
- コンポーネントは関数。propsを引数として受け取り戻り地としてreact Elementsを返す
- 関数との違いはコンポーネントは個々が状態を持つことができること(state)
- React の差分検出処理エンジンは、仮想DOM 内のReact Elements すべてを監視していて、そのどれかのpropsまたはその保持しているstate の値に差分を検出すると、そのコンポーネントのレンダリング処理を再実行する

### 8-2. コンポーネントとProps
- アプリケーションの純粋関数で宣言的に記述するためにstateは極力使わない
- コールされるコンポーネント側では{ 属性名: 属性値} の形式のpropsというオブジェクトとして受け取る
- 受け取り方は2つ
    - コンポーネントの実装が関数だった場合はその関数の第1引数として渡される
    - クラスだった場合は初期化時にそのメンバーオブジェクトpropsとして設定される
- 関数コンポーネントの記述推奨
- Reactの関数コンポーネントの型インターフェース`FunctionComponent` はエイリアスの`FC`でエイリアス
- FCに型引数`FC<Props>`を渡すことで、そのコンポーネントのpropsの型を指定できる
- 関数コンポーネントでは、レンダリングしたい内容を戻り値としてreturn で返す
- フラグメント`<></>`は必ず中身のノードが必要
- オプショナルなプロパティで値が省略されてるケースに適用するのであれば、`nullish coalescing`を使うほうが適切

### 8-3. クラスコンポーネントで学ぶ State 
- デメリット
    - `this`の挙動が不可解
    -  minifyやホットリローティング、AOTコンパイルなどの最適化が困難
    - ライフサイクルメソッドの可読性の低さ
    - ステートの分離が難しく再利用性が低い

#### コンポーネントをクラスで表現する 
- クラスコンポーネントでのprops の型定義は`Component<Props>` のように型引数で行い、
渡されたpropsへのアクセスはメンバー変数`props`から行う

#### クラスコンポーネントに State を持たせる 
- `{} `の型はTypeScriptの解釈では『null 以外のあらゆるオブジェクト』になってしまうため、[@typescript-eslint/ban-types]176 のルールで使用が禁じられている
- stateの初期化にはコンストラクタが必要
- stateに直接代入していいのはコンストラクタの中だけで、それ以外では値の設定には必ずsetState メソッドを使う
- `setState`メソッドの2種類の引数
    - state 内の変更したい要素・ 名をキーに、値をその値にしたオブジェクト
    e.g. `{ count: 0 }`
    - `(prevState, props?) => newState` 形式の、以前の stateとpropsを引数として受け取って新しいstate を返す関数
    e.g. `(state, props) => ({ foo: state.foo + props.bar })`
- Reactではstateの更新はレンダリング最適化処理の中で非同期に行われるため、stateの前の状態に依存する変更処理は関数で設定したほうが良い

``` JavaScript
reset = (e: SyntheticEvent) => {
e.preventDefault();
this.setState({ count: 0 });
};
increment = (e: SyntheticEvent) => {
e.preventDefault();
this.setState((state) => ({ count: state.count + 1 }));
};
```
- イベントハンドラを使ってメソッド内部で操作をしたい場合、Reactが用意している`e: SyntheticEvent` という型で定義されるイベントオブジェクトを引数にとることができる
- `e.preventDefault()`は該当要素のオリジナルな`onClick`の挙動なんかを抑制するための記述

### 8-4. コンポーネントのライフサイクル 
- コンポーネントにおけるライフサイクルとは、まずマウント初期化され、次にレンダリングされた後、
何らかのきっかけで再レンダリングされ、最後にアンマウントされるまでの過程をいう
- クラスコンポーネントにはライフサイクルの各フェーズに対応した『ライフサイクルメソッド
（lifecycle methods）』というものがあり、そこに必要な処理を登録しておける
1. Mounting フェーズ…… コンポーネントが初期化され、仮想DOM にマウントされるまでのフ
ェーズ。このフェーズで初めてコンポーネントがレンダリングされる
2. Updating フェーズ…… 差分検出処理エンジンが変更を検知してコンポーネントが再レンダ
リングされるフェーズ
3. Unmounting フェーズ…… コンポーネントが仮想DOM から削除されるフェーズ
4. Error Handling フェーズ…… 子孫コンポーネントのエラーを検知、捕捉するフェーズ

- ライフサイクルメソッドの中でも下記は大切
    - `componentDidMount`
    - `shouldComponentUpdate`
    - `componentDidUpdate`
    - `componentWillUnmount`
- コンポーネントのライフサイクルはバージョン16.3 から17 にかけて大きな変更があり、最近非推奨になったメソッドも多いので注意が必要
    - `componentWillMount`, 
    - `componentWillReceiveProps`, 
    - `componentWillUpdate`
- `setInterval()` というのはJavaScript の組み込み関数で、第1 引数の関数を第2 引数のミリ秒ごと延々と実行し続けるようにする

### 8-5. Presentational Component とContainer Component 
- コンポーネントを『Presentational Component』と『Container Component』の2 つに分割しようというデザインパターンが存在
    - Presentational Componentは見た目だけ
        - 内部にDOMマークアップを持つ
        - データをpropsとして一方的に受け取る
        - Fluxのstoreなどに依存しない
        - ステートレス
        - データ変更に介入しない
        - 関数コンポーネントで表現されることが多い
    - Container Componentはロジックを追加するコンポーネント
        - DOMマークアップは最大限持たない
        - データや挙動を他のコンポーネントに受け渡す
        - Fluxのactionwを実行したりFluxのstoreに依存する
        - ステートを持つ
        - データの変更に介入して任意の処理を行う
        - HOC、Render Props、Hooksが使われることがおおい
- ロジックから独立して純粋に見た目だけを責務としたコンポーネントを分離しておけば、それらをスタイルガイドに登録してデザインの運用に活用できる。そうすることですでにどんな見た目のコンポーネントがあるか調べやすいし、再利用性も高まる
- 公式が推奨する正しいコンポーネントの作り方
    - デザインモックから始め、そのUIをコンポーネントの構造に分解して落とし込む
    - ロジックを除外した、静的に動作するバージョンを作成する
    - UI を表現するために最低限必要な「状態」を特定する
    - 3の「状態」をどこに配置すべきかを決める
    - 階層構造を逆のぼって考え、データが上階層から流れてくるようにする

## 第 9章 Hooks、関数コンポーネントの合体強化パーツ 
### 9-1. Hooksに至るまでの物語 
- HOCとはコンポーネントを引数に取り、戻り値としてコンポーネントを返す関数

#### 手軽ながら壊れやすかった Mixins 
#### コミュニティによって普及したHOC
- HOC をTypeScript で使う場合、この内側のコンポーネントのprops とHOC 適用後の外側のコンポーネントのprops をうまく辻褄が合うように型合成してあげる必要がある
- 内側のコンポーネントはそれ単体でも成立するよう、HOC から注入される予定のprops にはデフォルト値を設定しないといけない



#### HOC の対抗馬Render Props 
#### ついにHooks が登場する 
### 9-2. HooksでStateを扱う 
### 9-3. Hooksで副作用を扱う 
#### Effect Hookの使い方
#### Effect Hookとライフサイクルメソッドの相違点 
### 9-4. Hooksにおけるメモ化を理解する 
### 9-5. Custom Hook でロジックを分離・再利用する 