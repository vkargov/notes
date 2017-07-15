xkb
-

`$ setxkbmap us`

0) `/usr/share/X11/xkb/rules/evdev` and `/usr/share/X11/xkb/rules/evdev.lst` are read. See `maprules.c` for details.
0) First, normal rules are applied (`XkbRF_Normal`) General rules with asterisks(*) have lower priority than the rest.
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
0) Then, append rules are applied (`XkbRF_Append`, they start with a + or a |)
