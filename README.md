![](https://github.com/Dima353/SM64-Rus-Patch/blob/main/scr.jpg?raw=true)

## Инструкция по применению патчей
1. Скомпилировать необходимую версию согласно иструкции из репозитория;
  
    `Windows` - https://github.com/sm64-port/sm64-port

    `Linux (Steam Deck)` - https://github.com/sm64pc/sm64ex

    `N64` - https://github.com/n64decomp/sm64

    `PSP` - https://github.com/mrneo240/sm64-port

    `PS2` - https://github.com/fgsfdsfgs/sm64-port/tree/ps2

    `PSV` - https://github.com/Dima353/sm64-vita

    `3DS` - https://github.com/mkst/sm64-port

    `Switch` - https://github.com/fgsfdsfgs/sm64ex/tree/switch

3. [Скачать](https://github.com/Dima353/SM64-Rus-Patch/releases) необходимую версию патча;
4. Распаковать архив, перемесить содержимое с заменой файлов;
5. Очистить каталог `build`;
6. Запустить сборку повторно.


##  Описание файлов перевода SM64
###  Текст

* `sm64\text\us\courses.h` - названия миров, описание звёзд.
* `sm64\text\us\dialogs.h` - диалоги.
* `sm64\include\text_strings.h.in` - пункты меню, концовка (если применён патч puppycam, то его строки добавляются в этот файл).
* `sm64\include\text_options_strings.h` - создаётся при использовании патча EXT_OPTIONS_MENU в версии `ex`, содержит строки доп. меню
* `sm64\src\game\area.c` - содержит цветную надпись "PRESS START" на стартовом экране.
* `sm64\src\game\hud.c` - содержит цветную надпись "TIME" и её настройки.

###  Шрифт
* `sm64\textures\segment2` - шрифт 16х8 для диалогов и меню паузы. Шрифт 16х16 - цветной.
* `sm64\levels\menu` - шрифт 8х8 для некоторых элементов меню и названий миров при выборе звёзд.
* `sm64\bin\segment2.c` - отвечает за глифы обычного шрифта, можно добавлять новые и раскоментировать уже имеющиеся.
* `sm64\levels\menu\leveldata.c` - отвечает за глифы цветного шрифта, аналогичен предыдущему.

Например:
```c++
ALIGNED8 static const u8 texture_hud_char_0[] = {
#include "textures/segment2/segment2.00000.rgba16.inc.c"
};

ALIGNED8 static const u8 texture_hud_char_1[] = {
#include "textures/segment2/segment2.00200.rgba16.inc.c"
};

ALIGNED8 static const u8 texture_hud_char_2[] = {
#include "textures/segment2/segment2.00400.rgba16.inc.c"
};

ALIGNED8 static const u8 texture_hud_char_3[] = {
#include "textures/segment2/segment2.00600.rgba16.inc.c"
```
Эти блоки отвечают за цифры, вы можете создавать новые блоки или поискать закомментированные.
После добавления или раскомментирования необходимо добавить значение в таблицу `const u8 *const main_hud_lut` ниже.

```c++
const u8 *const main_hud_lut[] = {
#ifdef VERSION_EU
    texture_hud_char_0, texture_hud_char_1, texture_hud_char_2, texture_hud_char_3,
    texture_hud_char_4, texture_hud_char_5, texture_hud_char_6, texture_hud_char_7,
    texture_hud_char_8, texture_hud_char_9, texture_hud_char_A, texture_hud_char_B,
    texture_hud_char_C, texture_hud_char_D, texture_hud_char_E, texture_hud_char_F,
    texture_hud_char_G, texture_hud_char_H, texture_hud_char_I,               0x0,
    texture_hud_char_K, texture_hud_char_L, texture_hud_char_M, texture_hud_char_N,
    texture_hud_char_O, texture_hud_char_P,               0x0, texture_hud_char_R,
    texture_hud_char_S, texture_hud_char_T, texture_hud_char_U, texture_hud_char_V,
    texture_hud_char_W,               0x0, texture_hud_char_Y, texture_hud_char_Z,
                  0x0,               0x0,               0x0,               0x0,
                  0x0,               0x0,               0x0,               0x0,
                  0x0,               0x0,               0x0,               0x0,
                  0x0,               0x0, texture_hud_char_multiply, texture_```_char_coin,
    texture_hud_char_mario_head, texture_hud_char_star,               0x0,               0x0,
    texture_hud_char_apostrophe, texture_hud_char_double_quote, texture_hud_char_umlaut,
```
Где `0х0` - пустые значения. 
Обращайте внимание на значения `hud`, `font`, `eu`, `us` при редактировании.

* `\sm64\src\game\ingame_menu.c` - параметр `gDialogCharWidths` отвечает за ширину символов, значение указывается в пикселях +1 (например если символ 6 пикселей, пишем значение 7).

Расписал значения таблицы:
```c++
u8 gDialogCharWidths[256] = { // TODO: Is there a way to auto generate this?
    7,  7,  7,  7,  7,  7,  7,  7,  7,  7,  6,  6,  6,  6,  6,  6, // 0 1 2 3 4 5 6 7 8 9 A B C D E F
    6,  6,  6,  6,  6,  6,  8,  8,  6,  6,  6,  6,  6,  6,  8,  6, // G H I J K L M N O P Q R S T U V
    8,  7,  6,  6,  5,  5,  5,  5,  5,  5,  5,  5,  5,  5,  5,  5, // W X Y Z a b c d e f g h i j k l   
    7,  5,  6,  5,  5,  6,  6,  5,  6,  5,  8,  7,  5,  5,  5,  5, // m n o p q r s t u v w x y z
    5,  5,  6,  5,  6,  6,  6,  6,  6,  7,  6,  8,  7,  8,  5,  7, // á é í ó ú Á É Í Ó Ú ñ Ñ ü Ü ¡ ¿
    8,  8,  6,  8,  7,  7,  6,  7,  7,  0,  0,  0,  0,  0,  0,  0,
```
###  Текстуры
* `\sm64\sm64-port-map\actors\power_meter\power_meter_left_side.rgba16.png` и `power_meter_right_side.rgba16.png` - Индикатор энергии
* `\sm64\sm64-port-map\levels\castle_grounds\5.ia8.png` - Подпись "Пич" из стартовой катсцены
* `\sm64\sm64-port-map\levels\menu\main_menu_seg7.0D1A8.rgba16.png` и `main_menu_seg7.0E1A8.rgba16.png` - Надпись "Мир" при выборе уровня
* `\sm64\sm64-port-map\src\minimap\textures` - Кнопки на нижнем экране для 3DS версии
