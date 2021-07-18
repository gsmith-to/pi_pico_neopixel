# pi_pico_neopixel
a library for using ws2812b and sk6812 leds (aka neopixels) with Raspberry Pi Pico

![example](https://github.com/blaz-r/pi_pico_neopixel/blob/main/pico_rgbw_rgb.jpg)

You'll first need to save the neopixel.py file to your device (for example, open it in Thonny and go file > save as and select MicroPython device. Give it the same name). Once it's there, you can import it into your code. 

You create an object with the parameters number of LEDs, state machine ID, GPIO number and mode (RGB or RGBW) in that order. So, to create a strip of 10 leds on state machine 0 and GPIO 0 in RGBW mode you use:

```
from neopixel import Neopixel

pixels = Neopixel(10, 0, 0, "RGBW")
```

Mind that you can use whichever order of RGB / RGBW you want (GRB, WRGB, GRB, RGWB ...). This only represents order of data sent to led-strip, all functions still work with RGBW order. Exact order of leds should be on package of your led-strip. (My BTF-lights sk6812 has GRBW).

This class has many methods, two main ones being show() which sends the data to the strip, and set_pixel which sets the color values for a particular LED. The parameters are LED number and a tuple of form (red, green blue) or (red, green, blue, white) with the colors taking values between 0 and 255.

At the moment, this isn't working with the interpreter, so you have to run it from a file. Looks like it's running just too slow to keep up with the PIO buffer from the interpreter. The key methods are set_pixel(n (r,g,b)), set_pixel_line(p1, p2, (r, g, b)) which sets a row of pixels from pixel p1 to pixel p2 (inclusive), and fill((r,g,b)) which fills all the pixels with the color r, g, b.
If you want to use the library for RGBW, each function works the same just with last parameter being "white": set_pixel(num, (r, g, b, w))

```
pixels.set_pixel(5, (10, 0, 0))
pixels.set_pixel_line(5, 7, (0, 10, 0))
pixels.fill((20, 5, 0))

rgb1 = (0, 0, 50, 0)
rgb2 = (50, 0, 0, 250)
pixels.set_pixel(42, (0, 50, 0, 0))
pixels.set_pixel_line(5, 7, rgb1)
pixels.set_pixel_line_gradient(0, 13, rgb1, rgb2)
```

For new settings to take effect you write:
```
pixels.show()
```

Library also supports HSV colors. For example you can check [smoothRinbow.py](https://github.com/blaz-r/pi_pico_neopixel/blob/develop/examples/smoothRainbow.py) in examples folder. For more info about HSV colors you can check out [Adafruit NeoPixel library documentation](https://learn.adafruit.com/adafruit-neopixel-uberguide/arduino-library-use) and scroll down to HSV section.
To use HSV colors, call colorHSV(hue, sat, val) function with hue, saturation and value as parameters. The function returns rgb tuple that you can then use in all other functions.

```
color = strip.colorHSV(32000, 255, 200)
strip.fill(color)
strip.show()
```

Library is extended verison of https://github.com/blaz-r/pico_python_ws2812b, originally forked from https://github.com/benevpi/pico_python_ws2812b.