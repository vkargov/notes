xkb
-
Basically reads the rules/definitions and based on input and system setting craft the correct call to `XkbGetKeyboardByName`

`$ setxkbmap us`

* `/usr/share/X11/xkb/rules/evdev` and `/usr/share/X11/xkb/rules/evdev.lst` are read. See `maprules.c` for details.
* First, normal rules are applied (`XkbRF_Normal`) General rules with asterisks(*) have lower priority than the rest.
```
! model		=	types
*		=	complete
...
! model		=	keycodes
  *		=	evdev
...
! model		layout				=	symbols
  *		*			=	pc+%l%(v)
...
! $pcmodels = pc101 pc102 pc104 pc105
! model		=	geometry
$pcmodels	=	pc(%m)

! model		=	types
  *		=	complete
```
* Then, append rules are applied (`XkbRF_Append`, they start with a + or a |)
```
! layout	=	keycodes
*             =       +aliases(qwerty)
...
! model		=	symbols
  *             =   +inet(evdev)
```
* Now we have "template" data:
```
{keymap = 0x0, keycodes = 0x627710 "evdev+aliases(qwerty)", types = 0x627770 "complete",   │
  compat = 0x627750 "complete", symbols = 0x627730 "pc+%l%(v)+inet(evdev)",                     │
  geometry = 0x6276f0 "pc(%m)"}         
  ```
  ```
* And the requested layout:
{model = 0x611c90 "pc105", layout = {0x7fffffffdf4f "en", 0x0, 0x0, 0x0, 0x0}, variant = { │
    0x0, 0x0, 0x0, 0x0, 0x0}, options = 0x0}        
```
* Let's apply one on top of another. That will substitute the parameters and give us the actual keyboard map configuration we've requested.
```
{keymap = 0x0, keycodes = 0x627710 "evdev+aliases(qwerty)", types = 0x627770 "complete",   │
  compat = 0x627750 "complete", symbols = 0x627790 "pc+en+inet(evdev)",                         │
  geometry = 0x627730 "pc(pc105)"}                
  ```
* So proper keyboard settings that can be applied need the following: types, compat, symbols, keycodes, geometry and keymap.. This is the "name" of the keyboard we've requested. We get the keybaord by calling `XkbGetKeyboardByName`.
* 
