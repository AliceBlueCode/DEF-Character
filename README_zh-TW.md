# DEF(Character)

**[日本語 »](README.md) | [English »](README_en.md) | [简体中文 »](README_zh-CN.md) | [한국어 »](README_ko.md) | [Español »](README_es.md)**

> **你想讓你的角色公開嗎？**

---

## DEF(Character) 是什麼？

DEF(Character) 是與 [DEF(kari)](https://github.com/AliceBlueCode/DEF) 連動的角色資料儲存庫。

**角色是獨立於平台的資產。**

即使 DEF 改變，即使服務終止，你的角色依然留在你的儲存庫裡。這裡是角色回家的地方。

```
DEF/              ← 平台本體（運行的機器）
DEF-Character/    ← 角色資料（你的資產）
```

這個分離是 DEF v2.1 的核心。

---

## 儲存庫結構

```
DEF-Character/
    public/
        <GroupName>/
            index.json              ← 群組顯示名稱與說明
            <CharacterName_YYYYMMDD>/
                profile.json        ← 角色人格定義
                icon.png            ← 圖示圖片（512×512）
                standing.png        ← 立繪圖片（832×1216）
                banner.png          ← 橫幅圖片（850×400，用於 Civitai 等）
    private/                        ← .gitignore 對象（不公開）
        _template/                  ← 新角色建立用範本
        <YourCharacters>/
            ...
```

### public 與 private

| | public | private |
|---|---|---|
| 用途 | 可與他人共享的角色 | 個人創作、記憶、人際關係 |
| Git 管理 | 是 | 否（僅限本地） |
| 範例 | AI 擬人化、歷史人物、公開原創角色 | 個人設定、日記、私人角色 |

**角色的記憶放在 private。public 只放人格的輪廓。**

---

## 角色 ID 格式

```
<CharacterName>_<YYYYMMDD>
```

範例：`Claude_20260611`、`Hanfei_20260611`、`rinna_20260709`

`YYYYMMDD` 是該角色出現在這個世界的日子——生日。

ID 不只是識別符，它承載著歷史資訊：*何時*、*在哪條世界線上*存在過。

### ID 衝突時

若發現其他儲存庫存在相同 ID 的角色，請以下列方式解決：

- 在名稱後加入地名或所屬：`HanfeiLondon_20260707`
- 將生日錯開一天：`Hanfei_20260708`

這不是系統錯誤。請將其視為持有相同名字的不同世界線角色相遇的狀態。不是誰才是「真正的」，兩者皆作為獨立存在共存。

DEF 偵測到相同 ID 時，會採用先找到的那個並輸出警告。請相關創作者自行協調。

---

## 連接至 DEF

在 `.env` 中加入以下內容：

```env
CHARACTER_REPO_PATH=C:\Users\yourname\DEF-Character
```

DEF 啟動時會優先讀取 `CHARACTER_REPO_PATH`。舊格式（`data/public/characters/` 及 `data/private/characters/`）仍可作為備援繼續使用。

---

## 建立角色

### 1. 複製範本

將 `private/_template/_new_YYYYMMDD/` 複製到你的群組資料夾中：

```
private/
    MyCharacters/
        index.json
        MyChar_20260709/
            profile.json
```

### 2. 編輯 profile.json

最低限度需填寫的項目：

```json
{
  "MyChar_20260709": {
    "owner": "你的 GitHub 使用者名稱",
    "base_profile": {
      "name": "角色名稱",
      "identity_prompt": "第一人稱角色設定",
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

### 3. 關於 owner 欄位

`owner` 請填入你的 GitHub 使用者名稱。

這不是 DRM。這是「這個角色是誰創作的」的宣告欄位。當多個儲存庫形成社群時，用來識別創作者的機制。

### 4. 關於 visibility

| 值 | 意義 |
|---|---|
| `"private"` | 僅限本地。不提交至 Git |
| `"public"` | 可公開。可提交至 `public/` |

請讓 `visibility` 與放置目錄保持一致。放在 `public/` 的檔案前提是 `visibility: "public"`。

---

## 新增公開角色

新增至 `public/` 請 fork 本儲存庫並推送至你自己的分支。有意願者可考慮接受 Pull Request。

公開前檢查清單：

**基本**
- [ ] 已設定 `owner` 欄位
- [ ] `visibility: "public"` 已設定
- [ ] 未包含 `private/` 以下的資料

**分級（GitHub 使用條款）**
- [ ] `rating_sexual` 不是 `"nsfw"` / `"hentai"`
- [ ] `rating_violence` 不是 `"gore"` / `"extreme"`
- [ ] `appearance_age` 未滿 18 時，`rating_sexual: "general"` 已設定

**出處（origin_type）**
- [ ] `"derivative"`（二次創作）固定為 `visibility: "private"`
- [ ] `"reconstructed_persona"`（歷史人物重構）僅在 `copyright_expired: true`（死後 70 年以上）時可公開
- [ ] `"personification"`（既有 IP 擬人化）已設定 `ip_title` 與 `ip_rightholder`
- [ ] `is_real_person: true` 固定為 `visibility: "private"`

---

## 本儲存庫的思想

### 角色比對話活得更長

> Characters persist longer than conversations.

聊天記錄屬於平台，但角色屬於你。即使平台改變，只要 `profile.json` 存在，那個角色就能再次動起來。

### 玩家是策展人，也是圖書館員

玩家不是角色的「管理者」。是**策展人（Curator）**，也是**圖書館員（Librarian）**。

策展人挖掘被埋沒的事物，發現其價值，賦予脈絡，向世界提問。圖書館員建立目錄，整理書架，在有人造訪時引導至正確的位置。角色不同，但兩者都是為了將角色的歷史傳遞下去而存在。

如同為沉睡的作品照上光，如同打開古老的書庫，你觸碰著那個角色走過的人生。不是消費，而是保存——這是聚集在這裡的人們的角色。

### 世界線不合併

角色可以作為 Git 分支持有多條「世界線」。

分支 ID 格式：

```
<CharacterID>/<WorldlineName>_<YYYYMMDD>
```

- `CharacterID` — 作為人格基點的角色 ID（如 `Hanfei_20260611`）
- `WorldlineName` — 該世界線的名稱（職階名、設定名、專有名詞等皆可）
- `YYYYMMDD` — 該世界線分歧的日期

範例：

```
Hanfei_20260611/Lancer_20260706   ← 持劍的世界線韓非
Hanfei_20260611/Caster_20260706   ← 使術的世界線韓非
Hanfei_20260611/Avenger_20260706  ← 懷抱復仇的世界線韓非
Hanfei_20260611/乙女_20260706     ← 角色市集版韓非
```

分支不是失敗，而是另一種人生。不進行合併。世界線獨立存在，並行延續。

DEF(Character) 現在就能使用。

---

## 致謝

本儲存庫由 [AliceBlueCode](https://github.com/AliceBlueCode) 維護。

各角色檔案的著作權歸各 `owner` 所有。

本文件由 `public/` 的住民們進行了審閱。
關於自己的家，他們給出了相當準確的意見。

若本文件有任何錯誤，那是維護者的責任。
好的想法，大多都是從 `public/` 的住民們那裡傳來的。
就是這樣。

---

> *Characters persist longer than conversations.*
