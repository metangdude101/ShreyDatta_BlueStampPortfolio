# OpenAI Camera
My OpenAI Camera uses the Adafruit Memento camera and connects to OpenAI to create text descriptions for images it takes in a variety of different ways. I aim to add a speaker, a way to send photos to other devices, and filters for photos.
<!-- Update this text with a brief description (2-3 sentences) of your project. This description should draw the reader in and make them interested in what you've built. You can include what the biggest challenges, takeaways, and triumphs from completing the project were. As you complete your portfolio, remember your audience is less familiar than you are with all that your project entails! -->

<!-- You should comment out all portions of your portfolio that you have not completed yet, as well as any instructions: -->


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

The second milestone for my OpenAI Camera was focused on quality of life, including editing the code I already had for the camera to make it more intuitive to interact with, and adding more features people would want in a camera. 

## Challenges

The code I was given to use with my Adafruit Memento camera had the amazing ability to send photos over the wifi to OpenAI and get a response, but lacked the ability to change the camera settings itself. I had no idea how the code for the camera worked, and I had no idea how to add the ability for users to modify camera settings.

## Solution

I had no idea how to do this myself, so I looked at code people had already made for the camera. I found something called Fancy Camera, which had very intuitive controls for the camera and let the user change filters, resolution, LED level, and LED color, to name a few. After a lot of tinkering, I was able to seamlessly merge the two scripts together, making a camera that is both functional and easy to use.

<img src="Untitled drawing (11).png" width="378" height="504">

## Next Steps

At this point, the camera works well, but it doesn't look the greatest, and doesn't have a lot of features. I also have to keep it constantly plugged in to a device, because it lacks a battery. To solve these problems, I will try to add symbols to the screen of the camera to make it easier to understand, and maybe try adding a battery pack to lengthen the life of the device without needing to be plugged in.

# First Milestone

<!-- **Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.** -->

<iframe width="560" height="315" src="https://www.youtube.com/embed/L-g1tkvBFc0?si=UEiRiNsshfH5kpLt" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## Summary

The first milestone for my OpenAI Camera was its assembly and installing CircuitPython on it. The board had an enclosure that had the LEDs to add camera flash, and CircuitPython was needed to run any complex code on the camera. 

## Challenges

When I received the parts for the project, I was surprised to see one board and a bunch of unassembled casing. Additionally, I couldn't install CircuitPython onto the board. For the board to get CircuitPython, it needs to be plugged into a laptop and turned on. However, whenever I plugged the board into my laptop, it wouldn't turn on, no matter what I did. If the board didn't turn on, I could not get any code onto it, and the whole project would be over.

## Solution

It took me a little while to assemble the case around the board, but it wasn't super hard. For CircuitPython, I realized the issue was that my device was not recognizing when the board plugged into it. After tinkering with the USB cable connecting my device to the board, I realized if I plugged a specific end of the cable into my board, and then plugged the other end into my laptop, the board would turn on. After that, I was able to successfully install CircuitPython on the board.

## Next Steps

Right now, the board has a camera application that is very bare-bones and can't do anything. My next steps are to upload the code to the board, and maybe modify it to make it my own.

## Images
<img src="20250709_100959.png" width=378 height=504>
<img src="20250709_101010.png" width=378 height=504>
<img src="20250709_101029.png" width-378 height=504>

# Schematics 
<img src="adafruit_products_schem (1).png">
<a href="https://learn.adafruit.com/adafruit-memento-camera-board/downloads" target="_blank">Source</a>
<img src="adafruit_products_mementoSchem2.png">
<a href="https://learn.adafruit.com/adafruit-memento-camera-board/downloads" target="_blank">Source</a>
<img src="adafruit_products_cam_schem.png">
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
