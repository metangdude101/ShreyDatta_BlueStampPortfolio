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

The second milestone for my OpenAI Camera was editing the code on the camera to make it more intuitive and add more features. The code that connected my camera to the OpenAI API lacked the ability to change the settings of the camera itself, so after some fiddling, I managed to add those features. The code for the camera is hard to understand, and documentation for it is few and far between. However, I can gladly say I understand it much better than when I began the project. All I have to do now is change or add new prompts to send to OpenAI for the camera, and maybe add another speaker or a battery pack.

# First Milestone

<!-- **Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.** -->

<iframe width="560" height="315" src="https://www.youtube.com/embed/L-g1tkvBFc0?si=UEiRiNsshfH5kpLt" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

The first milestone for my OpenAI Camera was its assembly and installing CircuitPython on it. When I received the parts for the project, I was surprised to see one board and a bunch of unassembled casing. It took me a little while to assemble the case around the board, but it wasn't super hard. However, installing CircuitPython on the board was much harder. Because the Memento board lacks a battery itself, it needs to be charged constantly in order to turn on. For some reason, whenever I plugged the board into my computer to install CircuitPython, my computer wouldn't recognize it as a USB device. After about an hour's worth of fiddling with it, I managed to finally transfer charge and upload CircuitPython to the board at the same time. 

# Schematics 
<img src="adafruit_products_schem (1).png">
<a href="https://learn.adafruit.com/adafruit-memento-camera-board/downloads" target="_blank">Source</a>
<img src="adafruit_products_mementoSchem2.png">
<a href="https://learn.adafruit.com/adafruit-memento-camera-board/downloads" target="_blank">Source</a>
<img src="adafruit_products_cam_schem.png">
<a href="https://learn.adafruit.com/adafruit-ov5640-camera-breakout/downloads" target="_blank">Source</a>


# Code

```python
import board


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

# scale for displaying returned text from OpenAI
text_scale = 1

# OpenAI key and prompts from settings.toml
openai_api_key = os.getenv("OPENAI_API_KEY")
alt_text_prompt = os.getenv("ALT_TEXT_PROMPT")
haiku_prompt = os.getenv("HAIKU_PROMPT")
cable_prompt = os.getenv("CABLE_PROMPT")
pokemon_prompt = os.getenv("POKEMON_PROMPT")
translate_prompt = os.getenv("TRANSLATE_PROMPT")
alien_prompt = os.getenv("ALIEN_PROMPT")
weird_prompt = os.getenv("WEIRD_PROMPT")
define_prompt=os.getenv("DEFINE_PROMPT")

prompts = [alt_text_prompt,
           haiku_prompt,
           define_prompt,
           pokemon_prompt,
           cable_prompt,
           translate_prompt,
           weird_prompt]
num_prompts = len(prompts)
prompt_index = 0
prompt_labels = ["Alt Text", "Haiku", "Define", "Pokedex", "Cable ID","Translate", "Weird"]

flash = False

# encode jpeg to base64 for OpenAI
def encode_image(image_path):
    with open(image_path, 'rb') as image_file:
        image_data = image_file.read()
        base64_encoded_data = binascii.b2a_base64(image_data).decode('utf-8').rstrip()
        return base64_encoded_data

# view returned text on MEMENTO screen
def view_text(the_text):
    rectangle = vectorio.Rectangle(pixel_shader=palette, width=190, height=120, x=25, y=60)
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
              "text": f"{prompt}"
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
wifi.radio.connect(os.getenv('CIRCUITPY_WIFI_SSID'), os.getenv('CIRCUITPY_WIFI_PASSWORD'))
print("Connected to WiFi")
pool = socketpool.SocketPool(wifi.radio)
requests = adafruit_requests.Session(pool, ssl.create_default_context())

palette = displayio.Palette(1)
palette[0] = 0x000000
decoder = JpegDecoder()
# used for showing images from sd card
bitmap = displayio.Bitmap(240, 176, 65535)

pycam = adafruit_pycamera.PyCamera()

rand = random.randint(0,9)
"""
for i in range(0, rand):
    pycam.tone(440, 0.1)
    pycam.tone(880, 0.1)

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
"""
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
    "Max\nLevel 4"
)

current_level = 0
# pycam.led_color = 0  # 0-7  preset colors: 0: white, 1: green, 2: yellow, 3: red,
#                                          4: pink, 5: blue, 6: teal, 7: rainbow
led_colors = (
    "White",
    "Green",
    "Yellow",
    "Red",
    "Pink",
    "Blue",
    "Teal",
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
# sort image files by numeric order

all_images = [
    f"/sd/{filename}"
    for filename in os.listdir("/sd")
    if filename.lower().endswith(".jpg")
    ]
all_images.sort(key=lambda f: int(''.join(filter(str.isdigit, f))))
# add label for selected prompt
rect = vectorio.Rectangle(pixel_shader=palette, width=120, height=20, x=120, y=0)
prompt_txt = label.Label(
            terminalio.FONT, text=prompt_labels[prompt_index], color=0xFFFFFF, x=120, y=10, scale=2
        )
# pylint: disable=protected-access
pycam._botbar.append(rect)
pycam._botbar.append(prompt_txt)
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

pycam.display_message("Flash Off", color=0xFFFFFF)
time.sleep(1)

while True:
    if new_prompt:
        pycam.display_message("SEND?")
    if not view:
        if not new_prompt:
            pycam.blit(pycam.continuous_capture())
    pycam.keys_debounce()
    if pycam.shutter.long_press:
        pycam.autofocus()
    if pycam.shutter.short_count:
        try:
            if flash:
                setattr(pycam, "led_level", 0)
                setattr(pycam, "led_level", current_level)

            pycam.display_message("snap", color=0x00DD00)
            pycam.capture_jpeg()
            pycam.live_preview_mode()
            if flash:
                setattr(pycam, "led_level", 0)
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
        pycam.display_message("OpenAI..", color=0x00DD00)
        send_img(the_image, prompts[prompt_index])
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
                    pycam.display_message("Flash Off", color=0xFFFFFF)
                    time.sleep(0.25)
                else:
                    setattr(pycam, "led_level", 0)
                    current_level = 1
                    pycam.display_message("Flash On", color=0xFFFFFF)
                    time.sleep(0.25)
            else:
                print("getting", key, getattr(pycam, key))
                setattr(pycam, key, getattr(pycam, key) + 1)
                if key == "led_color":
                    current_color = (current_color + 1) % len(led_colors)
                    pycam.display_message(led_colors[current_color], color=corr_colors[current_color])
                    time.sleep(0.25)
                elif key == "led_level":
                    current_level = (current_level + 1) % len(led_levels)
                    pycam.display_message(led_levels[current_level], color=0xFFFFFF)
                    time.sleep(0.25)

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
                    pycam.display_message("Flash Off", color=0xFFFFFF)
                    time.sleep(0.25)
                else:
                    setattr(pycam, "led_level", 0)
                    current_level = 1
                    pycam.display_message("Flash On", color=0xFFFFFF)
                    time.sleep(0.25)
            else:
                setattr(pycam, key, getattr(pycam, key) - 1)
                if key == "led_color":
                    current_color = (current_color - 1) % len(led_colors)
                    pycam.display_message(led_colors[current_color], color=corr_colors[current_color])
                    time.sleep(0.25)
                elif key == "led_level":
                    current_level = (current_level - 1) % len(led_levels)
                    pycam.display_message(led_levels[current_level], color=0xFFFFFF)
                    time.sleep(0.25)


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
