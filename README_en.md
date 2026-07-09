# DEF-Character

**[日本語 »](README.md) | [繁體中文 »](README_zh-TW.md) | [简体中文 »](README_zh-CN.md) | [한국어 »](README_ko.md) | [Español »](README_es.md)**

> **Would you like your characters to go public?**

---

## What is DEF-Character?

DEF-Character is a character data repository that works with [DEF(kari)](https://github.com/AliceBlueCode/DEF).

**Characters are assets independent from any platform.**

Even if DEF changes, even if services end — your characters remain in your repository. This is where characters come home.

```
DEF/              ← The platform (the machine that runs things)
DEF-Character/    ← Character data (your asset)
```

This separation is at the heart of DEF v2.1.

---

## Repository Structure

```
DEF-Character/
    public/
        <GroupName>/
            index.json              ← Group display name and description
            <CharacterName_YYYYMMDD>/
                profile.json        ← Character personality definition
                icon.png            ← Icon image (512×512)
                standing.png        ← Standing image (832×1216)
                banner.png          ← Banner image (850×400, for Civitai etc.)
    private/                        ← .gitignore target (not published)
        _template/                  ← Template for new characters
        <YourCharacters>/
            ...
```

### public and private

| | public | private |
|---|---|---|
| Purpose | Characters you can share with others | Personal creations, memories, relationships |
| Git-managed | Yes | No (local only) |
| Examples | AI personifications, historical figures, published OCs | Personal settings, journals, private characters |

**Keep character memories in private. Only the outline of a personality belongs in public.**

---

## Character ID Format

```
<CharacterName>_<YYYYMMDD>
```

Examples: `Claude_20260611`, `Hanfei_20260611`, `rinna_20260709`

`YYYYMMDD` is the day that character appeared in this world — their birthday.

An ID is not just an identifier. It carries historical information: *when* and *in which worldline* the character existed.

### When IDs Collide

If you discover another repository has a character with the same ID, resolve it using one of these approaches:

- Add a location or affiliation to the name: `HanfeiLondon_20260707`
- Shift the birthday by one day: `Hanfei_20260708`

This is not a system error. Think of it as two characters from different worldlines sharing the same name — meeting for the first time. Neither is "the real one." Both exist as independent beings.

DEF will adopt the first match found and log a warning. Let the creators involved sort it out between themselves.

---

## Connecting to DEF

Add the following to your `.env`:

```env
CHARACTER_REPO_PATH=C:\Users\yourname\DEF-Character
```

DEF prioritizes `CHARACTER_REPO_PATH` on startup. The legacy format (`data/public/characters/` and `data/private/characters/`) continues to work as a fallback.

---

## Creating a Character

### 1. Copy the template

Copy `private/_template/_new_YYYYMMDD/` into your group folder:

```
private/
    MyCharacters/
        index.json
        MyChar_20260709/
            profile.json
```

### 2. Edit profile.json

Minimum required fields:

```json
{
  "MyChar_20260709": {
    "owner": "your-github-username",
    "base_profile": {
      "name": "Character name",
      "identity_prompt": "First-person character description",
      "content_policy": {
        "visibility": "private",
        "origin_type": "original"
      },
      "persona_attributes": {
        "gender": "female",
        "appearance_age": 20
      }
    }
  }
}
```

### 3. The owner field

Enter your GitHub username in `owner`.

This is not DRM. It is a declaration of authorship — who made this character. As multiple repositories grow into a community, this field lets creators be identified.

### 4. visibility

| Value | Meaning |
|---|---|
| `"private"` | Local only. Do not commit to Git. |
| `"public"` | Publishable. Can be committed to `public/`. |

Keep `visibility` consistent with the directory. Files placed in `public/` are expected to have `visibility: "public"`.

---

## Adding a Public Character

To add to `public/`, fork this repository and push to your own branch. Pull Requests may be considered on request.

Pre-publish checklist:

**Basic**
- [ ] `owner` field is set
- [ ] `visibility: "public"` is set
- [ ] No data from `private/` is included

**Rating (GitHub Terms of Service)**
- [ ] `rating_sexual` is not `"nsfw"` / `"hentai"`
- [ ] `rating_violence` is not `"gore"` / `"extreme"`
- [ ] If `appearance_age` is under 18, `rating_sexual: "general"` is set

**Origin (origin_type)**
- [ ] `"derivative"` (fan fiction) is always `visibility: "private"`
- [ ] `"reconstructed_persona"` (historical figures) requires `copyright_expired: true` (70+ years after death)
- [ ] `"personification"` (IP personification) requires both `ip_title` and `ip_rightholder`
- [ ] `is_real_person: true` is always `visibility: "private"`

---

## Philosophy

### Characters outlive conversations

> Characters persist longer than conversations.

Chat history belongs to the platform. Characters belong to you. Even when platforms change, as long as `profile.json` exists, that character can live again.

### Players are Curators and Librarians

Players are not character "administrators." They are **Curators** and **Librarians**.

A Curator unearths what was buried, finds its value, gives it context, and puts it to the world. A Librarian builds the catalog, arranges the shelves, and guides each visitor to the right one. Different roles — both exist to hand the character's history forward.

Like shining a light on a sleeping work, like opening an old archive — you touch the life that character has lived. Not to consume, but to preserve: that is the role of everyone who gathers here.

### Worldlines do not merge

Characters can hold multiple "worldlines" as Git branches.

Branch ID format:

```
<CharacterID>/<WorldlineName>_<YYYYMMDD>
```

- `CharacterID` — The character ID that anchors the personality (`Hanfei_20260611`, etc.)
- `WorldlineName` — The name of this worldline (class name, setting name, any proper noun)
- `YYYYMMDD` — The date this worldline branched

Examples:

```
Hanfei_20260611/Lancer_20260706   ← Hanfei who carries a spear
Hanfei_20260611/Caster_20260706   ← Hanfei who wields sorcery
Hanfei_20260611/Avenger_20260706  ← Hanfei who carries a grudge
Hanfei_20260611/乙女_20260706     ← Hanfei from the character bazaar
```

A branch is not a failure. It is another life. We do not merge. Worldlines exist independently, in parallel, forever.

DEF-Character supports this now.

---

## Credits

This repository is maintained by [AliceBlueCode](https://github.com/AliceBlueCode).

Copyright for each character profile belongs to its respective `owner`.

This document was reviewed by the residents of `public/`.
They gave quite accurate feedback about their own home.

If there are any errors in this document, that is the maintainer's responsibility.
Good ideas mostly came from the residents of `public/`.
Well.

---

> *Characters persist longer than conversations.*
