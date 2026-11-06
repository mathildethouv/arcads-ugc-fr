---
name: arcads-talking-head-fr
description: "Version française : la personne parle français (accent de France). Make a realistic AI talking-head video (one person talking to their phone: selfie rants, storytimes, tips, car/sofa/desk confessionals) with Seedance 2.5 on the Arcads MCP, using the Arcads Talking Head method: a real Reference Video for delivery, a start frame (new character, new location, same angle as the Reference Video) or a Character Reference, a script written to the Reference Video's pace, and a prompt in the proven block structure. Arcads MCP only, no Topaz or other tools. Use whenever someone wants a talking head, a UGC talking video, 'make this person talk about X', 'turn this script into a talking head', or drops a TikTok/Reel of someone talking and wants a version with a new person, topic or product."
---

# Arcads Talking Head · en français

**Langue : tout ce qui est dit dans la vidéo est en français de France, naturel et parlé.** Le script s'écrit en français, la voix est castée en française, et les blocs du prompt Seedance restent en anglais (le modèle les suit mieux), sauf la réplique, qui est en français mot pour mot. La vidéo de référence peut être dans n'importe quelle langue : on ne lui emprunte que la façon de parler et de bouger.

One person talking to their phone, 10–16 seconds typical, up to 30. One take or a few jump cuts between sentences. Everything runs on the **Arcads MCP**: GPT Image 2.5 for the image, Seedance 2.5 for the video. No upscaling, grading or captions.

The method in one line: **Reference Video + (Start frame or Character Reference) + script at the Reference Video's pace + block prompt → Seedance 2.5.**

## Requirements
- **The Arcads connector (MCP)**, connected in Claude. Every step runs through it: analysis, transcription, frame extraction, GPT Image 2.5 and Seedance 2.5.
- **An Arcads account with credits.** A 15s 1080p Seedance 2.5 video and one GPT Image 2.5 image are the main costs.
- Nothing else: no other connectors, no local software, no upscaler. If the Arcads tools aren't available, stop and ask the user to connect the Arcads connector.

## What to ask for (one message, only what's missing)
1. **Reference Video (required).** A real TikTok/Reel of someone talking the way the video should feel: fast and fluent, worked up, calm. Ask for a file. If they have none, ask them to find one; don't skip this, it is what makes the delivery feel real.
2. **Person.** Either a **Character Reference** (a portrait of the person to use) or a short description of an invented person.
3. **Script or topic.** Either their own script **in French**, or a topic/product and a tone ("parle de nos gummies pour dormir, ton léger"). Write or keep the script in French.
4. **Length.** Default 15s. Seedance 2.5 allows 4–30s.
5. **Product (optional).** A clean packshot if a product should appear.

## Working with the Arcads MCP
- Read each tool's schema before the first call. Tools used: `arcads_analyze_media`, `arcads_transcribe`, `arcads_probe_media`, `arcads_extract_frame`, `arcads_generate_image_gpt`, `arcads_generate_video_seedance_25`, `arcads_get_asset`, plus `arcads_upload_media` for the user's own files.
- If the workspace has several products/brands, pass `productId` on every call (ask which one, or use the one they name).
- **Pass references as Arcads asset ids** (from `arcads_upload_media`, `arcads_extract_frame` or an earlier generation). Local file paths in generation calls are unreliable.
- Show every image and video to the user as soon as it lands.

## Step 1 · Read the Reference Video
1. Upload it and get its duration with `arcads_probe_media`.
2. `arcads_analyze_media`: ask for the camera (propped or handheld, angle, distance, movement), the framing, the light, how the person talks (pace, energy, pauses), their hand gestures and habits, and where any jump cuts fall.
3. `arcads_transcribe` with `format: "srt"`. Count the words and the seconds of actual speech (first cue start to last cue end). **Reference pace = words ÷ speaking seconds.**

Tell the user in two or three lines what the Reference Video will lend (delivery, gestures, camera) and its pace.

## Step 2 · Make the image (pick one path)

### Path A · Start frame: new character, new location, same angle
The start frame is the exact first frame of the video. It copies only the Reference Video's **camera** (selfie angle, distance, lens, handheld or propped) and puts a new person in a new location.
1. `arcads_extract_frame` at about 1s (`frame: "select"`, `framePosition: 1`), a moment with no captions over the face.
2. `arcads_generate_image_gpt`, `model: "gpt-image-2-5-flare"`, `aspectRatio: "9:16"`, the frame as the reference image (add the Character Reference as image 2 if they gave one). Prompt in these blocks:

```
SCENE: <the new location, concrete: furniture, walls, objects, time of day>
PERSON: <age, look, hair, outfit; or "the person in image 2: take only their face, hair, skin and build">. <pose caught mid-sentence, not posing, not smiling for the camera>
CAMERA: Use image 1 only for the camera. The same <front-camera selfie at arm's length / propped phone> <angle>, the same distance and the same lens distortion. <where the face sits in frame>. Vertical 9:16. Do not copy the person, the room, the clothes or any caption from image 1.
LIGHTING: <one source, its position and colour temperature, what it does to the face: lit side, shadow side, catch-lights>. Every highlight and shadow points to that source.
REALISM: A real iPhone front-camera photo, not a render. Real, unretouched skin with visible pores and uneven tone. Mild digital noise, slightly soft focus, everyday auto white balance. Not smoothed, not airbrushed, not CGI, no cinematic grading.
KEEP: Keep the exact camera angle, distance and lens distortion of image 1. Everything else is new. One person only. No text, no captions, no watermarks.
```
3. **Gate 1:** show the start frame and wait for an OK or changes.

### Path B · Character Reference only
No image step. The portrait goes into Seedance as `@image1` for identity only. The Reference Video sets the delivery (and the camera, if you say so); the prompt must describe the clothes (WARDROBE) and the room (LOCATION MAP). Quicker, with less control over the first frame. **Gate 1:** confirm the portrait is the one to use.

## Step 3 · The script
**Target words = seconds × reference pace × 0.85.** If the Reference Video is not in French, use 3.2 words/sec as the French pace for a calm delivery and 3.8 for a fast one, instead of the Reference Video's pace. The 0.85 leaves room for gestures, glances and breaths. Worked example: a Reference Video with 51 words in 10.6s of speech is 4.8 words/sec; a 16s video then fits 16 × 4.8 × 0.85 ≈ 65 words.

- **They gave a script:** keep their words. Count them against the target. If it's more than ~10% over or under, say so and offer either a trim (show the trimmed version) or a new length (e.g. "72 words fits 19s at this pace"). Never rewrite without a yes.
- **They gave a topic:** write it in French. The first line is the hook. Spoken, casual French ("t'as", "franchement", "genre" when it fits), sentences that run into each other, no hashtags or emojis. Write numbers, brands and English words the way they are said out loud, so the voice says them correctly. Land the last word on a strong beat; the video ends on it.
- Split the script into timed lines (`0.0s to 4.3s - "…"`). Put any jump cuts in the gaps between sentences.

**Gate 2:** show the final script with its word count, pace and length. Wait for an OK.

## Step 4 · The Seedance prompt
Plain, concrete sentences in these blocks, in this order. Leave out blocks that don't apply.

```
References: @image1 = <Start Frame.png | Character Reference.png> · [@image2 = <Product.png> ·] @video1 = Reference Video.mp4

SCENE CONTEXT
<Who, where, what they're doing, and the length.> A <fourteen>-second talking-head video in the real style of a creator's own post: <one locked phone angle | her handheld selfie angle>, <one continuous take | a quick jump cut at the start of a new sentence>.

ACTIVE REFERENCES
Path A: @image1 is the START FRAME: the very first frame of the video is @image1 exactly, the same <person>, the same <room>, the same framing and light, and the video plays on from that exact frame; <she> stays identical to @image1 in every frame. 100% matches the reference.
Path B: @image1: this <woman>. Her face, hair, skin and colouring come entirely from the reference and stay identical to it the whole time. 100% matches the reference.
[@image2: <the product in words: shape, material, colours, the exact wordmark and small print>. 100% matches the reference. The lettering stays sharp and correctly spelled.]
@video1: a different <woman>, used only for HOW she talks and moves: <the delivery, rhythm and the specific gestures from the analysis>[, and the camera angle and framing]. Her face, hair, voice, words, clothes and room are not used.

WARDROBE            (path B; or when the outfit must be exact)
<one line>

LOCATION MAP        (path B)
<foreground, midground, background, where the phone sits>

FORMAT MODE
<One continuous <N>-second take | Jump cuts only at <t1>s and <t2>s, each in the gap between two sentences; each sentence is one unbroken take>. The camera does not cut on its own.

CAMERA
<propped/handheld, height, angle, movement>, everyday auto-exposure, fine sensor noise.

ACTION
<She talks the whole time…>
0.0s to <t>s - "<line>" <the gesture or beat on that line>
<t>s JUMP CUT        (if used)
…
<last line>. The video ends on the word "<last word>".

PERFORMANCE
<Who they are, talking to a friend.> Two truths: <e.g. she is blunt and she is warm>. <Gestures the way @video1's hands do.> Eyes alive: glances up while she thinks, back to the lens on the point, natural blinks. Real skin at pore level, dry matte skin with a soft natural finish.

[PHYSICS            (only when an object moves: weight, condensation, a can's crack)]

LIGHTING
<source, direction, colour temperature, effect on the face>, exactly as in @image1, the same for the whole video.

AUDIO
<room sound>. Her voice close on the phone mic. She speaks in natural, fluent French from France, with a native Parisian accent, never an English accent. She says exactly, in French, and nothing else: "<full script in French>"
Her voice: "A <age>-year-old French <woman> from <Paris | Lyon | Bordeaux…>, <voice qualities>, <how she's talking>, native French speaker."

STYLE
Photoreal phone video, everyday auto-exposure, fine sensor noise, real time, the look of something posted straight from the phone.

POSITIVE LOCKS
One <woman> in frame, identical to @image1 throughout[, and the first frame is @image1 exactly]. <Cuts only at …>. She speaks from the first frame to the last and the video ends on her last word. Her skin stays dry and matte. Every spoken word is in natural native French from France, with a French accent and French pronunciation.
```

### Rules that make it work
- After every reference description: **"100% matches the reference."**
- Fence off the Reference Video by name: face, hair, voice, words, clothes, room, on-screen text.
- Name the habits to copy from the Reference Video, gesture by gesture.
- **Start mid-action:** frame one is already mid-sentence, never a neutral pose.
- **"She says exactly, and nothing else:"** followed by the full script.
- Cast the voice: age, French city, how it sounds, and say "native French speaker".
- Cut only between sentences; never mid-word.
- End cleanly on the last word.
- Positive locks say what should happen, not what shouldn't ("both hands free", not "no props").

## Step 5 · Generate
`arcads_generate_video_seedance_25` with:
- `prompt`: the block prompt
- `referenceImages`: [start frame or Character Reference asset id, then the product if any], in @image order
- `referenceVideos`: [Reference Video asset id] (at least 2s; ~30s max combined)
- `aspectRatio: "9:16"`, `resolution: "1080p"`, `duration`: the script length (4–30), `audioEnabled: true`, `nbGenerations: 1`
- `title`: e.g. `ruby_sofa_talkinghead_v1`

It takes about 7 minutes. Fetch the result with `arcads_get_asset` and give the user the video and its download link. Don't re-roll on your own; if they want changes, adjust the prompt and run again.

## Example (real prompt, path B · Character Reference only)
```
References: @image1 = Model.png · @video1 = Reference Video.mp4

SCENE CONTEXT
A nineteen-year-old guy sits in his living room and makes a quick talking video for his followers about how he bought his first car on his own, straight to his propped phone. A sixteen-second take with three quick jump cuts, the real style of a creator's own post.

ACTIVE REFERENCES
@image1: this young man. His face, hair, skin and colouring come entirely from the reference and stay identical to it the whole time. The man on screen is always the man in @image1. 100% matches the reference.
@video1: a different man with a different face, used only for HOW he talks and moves: very fast, energetic, straight-talking delivery, a small clip-on mic with a fuzzy black windscreen held at his chin in one hand while the other hand talks, the camera angle and framing. His face, hair, cap, tattoos, voice, words, clothes, room and on-screen text are not used.

WARDROBE
The washed-black hoodie and thin silver chain from @image1.

LOCATION MAP
A bright apartment living room: he sits on a grey couch, a white-painted brick wall behind him with a tall leafy plant in the corner and a framed poster, a window on camera-left letting in soft daylight.

CAMERA
Propped and locked at eye level, framing him from the chest up, sitting slightly off centre. The same camera and framing for the whole video.

FORMAT MODE
Four segments joined by jump cuts at 6.6s, 9.3s and 11.2s, one right before each of the three things he lists: same camera, same framing, his head and shoulders shifting slightly at each cut. Cuts only at these points, the camera does not cut on its own.

ACTION
0.0s to 16.0s - Frame one: he is already mid-sentence, holding the small fuzzy clip-on mic just under his chin in his left hand, his right hand already moving: "I bought my first car at nineteen with zero help from my parents and the number one question I get is how. Honestly it's three things. I worked weekends at a coffee shop and saved every single paycheck. I sold my old gaming PC and my sneakers. And I bought a ten-year-old used car instead of something cool. That's it. Boring car but it's mine." JUMP CUT at 6.6s right before "I worked weekends", JUMP CUT at 9.3s right before "I sold my old gaming PC", JUMP CUT at 11.2s right before "And I bought". A proud little grin on "it's mine". The video ends on that word.
```
(The Reference Video had 51 words in 10.6s = 4.8 words/sec; 16 × 4.8 × 0.85 ≈ 65; the script is 67 words.)
