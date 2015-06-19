Cycle Placement for Sawfish
===========================

A small sawfish script to switch a window's placement and size between multiple 
candidate positions.  Funtionality was inspired by the windows utilty "Winsplit 
Revolution".  Winsplit seems to have disappeared from the internet, but 
there is an [old blogpost](http://www.freewaregenius.com/winsplit-revolution/) 
describing it.

# Usage #

Place the following in your ~/.sawfish/rc

```lisp
(bind-keys window-keymap
	"Super-KP_Left" '(cycle-window-placement-on-current-head (input-focus) '((0 0 25 100) (0 0 50 100)))
)
```

Now repeatedly hitting the "Super-KP_Left" key combination will toggle the 
currently focused window between taking the entire left quarter of the screen 
and the entire left half of the screen on the monitor the window is currently 
positioned on.

# API #

The script exposes two lisp functions:

## cycle-window-placement-on-current-head ##

`(cycle-window-placement-on-current-head window list-of-candidates)`

_window_ is a window, as would be returned by sawfish functions such as 
`(input-focus)`.

_list-of-candidates_ is a list-of-lists.  Each of the contained lists is 
a candidate position.  The candidate position describes where the window should 
be placed as `(x-percent y-percent width-percent height-percent)`.  The first 
invocation of the function will place the window according to the first entry in 
`list-of-candidates`.  Successive invocations will select from the following 
entries in the list.

## move-resize-frame-by-percent-on-current-head ##

`(move-resize-frame-by-percent-on-current-head window x-percent y-percent 
width-percent height-percent)`

_window_ is a window as returned by `(input-focus)`.

_x-percent_ is the desired x position of the window as a percentage of the 
current screen

_y-percent_ is the desired y position of the window as a percentage of the 
current screen

_width-percent_ is the desired width of the window as a percentage of the 
current screen

_height-percent_ is the desired height of the window as a percentage of the 
current screen

# Notes #

`(x y w h)` are specified in percent, as opposed to most window coordinate 
specifications in sawfish, which use pixels.  This is done to make it simpler to 
move your configuration between machines without worrying about screen sizes.

The placement algorithm is pretty naive when it comes to avoiding dock windows 
(such as xfce4-panel).  If you shove a window into a region of the screen that 
contains a dock window, the window will be shrank in such a way as to not 
overlap with the dock, but still be wholly contained within the region.
