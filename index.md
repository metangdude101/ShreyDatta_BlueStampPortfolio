# OpenAI Camera
My OpenAI Camera uses the Adafruit Memento camera and connects to OpenAI to create text descriptions for images it takes in a variety of different ways. I aim to add a speaker, a way to send photos to other devices, and filters for photos.
<!-- Update this text with a brief description (2-3 sentences) of your project. This description should draw the reader in and make them interested in what you've built. You can include what the biggest challenges, takeaways, and triumphs from completing the project were. As you complete your portfolio, remember your audience is less familiar than you are with all that your project entails! -->

<!-- You should comment out all portions of your portfolio that you have not completed yet, as well as any instructions: -->

<!-- <embed src="https://minecraft-eaglercraft.github.io/go/minecraft-1.5.2/" style="width:500px; height: 500px;"> -->

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Shrey D | Lynbrook High School | Mechanical Engineering | Incoming Sophomore

<!-- **Replace the BlueStamp logo below with an image of yourself and your completed project. Follow the guide [here](https://tomcam.github.io/least-github-pages/adding-images-github-pages-site.html) if you need help.** -->

<img src="ShreyD.jpg" width="378" height="504">
  
<!-- # Final Milestone

**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="560" height="315" src="https://www.youtube.com/embed/F7M7imOVGug" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

For your final milestone, explain the outcome of your project. Key details to include are:
- What you've accomplished since your previous milestone
- What your biggest challenges and triumphs were at BSE
- A summary of key topics you learned about
- What you hope to learn in the future after everything you've learned at BSE -->



# Second Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/m5oPV_kyGF0?si=Vs4LNTCf7iLXl55b" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## Summary

The second milestone for my OpenAI Camera was focused on quality of life, including editing the code I already had for the camera to make it more intuitive to interact with, and adding more features people would want in a camera, because it lacked many things. I combined the code I was given from the OpenAI Camera program with the code from the Fancy Camera program to add the ability to change settings, and on top of that I added more features. The features I've added are: a countdown timer of various lengths, a flash setting, custom displays for each setting, a bottom status bar that details how the camera works, and randomized messages from the Memento so it has more of a personality.

<img src="Untitled drawing (11).png" width="600" height="600">
A diagram showcasing how I combined the OpenAI Camera code and the Fancy Camera code to create my own code, which had features from both programs.
<a href="https://learn.adafruit.com/openai-image-descriptors-with-memento/circuitpython-code" target="_blank">OpenAI Camera</a>
<a href="https://learn.adafruit.com/memento-camera-quick-start-guide/fancy-camera" target="_blank">Fancy Camera</a>

## Challenges

I had a lot of trouble when I was adding my own new features to the camera's code, especially with flash. The way flash works on a regular camera is that a bright light turns on, the camera takes a photo, and then that bright light turns off. That all happens in an instant. When I coded flash onto my camera, the light worked perfectly, and the images taken with flash on were noticably brighter. However, for some reason, when taking an image with flash, the screen wouldn't update when the image was taken, so it looked like an image was taken without flash. I spent an entire week trying to figure out why it wasn't working, but gave up and decided to work on other features. However, when I eventually came back to it, I figured out there was a command that updated the camera's screen, so if I updated the camera's screen once the flash would turn on, the screen would show an image taken with flash.

I also had a little bit of trouble with making displays for the camera settings. These are the camera effects I currently display on the screen: prompt sent to OpenAI, camera filter, flash, LED level, LED color, and a countdown timer. If I displayed them all as text, they would either be super tiny and hard to read, or there wouldn't be enough space to fit all of them. To solve this problem, I created custom visual displays for LED level, LED color, and flash. I was unfamiliar with how to render polygons onto the screen, and so I looked up modules like vectorio and displayio. I figured out I could make my own polygons, but I would have to manually enter points for the vertices of the polygon. Eventually, I was able to make a lightning bolt that changes color with flash, and bars that change their levels and colors corresponding to the LEDs.

## Next Steps

The camera, despite having an enclosure, is still quite exposed. It's really easy to access the inside of the board, or mess with the SD card, so I think my next milestone will be 3D printing a case. With a case, I can protect the camera much more, and I can make it much easier to hold.

# First Milestone

<!-- **Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.** -->

<iframe width="560" height="315" src="https://www.youtube.com/embed/L-g1tkvBFc0?si=UEiRiNsshfH5kpLt" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## Summary

The first milestone for my OpenAI Camera was its assembly and installing CircuitPython on it. When I received my project, the board lacked an enclosure, which I had to assemble myself. In addition, the camera had a very bare bones app running on it, so I had to install CircuitPython, a version of Python suitable for microcontrollers, so I could install a much better camera app.

## Challenges

Going into this project, I thought I would receive my Memento fully assembled, and ready to install CircuitPython on. However, when I actually started this project, I found out that the board for the Memento and its enclosure needed to be assembled. The enclosure was two plates that I could screw on onto the top and bottom of the board. The enclosure plate that I could screw on to the camera side had a ring of Neopixels on it, that needed to be connected to the board with wiring so I could control them. Luckily, the assembly wasn't that hard. The next step for setting up the Memento was to install CircuitPython on it. Normally, this would be an easy task. The Memento typically has its own lithium ion battery, which provides power automatically. To install CircuitPython, the Memento just has to be connected to a Laptop via USB, and then CircuitPython can be installed. However, I wasn't allowed to use a lithium ion battery, so my Memento's one USB-C port was always used to power it, not to transfer data. For some reason, whenever I plugged my Memento into my laptop, it wouldn't turn on, and the laptop wouldn't recognize it as a USB device. After a lot of fiddling with the cable, I figured out when I plugged in a specific end of the cable into my Memento first, and then the other end into my laptop, the Memento would turn on and be connected. I don't know exactly why it works, but I was able to install CircuitPython with no other hiccups.

## Next Steps

Right now, the board has a camera application that is very bare bones and can't edit any of its own settings. My next steps are to upload the OpenAI Camera program to the board so it can send photos to OpenAI, and maybe edit it so the project becomes more of my own.

## Images
<img src="20250709_100959.png" width=378 height=504>
An image of my Memento from the screen side, showcasing its buttons.
<img src="20250709_101010.png" width=378 height=504>
An image of my Memento from the camera side, showcasing the camera and the Neopixels.
<img src="20250709_101029.png" width-378 height=504>
An image of my Memento from the side with the ports and the MicroSD card.

# Schematics 
<img src="adafruit_products_schem (1).png">
A schematic of most of the modules on my Memento board.
<a href="https://learn.adafruit.com/adafruit-memento-camera-board/downloads" target="_blank">Source</a>
<img src="adafruit_products_mementoSchem2.png">
A second schematic containing more modules on my Memento board.
<a href="https://learn.adafruit.com/adafruit-memento-camera-board/downloads" target="_blank">Source</a>
<img src="adafruit_products_cam_schem.png">
A third schematic that is about the camera on my Memento board.
<a href="https://learn.adafruit.com/adafruit-ov5640-camera-breakout/downloads" target="_blank">Source</a>


# Code

```python
# SPDX-FileCopyrightText: 2024 Liz Clark for Adafruit Industries
#
# SPDX-License-Identifier: MIT

import os
import time
import ssl
import binascii
import wifi
import vectorio
import socketpool
import adafruit_requests
import displayio
from jpegio import JpegDecoder
from adafruit_display_text import label, wrap_text_to_lines
import terminalio
import adafruit_pycamera
import random
import asyncio

"""

A lot of the code in this project is from open-source code for the Memento Camera, mainly two projects, the OpenAI Camera and the Fancy Camera. Both of these projects are amazing by themselves, but both of them lack very important functionality, so I combined them to make a better script.

"""

"""

This section is basically configuring settings before the camera turns on, so everything works properly.

"""

# scale for displaying returned text from OpenAI
text_scale = 1

# OpenAI key and prompts from settings.toml
openai_api_key = os.getenv("OPENAI_API_KEY")
alt_text_prompt = os.getenv("ALT_TEXT_PROMPT")
haiku_prompt = os.getenv("HAIKU_PROMPT")
cable_prompt = os.getenv("CABLE_PROMPT")
translate_prompt = os.getenv("TRANSLATE_PROMPT")
alien_prompt = os.getenv("ALIEN_PROMPT")
weird_prompt = os.getenv("WEIRD_PROMPT")

pokemon_prompt = os.getenv("POKEMON_PROMPT")
translate_prompt = os.getenv("TRANSLATE_PROMPT")
define_prompt=os.getenv("DEFINE_PROMPT")

# Putting all the prompts in a list
prompts = [alt_text_prompt,
           haiku_prompt,
           define_prompt,
           pokemon_prompt,
           cable_prompt,
           translate_prompt,
           weird_prompt]
num_prompts = len(prompts)
prompt_index = 0
# Adding labels for the prompts that will show up on the screen
prompt_labels = ["Alt Text", "Haiku", "Define", "Pokedex", "Cable ID","Translate", "Weird"]


# Setting flash to be off by default
flash = False

palette = displayio.Palette(2)
palette[0] = 0xFFFFFF
palette[1] = 0x000000

pycam = adafruit_pycamera.PyCamera()

# Startup tone for camera, currently commented out
rand = random.randint(0,9)
rand2 = random.randint(0,99)

if rand2 == 0:
    if rand == 0:
        pycam.tone(208, 0.4)
        pycam.tone(349, 0.4)
        pycam.tone(311, 0.4)
        pycam.tone(262, 0.2)
        pycam.tone(208, 0.2)
        pycam.tone(233, 0.2)
        pycam.tone(262, 0.2)
        pycam.tone(233, 0.2)
        pycam.tone(208, 0.2)
        pycam.tone(175, 0.4)
        pycam.tone(156, 0.4)
    else:
        pycam.tone(831, 0.2)
        pycam.tone(659, 0.2)
        pycam.tone(831, 0.2)
        pycam.tone(932, 0.2)
        pycam.tone(1047, 0.2)
        pycam.tone(932, 0.2)
        pycam.tone(831, 0.2)
        pycam.tone(659, 0.2)
        pycam.tone(622, 0.4)
else:
    pycam.tone(330/2, 0.2)

pycam.mode = 0  # only mode 0 (JPEG) will work in this example

# Resolution of 320x240 is plenty for OpenAI
pycam.resolution = 1  # 0-12 preset resolutions:
#                      0: 240x240, 1: 320x240, 2: 640x480, 3: 800x600, 4: 1024x768,
#                      5: 1280x720, 6: 1280x1024, 7: 1600x1200, 8: 1920x1080, 9: 2048x1536,
#                      10: 2560x1440, 11: 2560x1600, 12: 2560x1920


pycam.led_level = 0  # 0-4 preset brightness levels
led_levels = (
    "No Light",
    "Level 1",
    "Level 2",
    "Level 3",
    "Level 4"
)
current_level = 0

# pycam.led_color = 0  # 0-7  preset colors: 0: white, 1: green, 2: yellow, 3: red,
#                                          4: pink, 5: blue, 6: teal, 7: rainbow
led_colors = (
    "White",
    "Green",
    "Yellow",
    "Red",
    "Magenta",
    "Blue",
    "Cyan",
    "Rainbow"
)
corr_colors= (
    0xFFFFFF,
    0x00FF00,
    0xFFFF00,
    0xFF0000,
    0xFF00FF,
    0x0000FF,
    0x00FFFF,
    0xFFFFFF
)
current_color = 0

pycam.effect = 0  # 0-7 preset FX: 0: normal, 1: invert, 2: b&w, 3: red,
#                                  4: green, 5: blue, 6: sepia, 7: solarize
effects = (
    "None",
    "Invert",
    "B & W",
    "Red",
    "Green",
    "Blue",
    "Sepia",
    "Solarize"
)

effect_colors = (
    0xFFFFFF,
    0xFFFFFF,
    0x555555,
    0xFF0000,
    0x00FF00,
    0x0000FF,
    0x995500,
    0xFF6600
)

current_effect = 0

# add label for selected prompt
rect = vectorio.Rectangle(pixel_shader=palette, width=240, height=50, x=0, y=-10, color_index=1)
recttwo = vectorio.Rectangle(pixel_shader=palette, width = 240, height=50, x=0, y=0, color_index=1)

lightningx = 195

# Hand made lightning symbol
pointlist=[(0,0), (-5, 13), (3, 15), (0, 24), (13, 11), (5, 9), (10, 0)]
lightning = vectorio.Polygon(pixel_shader=palette, points=pointlist, x=210-lightningx, y=5)

# Inner lightning symbol that is turned black or white
innerpointlist=[(1, 1), (-3, 12), (5, 14), (1, 22), (11, 12), (3, 10), (8, 1)]
innerlightning = vectorio.Polygon(pixel_shader=palette, points=innerpointlist, x=210-lightningx, y=5, color_index=1)

# Text that displays prompt on screen
prompt_txt = label.Label(
            terminalio.FONT, text=prompt_labels[prompt_index], color=0xFFFFFF, x=120, y=10, scale=2
        )

effect_txt = label.Label(
            terminalio.FONT, text=effects[current_effect], color=effect_colors[current_effect], x=10, y=10, scale=2
        )

# pylint: disable=protected-access
pycam._botbar.append(rect)
pycam._botbar.append(prompt_txt)
pycam._botbar.append(effect_txt)

pycam._topbar.append(recttwo)

#pycam._topbar.append(led_txt)
#pycam._topbar.append(color_txt)

pycam._topbar.append(lightning)
pycam._topbar.append(innerlightning)


allcolors = displayio.Palette(15)
allcolors[0]  = 0xFFFFFF
allcolors[1]  = 0x00FF00
allcolors[2]  = 0xFFFF00
allcolors[3]  = 0xFF0000
allcolors[4]  = 0xFF00FF
allcolors[5]  = 0x0000FF
allcolors[6]  = 0x00FFFF

allcolors[7]  = 0x444444
allcolors[8]  = 0x004400
allcolors[9]  = 0x444400
allcolors[10] = 0x440000
allcolors[11] = 0x440044
allcolors[12] = 0x000044
allcolors[13] = 0x004444

allcolors[14] = 0x000000

barsx = 35

# Colored bars that represent color and level of LED lighting
bar1 = vectorio.Rectangle(pixel_shader=allcolors, width=12, height=24, x=5+barsx,  y=5, color_index=7)
bar2 = vectorio.Rectangle(pixel_shader=allcolors, width=12, height=24, x=22+barsx, y=5, color_index=7)
bar3 = vectorio.Rectangle(pixel_shader=allcolors, width=12, height=24, x=39+barsx, y=5, color_index=7)
bar4 = vectorio.Rectangle(pixel_shader=allcolors, width=12, height=24, x=56+barsx, y=5, color_index=7)

pycam._topbar.append(bar1)
pycam._topbar.append(bar2)
pycam._topbar.append(bar3)
pycam._topbar.append(bar4)

def updatebars():
    global current_level
    global current_color

    bars = [7, 7, 7, 7]

    if current_level > 0:
        bars[0] -= 7
    if current_level > 1:
        bars[1] -= 7
    if current_level > 2:
        bars[2] -= 7
    if current_level > 3:
        bars[3] -= 7
    
    
    if current_color != 7:
        bars[0] += current_color
        bars[1] += current_color
        bars[2] += current_color
        bars[3] += current_color
    else:
        bars[0] += 3
        bars[1] += 2
        bars[2] += 1
        bars[3] += 5
    
    bar1.color_index = bars[0]
    bar2.color_index = bars[1]
    bar3.color_index = bars[2]
    bar4.color_index = bars[3]

# pylint: enable=protected-access
pycam.display.refresh()

view = False
new_prompt = False
file_index = -1

settings = (
    "effect",
    "flash",
    "led_level",
    "led_color",
    "prompt"
)

setting_displays = (
    "Changing\nEffect",
    "Changing\nFlash",
    "Changing\nLED Level",
    "Changing\nLED Color",
    "Changing\nAI Prompt"
)
curr_setting = 0

"""

This next section is the code for sending the image to OpenAI.
Most of it comes from the OpenAI Camera source code from Adafruit.
I have barely edited this, so it should work without much error.

"""

# sort image files by numeric order
all_images = [
    f"/sd/{filename}"
    for filename in os.listdir("/sd")
    if filename.lower().endswith(".jpg")
    ]
all_images.sort(key=lambda f: int(''.join(filter(str.isdigit, f))))

decoder = JpegDecoder()
# used for showing images from sd card
bitmap = displayio.Bitmap(240, 176, 65535)

# encode jpeg to base64 for OpenAI
def encode_image(image_path):
    with open(image_path, 'rb') as image_file:
        image_data = image_file.read()
        base64_encoded_data = binascii.b2a_base64(image_data).decode('utf-8').rstrip()
        return base64_encoded_data

# view returned text on MEMENTO screen
def view_text(the_text):
    rectangle = vectorio.Rectangle(
        pixel_shader=palette, width=190, height=120, x=25, y=60, color_index=1
    )
    pycam.splash.append(rectangle)
    the_text = "\n".join(wrap_text_to_lines(the_text, 30))
    if prompt_index == 1:
        the_text = the_text.replace("*", "\n")
    text_area = label.Label(terminalio.FONT, text=the_text,
                            color=0xFFFFFF, x=30, y=70, scale=text_scale)
    pycam.splash.append(text_area)
    pycam.display.refresh()

# send image to OpenAI, print the returned text and save it as a text file
def send_img(img, prompt):
    base64_image = encode_image(img)
    headers = {
      "Content-Type": "application/json",
      "Authorization": f"Bearer {openai_api_key}"
    }
    payload = {
      "model": "gpt-4-turbo",
      "messages": [
        {
          "role": "user",
          "content": [
            {
              "type": "text",
              "text": f"{prompt} Limit your response to 210 characters."
            },
            {
              "type": "image_url",
              "image_url": {
                "url": f"data:image/jpeg;base64,{base64_image}"
              }
            }
          ]
        }
      ],
      "max_tokens": 300
    }
    response = requests.post("https://api.openai.com/v1/chat/completions",
                             headers=headers, json=payload)
    json_openai = response.json()
    print(json_openai['choices'][0]['message']['content'])
    alt_text_file = img.replace('jpg', 'txt')
    alt_text_file = alt_text_file[:11] + f"_{prompt_labels[prompt_index]}" + alt_text_file[11:]
    if prompt_index == 5:
        alt_text_file = alt_text_file.replace("?", "")
    with open(alt_text_file, "a") as fp:
        fp.write(json_openai['choices'][0]['message']['content'])
        fp.flush()
        time.sleep(1)
        fp.close()
    view_text(json_openai['choices'][0]['message']['content'])
# view images on sd card to re-send to OpenAI
def load_image(bit, file):
    bit.fill(0b00000_000000_00000)  # fill with a middle grey
    decoder.open(file)
    decoder.decode(bit, scale=0, x=0, y=0)
    pycam.blit(bit, y_offset=32)
    pycam.display.refresh()

print()
print("Connecting to WiFi")
wifi.radio.connect(
    os.getenv('CIRCUITPY_WIFI_SSID'),
    os.getenv('CIRCUITPY_WIFI_PASSWORD')
)
print("Connected to WiFi")
pool = socketpool.SocketPool(wifi.radio)
requests = adafruit_requests.Session(pool, ssl.create_default_context())

"""

The next section of code is what the camera does after everything is set up properly

"""

pycam.display_message("Flash Off", color=0xFFFFFF)

while True:
    if new_prompt:
        pycam.display_message("SEND?")
    if not view:
        if not new_prompt:
            pycam.blit(pycam.continuous_capture())
    pycam.keys_debounce()
    if pycam.shutter.long_press:
        pycam.tone(330/2, 0.4)
        pycam.autofocus()
    if pycam.shutter.short_count:
        pycam.tone(330/2, 0.2)
        pycam.tone(396/2, 0.2)
        pycam.tone(495/2, 0.2)
        pycam.tone(587/2, 0.2)


        try:
            if flash:
                # turning on LEDs when the camera tries to take a photo
                setattr(pycam, "led_level", 0)
                setattr(pycam, "led_level", current_level)
            pycam.live_preview_mode()
            pycam.capture_jpeg()
            pycam.display_message("snap", color=0xFFFFFF)
            pycam.live_preview_mode()
        except TypeError as exception:
            pycam.display_message("Failed", color=0xFF0000)
            time.sleep(0.5)
            pycam.live_preview_mode()
        except RuntimeError as exception:
            pycam.display_message("Error\nNo SD Card", color=0xFF0000)
            time.sleep(0.5)
        all_images = [
        f"/sd/{filename}"
        for filename in os.listdir("/sd")
        if filename.lower().endswith(".jpg")
        ]
        all_images.sort(key=lambda f: int(''.join(filter(str.isdigit, f))))
        the_image = all_images[-1]
        pycam.display_message("OpenAI..", color=0xFFFFFF)
        send_img(the_image, prompts[prompt_index])
        pycam.tone(587/2, 0.2)
        pycam.tone(495/2, 0.2)
        pycam.tone(396/2, 0.2)
        pycam.tone(330/2, 0.2)
        view = True
        if flash:
            setattr(pycam, "led_level", 0)

    if pycam.up.fell:
        key = settings[curr_setting]
        if key:
            if key == "prompt":
                prompt_index = (prompt_index + 1) % num_prompts
                prompt_txt.text = prompt_labels[prompt_index]
                pycam.display.refresh()
            elif key == "flash":
                flash = not flash
                if not flash:
                    setattr(pycam, "led_level", current_level)
                    innerlightning.color_index = 1
                    pycam.display.refresh()
                    pycam.display_message("Flash Off", color=0xFFFFFF)
                    time.sleep(0.25)
                else:
                    setattr(pycam, "led_level", 0)
                    if current_level == 0:
                        current_level = 1
                        updatebars()
                        pycam.display.refresh()
                    innerlightning.color_index = 0
                    pycam.display.refresh()
                    pycam.display_message("Flash On", color=0xFFFFFF)
                    time.sleep(0.25)
            else:
                print("getting", key, getattr(pycam, key))
                setattr(pycam, key, getattr(pycam, key) + 1)
                if key == "led_color":
                    current_color = (current_color + 1) % len(led_colors)
                    updatebars()
                    pycam.display.refresh()
                    pycam.display_message(led_colors[current_color], color=corr_colors[current_color])
                    time.sleep(0.25)
                elif key == "led_level":
                    current_level = (current_level + 1) % len(led_levels)
                    updatebars()
                    pycam.display.refresh()
                    pycam.display_message(led_levels[current_level], color=0xFFFFFF)
                    time.sleep(0.25)
                elif key == "effect":
                    current_effect = (current_effect + 1) % len(effects)
                    effect_txt.text = effects[current_effect]
                    effect_txt.color = effect_colors[current_effect]
                    pycam.display.refresh()
                    pycam.display_message(effects[current_effect], color=effect_colors[current_effect])

    if pycam.down.fell:
        key = settings[curr_setting]
        if key:
            if key == "prompt":
                prompt_index = (prompt_index - 1) % num_prompts
                prompt_txt.text = prompt_labels[prompt_index]
                pycam.display.refresh()
            elif key == "flash":
                flash = not flash
                if not flash:
                    setattr(pycam, "led_level", current_level)
                    innerlightning.color_index = 1
                    pycam.display.refresh()
                    pycam.display_message("Flash Off", color=0xFFFFFF)
                    time.sleep(0.25)
                else:
                    setattr(pycam, "led_level", 0)
                    if current_level == 0:
                        current_level = 1
                        updatebars()
                        pycam.display.refresh()
                    innerlightning.color_index = 0
                    pycam.display.refresh()
                    pycam.display_message("Flash On", color=0xFFFFFF)
                    time.sleep(0.25)
            else:
                setattr(pycam, key, getattr(pycam, key) - 1)
                if key == "led_color":
                    current_color = (current_color - 1) % len(led_colors)
                    updatebars()
                    pycam.display.refresh()
                    pycam.display_message(led_colors[current_color], color=corr_colors[current_color])
                    time.sleep(0.25)
                elif key == "led_level":
                    current_level = (current_level - 1) % len(led_levels)
                    updatebars()
                    pycam.display.refresh()
                    pycam.display_message(led_levels[current_level], color=0xFFFFFF)
                    time.sleep(0.25)
                elif key == "effect":
                    current_effect = (current_effect - 1) % len(effects)
                    effect_txt.text = effects[current_effect]
                    effect_txt.color = effect_colors[current_effect]
                    pycam.display.refresh()
                    pycam.display_message(effects[current_effect], color=effect_colors[current_effect])

    if pycam.right.fell:
        if new_prompt:
            file_index = (file_index - -1) % -len(all_images)
            filename = all_images[file_index]
            load_image(bitmap, filename)
        else:
            curr_setting = (curr_setting + 1) % len(settings)
            if pycam.mode_text != "LAPS" and settings[curr_setting] == "timelapse_rate":
                curr_setting = (curr_setting + 1) % len(settings)
            print(settings[curr_setting])
            pycam.select_setting(settings[curr_setting])
            pycam.display_message(setting_displays[curr_setting], color=0xFFFFFF, scale=2)
            if settings[curr_setting] == "led_level":
                setattr(pycam, "led_level", current_level)
            else:
                if flash:
                    setattr(pycam, "led_level", 0)
            time.sleep(0.25)

    if pycam.left.fell:
        if new_prompt:
            file_index = (file_index + -1) % -len(all_images)
            filename = all_images[file_index]
            load_image(bitmap, filename)
        else:
            curr_setting = (curr_setting - 1) % len(settings)
            if pycam.mode_text != "LAPS" and settings[curr_setting] == "timelapse_rate":
                curr_setting = (curr_setting - 1) % len(settings)
            print(settings[curr_setting])
            pycam.select_setting(settings[curr_setting])
            pycam.display_message(setting_displays[curr_setting], color=0xFFFFFF, scale=2)
            if settings[curr_setting] == "led_level":
                setattr(pycam, "led_level", current_level)
            else:
                if flash:
                    setattr(pycam, "led_level", 0)
            time.sleep(0.25)

    if pycam.select.fell:
        if not new_prompt:
            file_index = -1
            new_prompt = True
            filename = all_images[file_index]
            load_image(bitmap, filename)
        else:
            new_prompt = False
            pycam.display.refresh()

    if pycam.ok.fell:
        if view:
            pycam.splash.pop()
            pycam.splash.pop()
            pycam.display.refresh()
            view = False
        if new_prompt:
            pycam.display_message("OpenAI..", color=0x00DD00)
            send_img(filename, prompts[prompt_index])
            new_prompt = False
            view = True
```

# Bill of Materials

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| Adafruit Memento Camera Board | Taking photos and housing other components | $34.95 | <a href="https://www.adafruit.com/product/5420"> Link </a> |
| 3.7V 420mAh Lithium Ion Polymer Battery | Power | $6.95 | <a href="https://www.adafruit.com/product/4236"> Link </a> |
| 256MB Micro SD Card | Storing photos and text | $4.50 | <a href="https://www.adafruit.com/product/5251"> Link </a> |

# Starter Project: Retro Arcade Console
<iframe width="560" height="315" src="https://www.youtube.com/embed/QSVcFWAX7O8?si=au_ZTLx9YDTecXB0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>


<img src="20250618_115017cropped.jpg" width="226" height="300">


My starter project was a mini retro arcade console. This project marked the first time I had to solder anything. Naturally I had some problems in the beginning, but I've learned how to make and recognize good soldering work, after soldering dozens of joints. One of the challenges with this project was when I soldered something upside-down, a semi-permanent mistake that I had no idea how to fix. But, with the help of my instructors, I was able to fix the problem and create a finished product I'm happy with.

# Schematics

<img src="schematics-_WNfuLqZO8t.jpg">
<a href="https://www.hackster.io/lewisdiy/build-your-own-game-console-kit-play-the-classic-games-5ca95f#schematics" target="_blank">Source</a>

# Bill of Materials

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| DIY Soldering Project Game Kit Retro Classic Electronic Soldering Kit with 5 Retro Classic Games and Acrylic Case | Code for game, housing, and all parts neccesary | $24.99 | <a href="https://etoput.com/products/diy-soldering-project-game-kit-retro-classic-electronic-soldering-kit-with-5-retro-classic-games-and-acrylic-case"> Link </a> |

<!-- # Other Resources/Examples
One of the best parts about Github is that you can view how other people set up their own work. Here are some past BSE portfolios that are awesome examples. You can view how they set up their portfolio, and you can view their index.md files to understand how they implemented different portfolio components.
- [Example 1](https://trashytuber.github.io/YimingJiaBlueStamp/)
- [Example 2](https://sviatil0.github.io/Sviatoslav_BSE/)
- [Example 3](https://arneshkumar.github.io/arneshbluestamp/)

To watch the BSE tutorial on how to create a portfolio, click here. -->
