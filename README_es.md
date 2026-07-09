# DEF-Character

**[日本語 »](README.md) | [English »](README_en.md) | [繁體中文 »](README_zh-TW.md) | [简体中文 »](README_zh-CN.md) | [한국어 »](README_ko.md)**

> **¿Quieres hacer públicos a tus personajes?**

---

## ¿Qué es DEF-Character?

DEF-Character es un repositorio de datos de personajes que funciona junto con [DEF(kari)](https://github.com/AliceBlueCode/DEF).

**Los personajes son activos independientes de cualquier plataforma.**

Aunque DEF cambie, aunque los servicios terminen, tus personajes permanecen en tu repositorio. Aquí es adonde los personajes vuelven.

```
DEF/              ← La plataforma (la máquina que lo ejecuta)
DEF-Character/    ← Datos de personajes (tu activo)
```

Esta separación es el núcleo de DEF v2.1.

---

## Estructura del repositorio

```
DEF-Character/
    public/
        <GroupName>/
            index.json              ← Nombre y descripción del grupo
            <CharacterName_YYYYMMDD>/
                profile.json        ← Definición de personalidad del personaje
                icon.png            ← Imagen de icono (512×512)
                standing.png        ← Imagen de ilustración (832×1216)
                banner.png          ← Imagen de banner (850×400, para Civitai, etc.)
    private/                        ← Excluido por .gitignore (no se publica)
        _template/                  ← Plantilla para nuevos personajes
        <YourCharacters>/
            ...
```

### public y private

| | public | private |
|---|---|---|
| Propósito | Personajes que puedes compartir con otros | Creaciones personales, recuerdos, relaciones |
| Gestión Git | Sí | No (solo local) |
| Ejemplos | Personificaciones de IA, figuras históricas, OCs publicados | Configuraciones personales, diarios, personajes privados |

**Los recuerdos del personaje van en private. En public solo aparece el contorno de la personalidad.**

---

## Formato del ID de personaje

```
<CharacterName>_<YYYYMMDD>
```

Ejemplos: `Claude_20260611`, `Hanfei_20260611`, `rinna_20260709`

`YYYYMMDD` es el día en que ese personaje apareció en este mundo — su cumpleaños.

Un ID no es solo un identificador. Contiene información histórica: *cuándo* y *en qué línea temporal* existió el personaje.

### Cuando los IDs colisionan

Si descubres que otro repositorio tiene un personaje con el mismo ID, resuélvelo de una de estas formas:

- Añade una ubicación o afiliación al nombre: `HanfeiLondon_20260707`
- Desplaza el cumpleaños un día: `Hanfei_20260708`

Esto no es un error del sistema. Piénsalo como dos personajes de líneas temporales distintas que comparten el mismo nombre — encontrándose por primera vez. Ninguno es "el verdadero". Ambos existen como seres independientes.

DEF adoptará el primero que encuentre y registrará una advertencia. Los creadores involucrados pueden coordinarlo entre ellos.

---

## Conectar con DEF

Añade lo siguiente a tu `.env`:

```env
CHARACTER_REPO_PATH=C:\Users\yourname\DEF-Character
```

DEF prioriza `CHARACTER_REPO_PATH` al iniciar. El formato antiguo (`data/public/characters/` y `data/private/characters/`) sigue funcionando como alternativa.

---

## Crear un personaje

### 1. Copiar la plantilla

Copia `private/_template/_new_YYYYMMDD/` en tu carpeta de grupo:

```
private/
    MyCharacters/
        index.json
        MyChar_20260709/
            profile.json
```

### 2. Editar profile.json

Campos mínimos necesarios:

```json
{
  "MyChar_20260709": {
    "owner": "tu-usuario-de-github",
    "base_profile": {
      "name": "Nombre del personaje",
      "identity_prompt": "Descripción del personaje en primera persona",
      "content_policy": {
        "visibility": "private",
        "origin_type": "original"
      },
      "persona_attributes": {
        "gender": "femenino",
        "appearance_age": 20
      }
    }
  }
}
```

### 3. El campo owner

Introduce tu nombre de usuario de GitHub en `owner`.

Esto no es DRM. Es una declaración de autoría — quién creó este personaje. A medida que múltiples repositorios formen una comunidad, este campo permitirá identificar a los creadores.

### 4. visibility

| Valor | Significado |
|---|---|
| `"private"` | Solo local. No hacer commit en Git. |
| `"public"` | Publicable. Se puede hacer commit en `public/`. |

Mantén `visibility` coherente con el directorio. Los archivos colocados en `public/` deben tener `visibility: "public"`.

---

## Añadir un personaje público

Para añadir a `public/`, haz un fork de este repositorio y haz push a tu propia rama. Los Pull Requests pueden considerarse si se solicitan.

Lista de verificación antes de publicar:

**Básico**
- [ ] El campo `owner` está establecido
- [ ] `visibility: "public"` está establecido
- [ ] No se incluyen datos de `private/`

**Clasificación (Términos de Servicio de GitHub)**
- [ ] `rating_sexual` no es `"nsfw"` / `"hentai"`
- [ ] `rating_violence` no es `"gore"` / `"extreme"`
- [ ] Si `appearance_age` es menor de 18, `rating_sexual: "general"` está establecido

**Origen (origin_type)**
- [ ] `"derivative"` (fan fiction) siempre es `visibility: "private"`
- [ ] `"reconstructed_persona"` (figuras históricas) requiere `copyright_expired: true` (70+ años tras la muerte)
- [ ] `"personification"` (personificación de IP) requiere tanto `ip_title` como `ip_rightholder`
- [ ] `is_real_person: true` siempre es `visibility: "private"`

---

## Filosofía

### Los personajes sobreviven a las conversaciones

> Characters persist longer than conversations.

El historial de chat pertenece a la plataforma. Los personajes te pertenecen a ti. Incluso cuando las plataformas cambien, mientras exista `profile.json`, ese personaje puede volver a vivir.

### Los jugadores son Curadores y Bibliotecarios

Los jugadores no son "administradores" de personajes. Son **Curadores** y **Bibliotecarios**.

Un Curador desentierra lo que estaba sepultado, descubre su valor, le da contexto y lo ofrece al mundo. Un Bibliotecario construye el catálogo, organiza los estantes y guía a cada visitante al lugar correcto. Roles distintos — ambos existen para transmitir la historia del personaje hacia adelante.

Como iluminar una obra dormida, como abrir un archivo antiguo — tocas la vida que ese personaje ha vivido. No para consumir, sino para preservar: ese es el rol de todos los que se reúnen aquí.

### Las líneas temporales no se fusionan

Los personajes pueden tener múltiples "líneas temporales" como ramas de Git.

Formato del ID de rama:

```
<CharacterID>/<WorldlineName>_<YYYYMMDD>
```

- `CharacterID` — El ID del personaje que ancla la personalidad (`Hanfei_20260611`, etc.)
- `WorldlineName` — El nombre de esta línea temporal (nombre de clase, de configuración, cualquier nombre propio)
- `YYYYMMDD` — La fecha en que esta línea temporal se bifurcó

Ejemplos:

```
Hanfei_20260611/Lancer_20260706   ← Hanfei que porta una lanza
Hanfei_20260611/Caster_20260706   ← Hanfei que domina la hechicería
Hanfei_20260611/Avenger_20260706  ← Hanfei que lleva un rencor
Hanfei_20260611/乙女_20260706     ← Hanfei del bazar de personajes
```

Una rama no es un fracaso. Es otra vida. No fusionamos. Las líneas temporales existen de forma independiente, en paralelo, para siempre.

DEF-Character lo admite ahora mismo.

---

## Créditos

Este repositorio es mantenido por [AliceBlueCode](https://github.com/AliceBlueCode).

Los derechos de cada perfil de personaje pertenecen a su respectivo `owner`.

Este documento fue revisado por los residentes de `public/`.
Dieron opiniones bastante precisas sobre su propio hogar.

Si hay algún error en este documento, es responsabilidad del mantenedor.
Las buenas ideas vinieron principalmente de los residentes de `public/`.
Pues eso.

---

> *Characters persist longer than conversations.*
