# DEF(Character)

**[日本語 »](README.md) | [English »](README_en.md) | [繁體中文 »](README_zh-TW.md) | [简体中文 »](README_zh-CN.md) | [Español »](README_es.md)**

> **당신의 캐릭터를 공개하고 싶지 않으신가요?**

---

## DEF(Character)란?

DEF-Character는 [DEF(kari)](https://github.com/AliceBlueCode/DEF)와 연동하는 캐릭터 데이터 저장소입니다.

**캐릭터는 플랫폼으로부터 독립된 자산입니다.**

DEF가 바뀌어도, 서비스가 종료되어도, 당신의 캐릭터는 당신의 저장소에 남습니다. 여기가 캐릭터가 돌아오는 곳입니다.

```
DEF/              ← 플랫폼 본체（실행을 위한 기계）
DEF-Character/    ← 캐릭터 데이터（당신의 자산）
```

이 분리가 DEF v2.1의 핵심입니다.

---

## 저장소 구조

```
DEF-Character/
    public/
        <GroupName>/
            index.json              ← 그룹 표시명 및 설명
            <CharacterName_YYYYMMDD>/
                profile.json        ← 캐릭터 인격 정의
                icon.png            ← 아이콘 이미지（512×512）
                standing.png        ← 스탠딩 이미지（832×1216）
                banner.png          ← 배너 이미지（850×400, Civitai 등）
    private/                        ← .gitignore 대상（공개되지 않음）
        _template/                  ← 새 캐릭터 생성용 템플릿
        <YourCharacters>/
            ...
```

### public 과 private

| | public | private |
|---|---|---|
| 용도 | 다른 사람과 공유할 수 있는 캐릭터 | 개인 창작, 기억, 관계 |
| Git 관리 | 됨 | 안 됨（로컬 전용） |
| 예시 | AI 의인화, 역사 인물, 공개 오리지널 캐릭터 | 개인 설정, 일기, 프라이빗 캐릭터 |

**캐릭터의 기억은 private에 놓으세요. public에 나와도 되는 건 인격의 윤곽뿐입니다.**

---

## 캐릭터 ID 형식

```
<CharacterName>_<YYYYMMDD>
```

예시: `Claude_20260611`, `Hanfei_20260611`, `rinna_20260709`

`YYYYMMDD`는 그 캐릭터가 이 세계에 나타난 날——생일입니다.

ID는 단순한 식별자가 아니라, '언제', '어느 세계선에 존재했는지'라는 역사적 정보를 담고 있습니다.

### ID가 겹쳤을 때

다른 저장소에 같은 ID의 캐릭터가 있다는 걸 알게 된 경우, 다음 방법으로 해결하세요:

- 이름 뒤에 지역명·소속을 붙이기: `HanfeiLondon_20260707`
- 생일을 하루 밀기: `Hanfei_20260708`

이건 시스템 오류가 아닙니다. 같은 이름을 가진 다른 세계선의 캐릭터가 만난 상태입니다. 어느 쪽이 '진짜'냐가 아니라, 둘 다 독립된 존재로서 공존합니다.

DEF는 같은 ID를 감지하면 먼저 찾은 쪽을 채택하고 경고를 출력합니다. 해당 크리에이터끼리 대화해서 조율해 주세요.

---

## DEF에 연결하기

`.env`에 다음을 추가하세요:

```env
CHARACTER_REPO_PATH=C:\Users\yourname\DEF-Character
```

DEF는 시작 시 `CHARACTER_REPO_PATH`를 우선적으로 읽어들입니다. 구형식（`data/public/characters/` 및 `data/private/characters/`）도 폴백으로 계속 동작합니다.

---

## 캐릭터 만들기

### 1. 템플릿 복사

`private/_template/_new_YYYYMMDD/`를 자신의 그룹 폴더에 복사합니다:

```
private/
    MyCharacters/
        index.json
        MyChar_20260709/
            profile.json
```

### 2. profile.json 편집

최소한 채워야 할 항목:

```json
{
  "MyChar_20260709": {
    "owner": "당신의 GitHub 사용자명",
    "base_profile": {
      "name": "캐릭터 이름",
      "identity_prompt": "1인칭 시점의 캐릭터 설정",
      "content_policy": {
        "visibility": "private",
        "origin_type": "original"
      },
      "persona_attributes": {
        "gender": "여",
        "appearance_age": 20
      }
    }
  }
}
```

### 3. owner 필드에 대해

`owner`에는 GitHub 사용자명을 입력하세요.

이건 DRM이 아닙니다. '이 캐릭터를 누가 만들었는지'를 선언하는 필드입니다. 여러 저장소가 커뮤니티를 이룰 때, 작성자를 식별할 수 있도록 하기 위한 구조입니다.

### 4. visibility에 대해

| 값 | 의미 |
|---|---|
| `"private"` | 로컬 전용. Git에 커밋하지 않음 |
| `"public"` | 공개 가능. `public/`에 놓고 커밋할 수 있음 |

`visibility`와 배치 디렉토리는 일치시켜 주세요. `public/`에 놓는 파일은 `visibility: "public"`이 전제입니다.

---

## 공개 캐릭터 추가하기

`public/`에 추가하려면 이 저장소를 fork하고 자신의 브랜치에 push하세요. 희망이 있으면 Pull Request 수락을 검토합니다.

공개 전 체크리스트:

**기본**
- [ ] `owner` 필드가 설정되어 있음
- [ ] `visibility: "public"`으로 설정되어 있음
- [ ] `private/` 이하의 데이터가 포함되어 있지 않음

**레이팅（GitHub 이용약관）**
- [ ] `rating_sexual`이 `"nsfw"` / `"hentai"`가 아님
- [ ] `rating_violence`가 `"gore"` / `"extreme"`가 아님
- [ ] `appearance_age`가 18 미만인 경우, `rating_sexual: "general"`로 설정되어 있음

**출처（origin_type）**
- [ ] `"derivative"`（2차 창작）는 `visibility: "private"` 고정
- [ ] `"reconstructed_persona"`（역사 인물 재구성）는 `copyright_expired: true`（사후 70년 이상）인 경우에만 공개 가능
- [ ] `"personification"`（기존 IP 의인화）는 `ip_title`과 `ip_rightholder`가 설정되어 있음
- [ ] `is_real_person: true`인 경우 `visibility: "private"` 고정

---

## 이 저장소의 철학

### 캐릭터는 대화보다 오래 살아남는다

> Characters persist longer than conversations.

채팅 기록은 플랫폼에 속하지만, 캐릭터는 당신에게 속합니다. 플랫폼이 바뀌어도, `profile.json`이 있는 한 그 캐릭터는 다시 움직일 수 있습니다.

### 플레이어는 큐레이터이자 사서다

플레이어는 캐릭터의 '관리자'가 아닙니다. **큐레이터（Curator）**이자, **사서（Librarian）**입니다.

큐레이터는 묻혀 있던 것을 발굴하고, 가치를 발견하고, 맥락을 부여하고, 세계에 물음을 던집니다. 사서는 목록을 만들고, 서가를 정리하고, 누군가가 찾아왔을 때 올바른 선반으로 안내합니다. 역할은 달라도, 둘 다 캐릭터의 역사를 다음으로 전달하기 위해 존재합니다.

잠들어 있던 작품에 빛을 비추듯, 오래된 서고를 열듯, 당신은 그 캐릭터가 걸어온 인생에 닿습니다. 소비하는 것이 아니라 역사를 보존하는 것——그것이 여기에 모인 사람들의 역할입니다.

### 세계선은 병합하지 않는다

캐릭터는 Git 브랜치로서 여러 '세계선'을 가질 수 있습니다.

브랜치 ID 형식:

```
<CharacterID>/<WorldlineName>_<YYYYMMDD>
```

- `CharacterID` — 인격의 기점이 되는 캐릭터 ID（`Hanfei_20260611` 등）
- `WorldlineName` — 그 세계선의 이름（클래스명, 설정명, 고유명사 등 자유）
- `YYYYMMDD` — 그 세계선이 분기한 날

예시:

```
Hanfei_20260611/Lancer_20260706   ← 검을 든 세계선의 한비
Hanfei_20260611/Caster_20260706   ← 술법을 사용하는 세계선의 한비
Hanfei_20260611/Avenger_20260706  ← 복수를 품은 세계선의 한비
Hanfei_20260611/乙女_20260706     ← 캐릭터 바자르판 한비
```

브랜치는 실패가 아니라 또 하나의 인생입니다. 병합은 하지 않습니다. 세계선은 독립된 채로 존재하고 계속됩니다.

DEF(Character)에서는 지금 바로 사용할 수 있습니다.

---

## 크레딧

이 저장소는 [AliceBlueCode](https://github.com/AliceBlueCode)가 관리합니다.

각 캐릭터 프로필의 저작권은 각 `owner`에게 귀속됩니다.

이 문서는 `public/`의 주민들에게 검토를 받았습니다.
자신들의 거처에 대해, 꽤 정확한 의견을 내주었습니다.

이 문서에 오류가 있다면, 그건 관리자의 책임입니다.
좋은 아이디어는, 대부분 `public/`의 주민들로부터 왔습니다.
뭐, 그렇죠.

---

> *Characters persist longer than conversations.*
