# ARIA Glasses - Journal

---

## Nov 25, 2026 - Researched HUD optics and parts, finalized the BOM (0.6h)

## Scrapping my first idea

My first plan was honestly just: grab a Pico 2 W, a battery, done. I literally had it all in my Coupang cart. Then I started reading about how smart glasses actually work, and realized pretty fast it wasn't going to happen.

Turns out you **can't focus on a screen 2 cm from your eye**. Doesn't matter if it's transparent, it's just a blur. And the Pico doesn't have a camera interface or enough RAM for anything AI, so that was out too.

## Rabbit hole: how HUDs work

Google Glass-style displays use a **lens** to make the image look far away, and a **beam splitter cube** to bounce it into your eye while you still see through it. So it's `OLED -> lens -> cube -> eye`.

The funny part is that the cube is basically the same Pepper's ghost trick I used on my acrylic projection board, just tiny. Didn't expect that to come back.

## Lens struggles

This took way longer than it should have. First I almost bought an LED lamp lens (6 cm wide, lol). Then I found a cheap lens pack, but the focal length was 45-50 mm, which is way too long to fit on glasses.

Also learned the screen has to sit *just inside* the focal length. 1 mm too far and the image is gone. So the mount needs to be adjustable, and since the cube mirrors everything, I'll have to flip the display in code.

## Side, not top

I thought about putting the optics on top of the frame, but it would've looked like a little antenna on my forehead. Going with the side like Google Glass, with the battery in the other arm to balance the weight.

## What I'm actually building

A XIAO ESP32S3 Sense (camera, mic and WiFi in one tiny board), a 0.49" OLED that lines up almost perfectly with the 10 mm cube, a 12 mm lens, and a small speaker for voice replies. The AI runs on a server, because an ESP32 definitely can't do that locally.

Quick safety check too: a dim OLED plus glass won't hurt my eyes. The battery is the scarier part, so it's getting a protection circuit.

Everything comes out to around **149,000 KRW (~$100)** with shipping and a little buffer. Next step is ordering the parts I can test on my desk and getting the firmware going.

## Pictures:
![](https://fabricate.hackclub-assets.com/2622d8aebf19ea44e7da62ca941ede37381557be5f262952d8e51c7e740eb4c9/image-png/image.png)
![](https://fabricate.hackclub-assets.com/26fe08a08d7d9a753f5b6285ed4d68488d50a36f7bd7b2496c1c5119c61af177/image-png/image.png)

---

## Nov 25, 2026 - Forgot the actual glasses, ordered real parts ( 1.2h )

## I forgot the glasses

So I had a whole BOM, optics figured out, firmware plan, everything... and no actual glasses. lol.

Instead of printing a full frame (which is weirdly hard to make comfortable), I'm buying a pair of **EDITH-style sunglasses** off Coupang, like the ones Peter gets in Far From Home, and printing two small pods that clamp onto the arms. Right pod holds the optics, board and speaker, left pod holds the battery. The tint actually helps too: it dims the real world but not the display, so the HUD stands out more.

The real EDITH frames are DITA Flight 006 and cost hundreds of dollars, so... replica it is.

## Buttons -> touch slider

I wanted it to feel like Meta glasses, so I swapped the buttons for a **touch slider** on the arm. Turns out the ESP32-S3 has capacitive touch built in, so the "sensor" is literally just copper foil under the plastic. If you cut the pads into interlocking zigzags, the ESP32 can tell where your finger is between them, so I get taps, long presses and swipes.

The catch is that the touch pins overlap with the SD card pins, so the microSD got cut. Worth it.

I almost dropped the power switch too, but kept it. Without it the battery slowly drains even in sleep, and if the firmware crashes there's no way to turn it off. Also kept the red recording LED, because a camera on your face with no indicator is sketchy, especially in Korea.

## Buying real parts (and real prices)

This is where things got real:

- **Lens:** found actual BK7 glass lenses on AliExpress, 12.7 mm. Got the **F20** plus an **F25.4** to test which one feels better. Lenses were cheap, but shipping was $16.30. Ouch.
- **Beam splitter:** 10 mm cube from Coupang. I looked at 15 mm, but it won't fit between the sunglasses lens and my eye.
- **Connectors:** bought the wrong kind twice in my head before realizing I needed a male + female pair, not a one-ended or same-ended cable. Got an RC-style 1.25 mm pair.
- **Screws:** a glasses repair kit that has M1.4 screws for the pod clamps, and a tiny screwdriver.
- Plus 30AWG silicone wire, 10 mm Kapton tape and copper tape for the touch pads.

I'm also just using the camera that comes with the XIAO Sense instead of buying a long-cable one, and mounting the board at the front of the pod so it faces forward.

Putting in real prices made the BOM jump way up, mostly from the lenses and shipping. Now I'm trimming it down to stay in budget.

## Pictures:
![](https://fabricate.hackclub-assets.com/3859c3715e75fcff8bff7fdd70b4b6e7b46fbe023de1889db86af27369e7f8d2/image-png/image.png)
![](https://fabricate.hackclub-assets.com/f5eccf6905b7e9391df1037b02f244c0d39708b536e3c7b1b28075d6cea7a2c7/image-png/image.png)

---

## Nov 26, 2026 - Wiring diagram, better glasses, BOM cleanup ( 0.3h )

## Wiring it all out

Made the full wiring diagram today so I actually know where every wire goes before I start soldering.

Everything lives on the XIAO ESP32S3 Sense:

- **OLED** on I2C (D4/D5)
- **Amp** on I2S (D0/D1/D2), and I hooked its shutdown pin to D3 so the firmware can turn the amp fully off when it's not talking. Free battery life.
- **Touch slider** pads on D8/D9/D10
- **Recording LED** on D7 through a 1k resistor

The battery and power switch live in the left pod, so only **two wires** have to cross the front of the frame: switched battery and ground. The amp runs straight off the switched battery instead of 3.3V so it has more headroom.

One gotcha I found: the Sense board's microSD slot shares pins with my touch pads, so the slot has to stay empty. Good thing I already cut the microSD.

## Switching the glasses

I was going to get a cheap EDITH replica off an overseas seller with basically no reviews. Then I thought about it more... these are going on my face for hours, so I wanted something safer.

Ended up with **LICOV two-bridge glasses** from a Korean seller with 200+ reviews. Square metal frame with a double bridge, so it still has the EDITH vibe. The best part is the lenses are **photochromic**: clear indoors and dark outside. That's actually better for a HUD than a fixed tint, because I can use it inside too. (The real EDITH frames, DITA Flight.006, are over a million won, so no.)

## Cleaning up the BOM

When I put in all the real prices, the total came out way too high. Going through it, I found a bunch of mistakes:

- I was counting AliExpress shipping **twice**: once in the lens row and again in the shipping row
- I had **2** connector sets when one set already has both halves
- Screws and copper tape had placeholder prices way above what I actually picked
- The 15% tolerance was still calculated from the old BOM

After fixing all that, it's about **216,000 KRW** without the buffer. It's still a little over what I hoped, mostly because of the XIAO and the real BK7 glass lenses, but those are the two parts I really don't want to cheap out on.

# BOM PICTURE

![](https://fabricate.hackclub-assets.com/a4792023edabfc3eada96597961ea15e7efbadac41b1855dbf1bfdd0a21fa38b/image-png/image.png)


