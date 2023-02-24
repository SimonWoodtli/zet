# Take a screenshot of a window/area larger than your screen

Scrolling while in screenshot area mode does not work so you need to create a virtual resolution or use another tool to solve the issue.

## Browser Plugins

* brave: My Screen Grab Extension
* firefox: Screenshot

## Linux

Create a virtual screen resolution that is higher than the actual max. monitor resolution.

* on x11: `xrandr --output HDMI-1 --rate 60 --mode 1920x1080 --panning 1920x6000`
* on wayland: `wlr-randr --output <output name> --mode <width>x<height> --transform 1,0,<width>,0,1,<height>,0,0,1`

And then use your screenshot tool of choice.

## Windows

* [Screenshot Captor]

[Screenshot Captor]:<https://www.donationcoder.com/software/mouser/popular-apps/screenshot-captor>

Tags:

    #screenshot #monitor #screen #large
