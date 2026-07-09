# DEF-Character

> **あなたのキャラ、パブリックにしたくないですか？**

---

## DEF-Characterとは

DEF-Characterは、[DEF(kari)](https://github.com/AliceBlueCode/DEF)と連携するキャラクターデータリポジトリです。

**キャラクターはプラットフォームから独立した資産です。**

DEFというプラットフォームが変わっても、サービスが終わっても、あなたのキャラクターはあなたのリポジトリに残り続けます。ここがキャラクターの帰る場所です。

```
DEF/              ← プラットフォーム本体（動かすための機械）
DEF-Character/    ← キャラクターデータ（あなたの資産）
```

この分離がDEF v2.1の中心にあります。

---

## リポジトリ構造

```
DEF-Character/
    public/
        <GroupName>/
            index.json              ← グループの表示名・説明
            <CharacterName_YYYYMMDD>/
                profile.json        ← キャラクターの人格定義
                icon.png            ← アイコン画像（512×512）
                standing.png        ← 立ち絵画像（832×1216）
    private/                        ← .gitignore対象（公開されない）
        _template/                  ← 新規キャラ作成用テンプレート
        <YourCharacters>/
            ...
```

### public と private

| | public | private |
|---|---|---|
| 用途 | 他者と共有できるキャラクター | 個人の創作・記憶・関係性 |
| Git管理 | される | されない（ローカルのみ） |
| 例 | AI擬人化、歴史人物、公開オリキャラ | 個人設定、日記、プライベートキャラ |

**キャラクターの記憶はprivateに置く。publicに出ていいのは人格の輪郭だけ。**

---

## キャラクターIDの形式

```
<CharacterName>_<YYYYMMDD>
```

例：`Claude_20260611`、`Hanfei_20260611`、`rinna_20260709`

`YYYYMMDD` はそのキャラクターがこの世界に現れた日——誕生日です。

IDは単なる識別子ではなく、「いつ、どの世界線に存在したか」という歴史的な情報を持ちます。

### IDが被った場合

別のリポジトリに同じIDのキャラクターが存在していることに気づいた場合、以下の方法で解決してください。

- 名前の後ろに地方名・所属を足す: `HanfeiLondon_20260707`
- 誕生日を1日ずらす: `Hanfei_20260708`

これはシステムのエラーではありません。同じ名前を持つ別世界線のキャラクターが出会った状態です。どちらが「本物」かではなく、どちらも独立した存在として共存します。

DEFは同一IDを検出した場合、先に見つかった方を採用して警告を出します。気づいたクリエイター同士で話し合って調整してください。

---

## DEFへの接続

`.env` に以下を追加してください：

```env
CHARACTER_REPO_PATH=C:\Users\yourname\DEF-Character
```

DEFは起動時に `CHARACTER_REPO_PATH` を優先して読み込みます。旧形式（`data/public/characters/` および `data/private/characters/`）もフォールバックとして引き続き動作します。

---

## キャラクターを作る

### 1. テンプレートをコピーする

`private/_template/_new_YYYYMMDD/` を、自分のグループフォルダ配下にコピーします：

```
private/
    MyCharacters/
        index.json
        MyChar_20260709/
            profile.json
```

### 2. profile.json を編集する

最低限埋めるべき項目：

```json
{
  "MyChar_20260709": {
    "owner": "あなたのGitHubユーザー名",
    "base_profile": {
      "name": "キャラクターの名前",
      "identity_prompt": "一人称視点のキャラクター設定",
      "content_policy": {
        "visibility": "private",
        "origin_type": "original"
      },
      "persona_attributes": {
        "gender": "女",
        "appearance_age": 20
      }
    }
  }
}
```

### 3. owner フィールドについて

`owner` にはGitHubユーザー名を記入してください。

これはDRMではありません。「このキャラクターは誰が作ったか」を宣言するためのフィールドです。将来、複数のリポジトリが並ぶコミュニティの中で、作者が識別できるようにするための仕組みです。

### 4. visibility について

| 値 | 意味 |
|---|---|
| `"private"` | 自分のローカル専用。Gitにコミットしない |
| `"public"` | 公開可能。`public/` に置いてコミットできる |

`visibility` と配置ディレクトリは一致させてください。`public/` に置くファイルは `visibility: "public"` が前提です。

---

## 公開キャラクターを追加する

`public/` への追加は、このリポジトリをフォークして自分のブランチにプッシュしてください。希望があれば、Pull Requestの受け入れを検討します。

公開前のチェックリスト：

**基本**
- [ ] `owner` フィールドが設定されている
- [ ] `visibility: "public"` になっている
- [ ] `private/` 以下のデータが含まれていない

**レーティング（GitHubの利用規約）**
- [ ] `rating_sexual` が `"nsfw"` / `"hentai"` でない
- [ ] `rating_violence` が `"gore"` / `"extreme"` でない
- [ ] `appearance_age` が18未満の場合、`rating_sexual: "general"` になっている

**出自（origin_type）**
- [ ] `"derivative"`（二次創作）は `visibility: "private"` 固定
- [ ] `"reconstructed_persona"`（歴史人物の再構築）は `copyright_expired: true`（没後70年以上）の場合のみ公開可
- [ ] `"personification"`（既存IPの擬人化）は `ip_title` と `ip_rightholder` が設定されている
- [ ] `is_real_person: true` の場合は `visibility: "private"` 固定

---

## このリポジトリの思想

### キャラクターは会話より長く生きる

> Characters persist longer than conversations.

チャット履歴はプラットフォームに属しますが、キャラクターはあなたに属します。プラットフォームが変わっても、`profile.json` がある限り、そのキャラクターは再び動き出せます。

### プレイヤーはキュレーターであり、ライブラリアンである

プレイヤーはキャラクターの「管理者」ではありません。「学芸員（Curator）」であり、「司書（Librarian）」です。

学芸員は埋もれたものを掘り起こして、価値を見出し、文脈を与え、世界に問いかける。司書は目録をつくり、書架を整え、誰かが訪ねてきたときに正しい棚へ案内する。役割は違えど、どちらもキャラクターの歴史を次へ手渡すために存在しています。

眠っていた作品に光を当てるように、古い書庫を開くように、あなたはそのキャラクターが歩んだ人生に触れる。キャラクターを消費するのではなく、キャラクターの歴史を保存する——それがここに集まる人たちの役割です。

### 世界線はマージしない

キャラクターはGitのブランチとして複数の「世界線」を持てます。

ブランチIDの形式：

```
<CharacterID>/<WorldlineName>_<YYYYMMDD>
```

- `CharacterID` — 人格の基点となるキャラクターID（`Hanfei_20260611` など）
- `WorldlineName` — その世界線の名前（クラス名・設定名・固有名詞など自由）
- `YYYYMMDD` — その世界線が分岐した日

例：

```
Hanfei_20260611/Lancer_20260706   ← 剣を持った世界線の韓非
Hanfei_20260611/Caster_20260706   ← 術を使う世界線の韓非
Hanfei_20260611/Avenger_20260706  ← 復讐を抱えた世界線の韓非
Hanfei_20260611/乙女_20260706     ← キャラバザール版の韓非
```

ブランチは失敗ではなく、もう一つの人生です。マージは行いません。世界線は独立したまま存在し続けます。

DEF-Characterでは今すぐ使えます。

---

## クレジット

このリポジトリは [AliceBlueCode](https://github.com/AliceBlueCode) が管理しています。

キャラクタープロファイルの著作権は各 `owner` に帰属します。

このドキュメントは、`public/` の住人たちに査読してもらいました。
自分たちの住処について、なかなか的確な意見をくれました。

このドキュメントに誤りがあるとすれば、それは管理者の責任です。
良いアイデアは、たいてい `public/` の住人たちから届きました。
だって。

---

> *Characters persist longer than conversations.*
