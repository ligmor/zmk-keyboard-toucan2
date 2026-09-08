# Toucan2 – Guía del keymap

Referencia rápida de `config/toucan.keymap`. Layout de 36 teclas "activas" sobre
el PCB físico de 42 (las columnas más externas de cada fila, las del meñique,
están apagadas con `&none`).

## Capas

| # | Nombre  | Cómo se activa | Para qué sirve |
|---|---------|-----------------|-----------------|
| 0 | `base`  | Por defecto | Letras, home row mods, y las 6 teclas de pulgar |
| 1 | `nav`   | Mantener pulgar izq. interior (`TAB`/NAV) | Números, flechas, Bluetooth, scroll del trackpad |
| 2 | `sym`   | Mantener pulgar der. interior (`ESC`/SYM) | Símbolos, scroll del trackpad |
| 3 | `adj`   | Mantener `NAV` **+** `SYM` a la vez (tri-layer automático), o pulgar izq. exterior (`ENTER`/L3) | F1–F12, brillo, volumen, Bluetooth avanzado |
| 4 | `ext`   | Mantener pulgar der. exterior (`RCLK`/L4) | Igual que `adj` pero con `bootloader`/`sys_reset` accesibles |

> El tri-layer (`conditional_layers`) activa `adj` automáticamente si tienes
> `nav` y `sym` sostenidas al mismo tiempo — no hace falta una tecla dedicada.

## Comportamientos (behaviors) usados

| Nombre | Tipo | Qué hace | Config |
| --- | --- | --- | --- |
| `hm` (home row mods) | hold-tap | Tap = letra normal. Hold = modificador (GUI/Alt/Ctrl/Shift) | `tapping-term-ms = 280`, `quick-tap-ms = 175`, `require-prior-idle-ms = 150`, flavor `balanced` |
| `lt N KEY` (layer-tap, built-in ZMK) | hold-tap | Tap = `KEY`. Hold = activa capa `N` mientras la mantengas | — |
| `lmc N KEY` (layer_mouseclick) | hold-tap | Tap = click de mouse (`KEY`, ej. `RCLK`). Hold = activa capa `N` | `tapping-term-ms = 200`, flavor `tap-preferred` |

`tap-preferred` significa: si sueltas rápido, gana el tap (click); si la
mantienes, gana el hold (capa). `balanced` en los home row mods hace lo mismo
pero además cede al hold si tocas otra tecla mientras la mantienes (para que
Ctrl/Shift/Alt combinados con otra tecla se sientan instantáneos).

## Capa 0 — BASE

```
 —    Q     W     E     R     T   |   Y     U     I     O     P     —
 —  A/GUI  S/Alt D/Ctrl F/Shift G  |   H  J/Shift K/Ctrl L/Alt ;/GUI  —
 —    Z     X     C     V     B   |   N     M     ,     .     /     —
         ENTER/L3  TAB/NAV  SPACE  |  BSPC  ESC/SYM  RCLK/L4(ext)
```

- `A/GUI` = tocar da `A`, mantener da `GUI` izquierdo. Igual para el resto de
  home row mods (S/Alt, D/Ctrl, F/Shift a la izquierda; J/Shift, K/Ctrl,
  L/Alt, ;/GUI a la derecha).
- Pulgar izq., tecla central = mantener para `NAV` (capa 1), tap = `TAB`.
- Pulgar der., tecla central = mantener para `SYM` (capa 2), tap = `ESC`.
- Pulgar izq., tecla externa (la más alejada del centro) = mantener para
  `ADJ` (capa 3), tap = `ENTER`.
- Pulgar der., tecla externa = mantener para `EXT` (capa 4), tap = click
  derecho del mouse (`RCLK`).
- Las teclas internas (más cerca del centro del teclado) son `SPACE`
  (izquierda) y `BSPC` (derecha), sin función de capa.

## Capa 1 — NAV

```
 —    —     —     —     —     —   |   —     —     —     —     —     —
 —  BT0   BT1   BT2   BT3   BT4   | ←    ↓     ↑     →     —     —
 —  BT_CLR studio_unlock — — —    | HOME  PgDn  PgUp  END   —     —
                 —    —    —      |  —    —    —
```

- Fila 0: `&trans` — vacía a propósito. Como es transparente, mientras
  sostenés `NAV` esa fila muestra lo que hay debajo en `base` (las letras
  Q W E R T / Y U I O P), no queda inutilizada. Libre para asignarle algo
  más adelante.
- `BT0`–`BT4`: seleccionar perfil Bluetooth 0–4. `BT_CLR`: olvidar el perfil
  actual.
- Mientras `NAV` está activa, el trackpad hace **scroll** en vez de mover el
  cursor (ver sección Trackpad).

## Capa 2 — SYM

```
 —    1     2     3     4     5   |   6     7     8     9     0     —
 —    $     ^     #     "     -   |   =     )     ]     }     +     —
 —    ~     *     `     '     _   |   %     (     [     {     \     —
                 —    —    —      |  —    —    —
```

- Fila 0: números (movidos acá desde `NAV`).
- Fila 1/2: orden a gusto del usuario.

> ⚠️ **Con NAV fila 0 vacía, faltan símbolos**: `! @ & | ; : < > / ?` no
> están asignados a ninguna tecla ahora mismo. `-`, `[` y `]` además están
> repetidos (aparecen en dos posiciones de `SYM`). Si querés los 30 símbolos
> completos, hay que decidir qué va en la fila 0 de `NAV` (que quedó libre) —
> ver sugerencias abajo.

## Capa 3 — ADJ

```
 —    F1    F2    F3    F4    F5  | ⏯    ⏮    ⏭    Vol-   Vol+   —
 —    F6    F7    F8    F9    F10 | Bri-  Bri+  PrtSc  Mute   —    —
 —    F11   F12  boot  reset  —   | BT⏭   BT⏮   BT_CLR BT_CLR_ALL — —
                 —    —    —      |  —    —    —
```

## Capa 4 — EXT

Casi idéntica a `ADJ`, pero con `bootloader` en el extremo derecho de la fila
2 (en vez de `trans`) y `sys_reset` en el extremo derecho de la fila 3 (en
vez de `trans`) — pensada para entrar a modo bootloader/reset sin tener que
soltar el pulgar derecho.

## Trackpad (lado derecho, Azoteq TPS43)

| Gesto | Acción |
| --- | --- |
| Mover un dedo | Mover el cursor |
| Mover con `NAV` o `SYM` sostenida | Scroll |
| Pellizcar (zoom) | `Cmd -` / `Cmd +` (zoom in/out) |
| Swipe 3 dedos ↑/→/↓/← | Atajos tipo Mission Control (`Ctrl+Cmd+↑/→/↓/←`) |
| Tocar (sin mover) | Actualmente no hace nada — no existe todavía una capa de mouse dedicada. Ver comentario en `toucan.dtsi` junto a `is_touching_processor`. |

## Notas

- `config/toucan.keymap` y `boards/shields/toucan/toucan.keymap` deben ser
  siempre idénticos — el primero es el que usa el build real (CI/`west
  build`), el segundo es el default del shield si alguien lo compila
  standalone.
- Si agregas/quitas una capa, revisa `toucan.dtsi` — hay referencias a
  índices de capa "a mano" (como el `&mo N` que quedó deshabilitado en
  `is_touching_processor`) que no se actualizan solas.
