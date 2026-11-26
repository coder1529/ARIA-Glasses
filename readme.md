# ARIA Glasses

Smart glasses I'm building from scratch. Think Meta Ray-Bans, but open source and small enough to make at home. A tiny display in the right arm floats text in front of your eye, and a camera and mic let you ask questions about whatever you're looking at.

The name comes from ARIA, the LLM I've been training from scratch.

## How it works

Hold a button and ask something. The glasses send your voice (and a photo, if you want) over WiFi to a server, which runs speech-to-text, an LLM, and text-to-speech. The answer shows up on the display and plays through a small speaker by your ear.

## The display

My first idea was a transparent OLED in front of my eye, but you can't focus on anything 2 cm away. So I'm doing what Google Glass did: a lens makes the image look a meter or two away, and a 10 mm beam splitter cube bounces it into your eye while staying see-through. It's basically a tiny Pepper's ghost.

The OLED has to sit just inside the lens's focal length, so the mount slides to adjust focus. The cube also flips the image, so it gets mirrored in code.

## Parts

- Seeed XIAO ESP32S3 Sense: camera, mic, WiFi, battery charging
- 0.49" OLED (64x32): the display
- 10 mm beam splitter cube and 12 mm lens: put the image in front of your eye
- MAX98357A amp and a tiny speaker: voice replies
- Small protected LiPo: power, about an hour of use
- 2 buttons and a red LED: talk, take a photo, recording light

The optics, board and speaker go in the right arm, and the battery goes in the left so it doesn't tilt to one side. The full list with links is in [bom.csv](bom.csv). It comes to about 162,750 KRW (~$110) with shipping.

## Pins

- OLED: D4 (SDA), D5 (SCL)
- Amp: D0 (BCLK), D1 (LRC), D2 (DIN)
- Talk button: D3
- Photo button: D6
- Recording LED: D7

## Safety

It's just a dim OLED and some glass, so it won't hurt your eyes. The battery has a protection circuit, and the red LED is always on when the camera or mic is recording.

Built for [Hack Club Fabricate](https://hackclub.com).

## Wiring Diagram

<img width="2100" height="1410" alt="wiring" src="https://github.com/user-attachments/assets/4ce3d581-b6b1-4cc3-82c1-cbbb820d65b5" />


## AI Usage

I use Claude AI in order to organize the BOM and solve problems like why I can't order or do check out and put into my cart. Also used claude AI to visualize the wiring diagram.
