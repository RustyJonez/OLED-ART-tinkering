# OLED-ART-tinkering
Repo for my original pixel art and projects involving the CRKBD OLED Displays

--- 

## OLED GUIDE
#### A guide for adding custom art to your keeb's OLED Screens.

**Disclaimers: this is going to be very long-winded, and definitely won't be perfect. This is only a list of steps detailing what I did to get mine working.**


### References:

[QMK OLED logo documentation](https://docs.qmk.fm/#/feature_oled_driver?id=logo-example)

[Helix font editor - for making custom glcdfont.c files](https://helixfonteditor.netlify.com/)


So, I'm writing this with the assumptions that you have:

1. A split keyboard with two OLEDs
2. They have already been enabled in your firmware

## GETTING STARTED

* You can grab a copy of the default glcdfont.c from the directory `qmk_firmware/keyboards/crkbd/lib`
**OR**
* [Grab a copy of my glcdfontstarter.c from the github repo](https://github.com/RustyJonez/OLED-ART-tinkering/blob/master/glcdfontstarter)
Just right-click "raw" and "save link as"

In my "starter" copy, I outlined the usable area to make it easy to see your "canvas size" while editing the file. The solid line border corresponds to the outermost pixel border on your OLED screen.

* Open it in the helix font editor page (linked above)
* Edit as desired, download, and save to your keymap directory (in my case `qmk_firmware/keyboards/crkbd/keymaps/rustyjonez`)



* Add this to your config.h file:
`#define OLED_FONT_H "keyboards/crkbd/keymaps/YourUsername/YourFontFileName.c"`
e.g. keyboards/crkbd/keymaps/rustyjonez/glcdfontstarter.c

**This step configures your keymap to point at your local copy, instead of the default font file.**
You can rename your file to whatever you want, just make sure your path matches your username/whatever you've named the font file.




* Finally, make sure you have the right stuff going on in your keymap.c - use the QMK logo documentation above

## KEYMAP

There's a few factors at this point.



**First: rotation** (since it's the first thing addressed in my OLED section of the keymap.c)

[See this link](https://gist.github.com/RustyJonez/52afc16c260ee669702dec846b1967c5#file-keymap-c-L141-L148). The highlighted section shows my master (left hand) OLED orientation - [It's at 270 because I like to display my layer information vertically.](https://i.imgur.com/7IX04iH.jpg) (stole that from /u/drashna 's keymap! Thanks Drashna!)

I've got the `OLED_ROTATION_180` setting for the non-master side. Because the glcdfont's logo space already at 90deg when you edit it, you don't need to rotate it as far to show your image vertically. This was key in making sure my image wasn't a garbled mess.




**Secondly** you'll want to ensure you have the render_logo piece discussed in the QMK docs. 

[In mine, you can see I named it "render_rj_logo"](https://gist.github.com/RustyJonez/52afc16c260ee669702dec846b1967c5#file-keymap-c-L223-L230). 

You can name this however you want, but keep it simple enough so you can reference it in the next piece.




**Third,** [We then reference this directly below,](https://gist.github.com/RustyJonez/52afc16c260ee669702dec846b1967c5#file-keymap-c-L232-L247) specifically in the `render_status_secondary` block at line 241. You can ignore the stuff I have commented out there, just take note of line 246- that's where you'll need to reference your `render_logo  block.


**Lastly**, [ensure your master/slave setup for your halves call the proper render_status.](https://gist.github.com/RustyJonez/52afc16c260ee669702dec846b1967c5#file-keymap-c-L259-L265). 



## Finishing up

Once you compile and have flash both microcontrollers, you should be in business! If you want to do a logo on both sides, or tackle it differently than my setup, that'll be a different can of worms, but is entirely doable. 

Once you play with all the pieces above, you should get a pretty good idea on the basics of how the OLED's do their magic. From there it's just a matter of trial and error!





**Best of luck to anyone who uses this guide, let me know if there's anything that needs clarification or is otherwise confusing. I'm by no means an expert, but if I could figure this out, you definitely can!**

Cheers :D
