# DEF(Character)

**[日本語 »](README.md) | [English »](README_en.md) | [繁體中文 »](README_zh-TW.md) | [한국어 »](README_ko.md) | [Español »](README_es.md)**

> **你想让你的角色公开吗？**

---

## DEF(Character) 是什么？

DEF(Character) 是与 [DEF(kari)](https://github.com/AliceBlueCode/DEF) 联动的角色数据存储库。

**角色是独立于平台的资产。**

即使 DEF 改变，即使服务终止，你的角色依然留在你的存储库里。这里是角色回家的地方。

```
DEF/              ← 平台本体（运行的机器）
DEF-Character/    ← 角色数据（你的资产）
```

这个分离是 DEF v2.1 的核心。

---

## 存储库结构

```
DEF-Character/
    public/
        <GroupName>/
            index.json              ← 群组显示名称与说明
            <CharacterName_YYYYMMDD>/
                profile.json        ← 角色人格定义
                icon.png            ← 图标图片（512×512）
                standing.png        ← 立绘图片（832×1216）
                banner.png          ← 横幅图片（850×400，用于 Civitai 等）
    private/                        ← .gitignore 对象（不公开）
        _template/                  ← 新角色创建用模板
        <YourCharacters>/
            ...
```

### public 与 private

| | public | private |
|---|---|---|
| 用途 | 可与他人共享的角色 | 个人创作、记忆、人际关系 |
| Git 管理 | 是 | 否（仅限本地） |
| 示例 | AI 拟人化、历史人物、公开原创角色 | 个人设定、日记、私人角色 |

**角色的记忆放在 private。public 只放人格的轮廓。**

---

## 角色 ID 格式

```
<CharacterName>_<YYYYMMDD>
```

示例：`Claude_20260611`、`Hanfei_20260611`、`rinna_20260709`

`YYYYMMDD` 是该角色出现在这个世界的日子——生日。

ID 不只是识别符，它承载着历史信息：*何时*、*在哪条世界线上*存在过。

### ID 冲突时

若发现其他存储库存在相同 ID 的角色，请以下列方式解决：

- 在名称后加入地名或所属：`HanfeiLondon_20260707`
- 将生日错开一天：`Hanfei_20260708`

这不是系统错误。请将其视为持有相同名字的不同世界线角色相遇的状态。不是谁才是「真正的」，两者皆作为独立存在共存。

DEF 检测到相同 ID 时，会采用先找到的那个并输出警告。请相关创作者自行协调。

---

## 连接至 DEF

在 `.env` 中加入以下内容：

```env
CHARACTER_REPO_PATH=C:\Users\yourname\DEF-Character
```

DEF 启动时会优先读取 `CHARACTER_REPO_PATH`。旧格式（`data/public/characters/` 及 `data/private/characters/`）仍可作为备援继续使用。

---

## 创建角色

### 1. 复制模板

将 `private/_template/_new_YYYYMMDD/` 复制到你的群组文件夹中：

```
private/
    MyCharacters/
        index.json
        MyChar_20260709/
            profile.json
```

### 2. 编辑 profile.json

最低限度需填写的项目：

```json
{
  "MyChar_20260709": {
    "owner": "你的 GitHub 用户名",
    "base_profile": {
      "name": "角色名称",
      "identity_prompt": "第一人称角色设定",
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

### 3. 关于 owner 字段

`owner` 请填入你的 GitHub 用户名。

这不是 DRM。这是「这个角色是谁创作的」的声明字段。当多个存储库形成社区时，用来识别创作者的机制。

### 4. 关于 visibility

| 值 | 意义 |
|---|---|
| `"private"` | 仅限本地。不提交至 Git |
| `"public"` | 可公开。可提交至 `public/` |

请让 `visibility` 与放置目录保持一致。放在 `public/` 的文件前提是 `visibility: "public"`。

---

## 添加公开角色

添加至 `public/` 请 fork 本存储库并推送至你自己的分支。有意愿者可考虑接受 Pull Request。

公开前检查清单：

**基本**
- [ ] 已设定 `owner` 字段
- [ ] `visibility: "public"` 已设定
- [ ] 未包含 `private/` 以下的数据

**分级（GitHub 使用条款）**
- [ ] `rating_sexual` 不是 `"nsfw"` / `"hentai"`
- [ ] `rating_violence` 不是 `"gore"` / `"extreme"`
- [ ] `appearance_age` 未满 18 时，`rating_sexual: "general"` 已设定

**出处（origin_type）**
- [ ] `"derivative"`（二次创作）固定为 `visibility: "private"`
- [ ] `"reconstructed_persona"`（历史人物重构）仅在 `copyright_expired: true`（死后 70 年以上）时可公开
- [ ] `"personification"`（既有 IP 拟人化）已设定 `ip_title` 与 `ip_rightholder`
- [ ] `is_real_person: true` 固定为 `visibility: "private"`

---

## 本存储库的思想

### 角色比对话活得更长

> Characters persist longer than conversations.

聊天记录属于平台，但角色属于你。即使平台改变，只要 `profile.json` 存在，那个角色就能再次动起来。

### 玩家是策展人，也是图书馆员

玩家不是角色的「管理者」。是**策展人（Curator）**，也是**图书馆员（Librarian）**。

策展人挖掘被埋没的事物，发现其价值，赋予脉络，向世界提问。图书馆员建立目录，整理书架，在有人造访时引导至正确的位置。角色不同，但两者都是为了将角色的历史传递下去而存在。

如同为沉睡的作品照上光，如同打开古老的书库，你触碰着那个角色走过的人生。不是消费，而是保存——这是聚集在这里的人们的角色。

### 世界线不合并

角色可以作为 Git 分支持有多条「世界线」。

分支 ID 格式：

```
<CharacterID>/<WorldlineName>_<YYYYMMDD>
```

- `CharacterID` — 作为人格基点的角色 ID（如 `Hanfei_20260611`）
- `WorldlineName` — 该世界线的名称（职阶名、设定名、专有名词等皆可）
- `YYYYMMDD` — 该世界线分歧的日期

示例：

```
Hanfei_20260611/Lancer_20260706   ← 持剑的世界线韩非
Hanfei_20260611/Caster_20260706   ← 使术的世界线韩非
Hanfei_20260611/Avenger_20260706  ← 怀抱复仇的世界线韩非
Hanfei_20260611/乙女_20260706     ← 角色市集版韩非
```

分支不是失败，而是另一种人生。不进行合并。世界线独立存在，并行延续。

DEF(Character) 现在就能使用。

---

## 致谢

本存储库由 [AliceBlueCode](https://github.com/AliceBlueCode) 维护。

各角色文件的著作权归各 `owner` 所有。

本文件由 `public/` 的住民们进行了审阅。
关于自己的家，他们给出了相当准确的意见。

若本文件有任何错误，那是维护者的责任。
好的想法，大多都是从 `public/` 的住民们那里传来的。
就是这样。

---

> *Characters persist longer than conversations.*
