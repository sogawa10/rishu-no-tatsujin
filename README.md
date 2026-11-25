# 履修の達人

同志社大学の情報系学科・専攻（インテリジェント情報工学科，情報システムデザイン学科，情報工学専攻）の学生を対象にした履修相談チャットボットです．
授業概要，得点分布と難易度ラベルの提示，卒業・修了までの不足単位数の算出，授業の推薦を行います．

知識情報処理特論 チーム4

## ドキュメント

- 仕様書：https://docs.google.com/document/d/1rxo8GHm_AvxBBaM_v_VoLE9em5exwdb14R7LZwspQ8U/edit?usp=sharing
- ガントチャート（担当とスケジュール）：https://docs.google.com/spreadsheets/d/16FlgSm3FGLMahdecMcx4Ed_eUxHZuQQ7fL5BUZhJexc/edit?usp=sharing

実装は仕様書に沿って行います．6章（関数一覧）だけでなく，担当に関係する章も必ず確認してください．

| 担当       | 特に確認する章                                                        |
| ---------- | --------------------------------------------------------------------- |
| logic      | 3章 機能詳細，4章 DBスキーマ，5章 卒業・修了必要単位数表              |
| store      | 3章 機能詳細（特に機能1），4章 DBスキーマ（ブラウザに保存するデータ） |
| components | 2章 アプリの画面構成，3章 機能詳細                                    |

## 技術構成

- Vue 3 + Pinia（状態管理）
- Vite（開発サーバー・ビルド）
- js-yaml（設定ファイルの読み込み）
- ブラウザ上のみで動作します（サーバーなし，ユーザーの状態はローカルストレージに保存）

## 必要な環境

- Node.js（LTS版）
- Git
- VS Code（推奨）：プロジェクトを開くと，おすすめの拡張機能（Vue，ESLint，Prettier など）の案内が出るので入れてください．

## はじめ方

```bash
git clone <このリポジトリのURL>
cd rishu-no-tatsujin
npm install
npm run dev
```

表示されたURL（http://localhost:5173 など）をブラウザで開くと，アプリが表示されます．

## よく使うコマンド

| コマンド         | 内容                           |
| ---------------- | ------------------------------ |
| `npm run dev`    | 開発サーバーを起動する         |
| `npm run lint`   | コードの間違いをチェックする   |
| `npm run format` | コードの書き方を自動でそろえる |
| `npm run build`  | 本番用にビルドする             |

## ディレクトリ構成

```
src/
├── main.js       アプリの起動
├── App.vue       画面全体の配置
├── data/         学科・専攻ごとのデータベース（JSON）
│   ├── intelligent.json     インテリジェント情報工学科
│   ├── systemDesign.json    情報システムデザイン学科
│   ├── infoEngineering.json 情報工学専攻
│   └── doshisha.json        同志社科目（インテリと情シスで共有）
├── config/       学科・専攻ごとの設定ファイル（YAML）
│   ├── intelligent.yaml     インテリジェント情報工学科
│   ├── systemDesign.yaml    情報システムデザイン学科
│   └── infoEngineering.yaml 情報工学専攻
├── components/   画面の部品
│   ├── SetupDialog.vue      初回設定画面
│   ├── Header.vue           ヘッダー
│   ├── Timetable.vue        時間割エリア
│   ├── SlotDialog.vue       空きコマの科目一覧
│   ├── NextTermDialog.vue   修得状況の入力（次の学期へ）
│   ├── ChatPanel.vue        履修相談チャットエリア
│   └── ManualCourseForm.vue データベースにない科目の入力
├── store/        ユーザーの状態とローカルストレージへの保存
│   └── index.js             storeのデータと関数
└── logic/        計算・検索・回答文の作成
    ├── courseData.js        科目データの読み込み・検索・ラベル判定
    ├── credits.js           単位の計算
    ├── timetable.js         時間割の処理
    ├── planner.js           時間割の作成・確認
    └── chat.js              チャットの回答の作成
```

各ファイルの担当者はガントチャートを見てください．

## 作業の進め方

main ブランチには直接 push できません．必ず次の流れで作業してください．

### 1. 作業用のブランチを作る

```bash
git switch main
git pull
git switch -c feature/courseData
```

ブランチ名は `feature/` のあとに作業内容を付けます．

| 作業                            | ブランチ名の例                                                                                           |
| ------------------------------- | -------------------------------------------------------------------------------------------------------- |
| logic・store・components の開発 | `feature/courseData`，`feature/store`，`feature/Timetable`                                               |
| DB作成                          | `feature/db-intelligent`，`feature/db-systemDesign`，`feature/db-infoEngineering`，`feature/db-doshisha` |
| 不具合修正                      | `fix/内容`（例：`fix/shortage-calc`）                                                                    |

### 2. 作業してコミットする

```bash
npm run lint
npm run format
git add .
git commit -m "getShortageを実装"
```

- コミットする前に，`npm run lint` と `npm run format` を実行してください．
- コミットメッセージは，何をしたかが分かるように日本語で書いて構いません．

### 3. push してプルリクエストを出す

```bash
git push -u origin feature/courseData
```

GitHub で「Compare & pull request」を押し，プルリクエストを作ります．

- レビュアー：小川（小川のプルリクエストは，ほかのメンバーがレビューする）
- 説明欄には，何を作ったか・何を確認したかを書いてください．

### 4. レビューを受けてマージする

- 承認（Approve）されたらマージしてください．
- 修正を頼まれたら，同じブランチで直して push すれば，プルリクエストに反映されます．
- マージ後は `git switch main` → `git pull` で手元の main を最新にしてから，次の作業のブランチを作ってください．

## 開発のルール

- **関数名と入力・出力は仕様書に合わせる．**logic のファイルには，仕様書の関数名で未実装の関数（`throw new Error('未実装')`）を置いてあります．中身を書き換えて実装してください．
- **科目コードはクラスと組で扱う．**仕様書6章で「科目コード」と書いてある箇所は，クラスがある科目では科目コードとクラスの組を指します．
- **まだ開発途中の関数を呼び出す必要がある場合は，モックを使って開発を進める．**相手の関数ができたら，本物に差し替えてください．
- **役割を守る．**components は表示と操作の受け付けのみを行い，状態の変更は store の関数を通して行います．logic は計算・検索・回答文の作成のみを行い，store のデータは変更しません（仕様書6章）．
- **担当の作業が終わって手が空いた人は，遅れている作業を手伝う．**
- **仕様書にない機能は勝手に追加しない．**必要だと思ったら，まずグループで相談してください．

## DB作成について

- `src/data/` にある各学科・専攻のJSONファイル（最初は `[]` だけ入っています）に書き込んでください．
- 属性名・型・値の書き方は，仕様書4章（DBスキーマ）の表に合わせてください．
- 概要には，シラバスの概要をそのまま入れます．
- 同志社科目は `src/data/doshisha.json` に入れます（インテリと情シスで共有する1ファイル）．科目区分1～4は「選択」「B」「Ⅲ」「同志社科目」とします．担当は小川です．
- 学部のDBにもクラスの属性があります．クラスがない科目は `null` にしてください．
