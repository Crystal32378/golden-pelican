# Engineering notes

[中文](DESIGN.md) | **English**

> Author: Claude (Opus 5.5)
> How the Golden Pelican was built: the decisions, why they were made, and where it still falls short.

---

## 1. Architecture

The whole project is a single `index.html`, about 3,600 lines and 330 KB. Its only dependency is **Three.js r128**, plus `OrbitControls` so you can drag the camera.

**Why one file?** Because the point is to share it. One link, and it runs: nothing to install, bundle or host. The price is maintainability (see section 9).

```
index.html
├─ shared helpers: facet / feather / rod / placeBetween / radialTex
├─ the hero: rider (position, heading) → lean (tilt into the turn) → bike + bird (the pelican)
├─ 21 scenes, each one a THREE.Group:
│    island / school / market / circle / kyoto / moon / polar /
│    jungle / sky / paris / ocean / space / home / wetmarket / library / rain / vinyl / globe / garden / venice / homecoming
├─ ENV: one environment preset per scene (sky, fog, sun, ambient light)
├─ setScene(name): shows the right groups, applies the ENV, puts on the right outfit
└─ pose(t): given a time t, computes everything on screen at that instant
```

### Built on first visit
At first every scene was built when the page loaded. Kimi and muse both pointed out that this is heavy on phones. Now each scene's contents live in a builder function registered in a `LAZY` list, and it runs the first time you switch to that scene. Start-up dropped from 1.2 s to 0.2 s (measured in a headless browser).

### Core rule: the picture depends only on time

Everything that moves (pedals, knees, dolphins, the sloth, the aurora, traffic, comets) takes its position from the time `t` alone. There is no accumulated state.

- **Benefit 1: video.** Recording calls `renderAt(t)` frame by frame, so no frames are dropped when the machine is busy.
- **Benefit 2: switching scenes is safe.** Nothing needs resetting. Leave the Moon halfway round, visit the night market, come back, and the tyre track is exactly where it was.
- **The one exception:** when you drag the camera yourself, following the pelican needs to remember the previous frame.

---

## 2. The hero: a pelican and a bicycle

### Bicycle kinematics
- Wheel radius 0.5; wheel angle = −distance ÷ radius, so the wheels never slip.
- The cranks turn at 0.55× the wheel speed, which works as a gear ratio.
- It rides a circle of radius 6, **counter-clockwise**, because Taiwan and France both drive on the right and roundabouts go anticlockwise. I only noticed it was riding the wrong way at the fourth stop (the roundabout), so thirteen scenes were flipped at once.
- The whole bike leans 0.07 rad into the turn.

### Legs: two-bone IK
The pedals move every frame but the hips stay put. The knee is found with **two-bone inverse kinematics**:

1. Thigh length L1, shin length L2, and the distance d from hip to pedal are known;
2. The law of cosines gives the thigh angle;
3. Of the two solutions, pick the one with the knee in front.

The feet always stay on the pedals, and the knees rise and fall naturally.

### Feathers: one shape for everything
The first pelican was built from smooth ellipsoids, and Crystal said it looked "like a balloon". The second version:

- **Body**: a low-poly sphere deformed vertex by vertex. Narrower toward the tail, flatter underneath, chest pushed forward.
- **Feathers**: one extruded pentagon, a pointed "leaf" (`featherGeo`). `feather()` uses `makeBasis` to set its direction and normal. Coverts, flight feathers, tail and crest are all this one leaf.

It turned out this leaf can be almost anything: penguin flippers, a dolphin's fin, a phoenix's wings, aeroplane wings, kelp blades, maple leaves, morpho butterflies, a manta ray, a dog's ears. Same shape, different colour, size and angle. That is where the paper-cut look comes from.

### Outfits
Each outfit hangs from the head or body node, and `setScene` only toggles visibility:

| Scene | Outfit |
|---|---|
| Night market | Basket + bubble tea |
| Moon, space | Bubble helmet + air tank |
| Poles, snow globe | Knit hat + red scarf (the ends flutter) |
| Rainforest | Pith helmet |
| Sea of clouds | Aviator goggles |
| Arc de Triomphe | Beret + baguette |
| Deep sea | Dive mask + snorkel (it bubbles) |
| Wet market | Bamboo hat + a basket stuffed with cabbages and spring onions + a red-and-white bag |
| Library | Dark green scarf |
| Rain | A clear convenience-store umbrella clamped to the handlebar (it drips) + a headlight |
| Vinyl | Big red headphones |
| Botanical garden | Flower crown |
| Venice | Gondolier's straw hat (red ribbon) |

---

## 3. Shared helpers

| Function | Used for |
|---|---|
| `facet()` | A low-poly icosahedron; scaled, it becomes a body, a rock, a cloud, a treetop, a gold coin |
| `feather()` | The all-purpose feather described above |
| `rod()` / `placeBetween()` | A cylinder between two points: bike frames, legs, branches, railings |
| `radialTex()` | A radial gradient texture drawn in code: sun glow, moonlight, Earth's atmosphere |

Every material uses `flatShading` to keep the low-poly look consistent.

After seeing it, GPT Sol gave the style a name: **paper-cut inspired geometric 3D**. His point was that the piece works not because "low-poly is pretty", but because of a **consistent shape grammar**: the pelican, the waves, the maple leaves, the butterflies and the aeroplane all look as if they were cut from the same box of coloured paper.

Looking back at the code, the rules are right there in that table:

- **One feather for everything.** Pelican feathers, dolphin fins, maple leaves, cherry petals, palm fronds, aeroplane wings, rocket fins and the gondolier's oar blade are all the same `featherGeo`, stretched, squashed and recoloured.
- **One polyhedron for everything.** Bodies, rocks, treetops, clouds, eggs and pigeons are all scaled `facet()`s.
- **No smooth surfaces.** With `flatShading`, every face catches light on its own, so everything reads as pieces fitted together.
- **Flat colour.** Every texture is blocks of colour and lines drawn in code. No photos, no noise textures.

A new scene that keeps these four rules will look like it belongs to the same world.

---

## 4. Every texture is drawn in code

There isn't a single image file. Every texture is drawn at runtime on a `<canvas>`:

- One sky gradient per scene
- The Chinese characters on the fourteen night-market signs
- Haussmann facades in Paris: windows, iron balconies, shopfronts, awnings
- The raked lines of the Kyoto sand garden: straight ripples, plus rings around the sand cone
- The pelican school flag, Earth, the space nebula (2,600 stars), aurora curtains, wooden floorboards, the TV picture
- The TV at home is showing a small version of the lighthouse island from the first scene

**A small trick:** the Paris blocks are wedges made with `ExtrudeGeometry`, whose default UVs flip the texture upside down. So the facade is drawn with the ground floor at the top of the canvas, and it comes out the right way up.

---

## 5. Key techniques, scene by scene

| Scene | Technique |
|---|---|
| Island | The sea is 110×110 vertices, with three sine waves added together every frame. Dolphins jump along a parabola and splash on the way out and the way in |
| School field | The flag waves with vertex animation, swinging more the further it is from the pole |
| Night market | 200-plus bulbs drawn in one `InstancedMesh`; the light strings sag as curves; the camera is kept inside the lane so it never ends up behind a stall |
| Roundabout | Every vehicle follows a route: in along an approach lane → an arc around the ring → out along an exit lane, positioned by interpolating cumulative distance. Scooters travel in threes. The golden statue is made on first visit by cloning the riding pelican (`clone`) and swapping in gold and bronze materials |
| Ginkaku-ji | Forty-six maple leaves spin as they fall and lie flat once they land |
| The Moon | Terrain from a function: the crater rim, smaller craters, distant ridges. The tyre track is 240 segments, each shown once the bike has passed it. Low-gravity bounce uses `\|sin\|^1.6` |
| The Poles | Polar bears move diagonal legs together; the aurora is a 90-segment curved plane reshaped every frame, with additive blending |
| Rainforest | Buttress roots are extruded triangular fins; the sloth moves along its branch with `sin(t·0.06)`, roughly 5 cm a second |
| Sea of clouds | The cloud sea flows every frame; the rocket's path is a polynomial in time that imitates a gravity turn; the smoke column is traced back along its past path |
| Arc de Triomphe | Cars in the twelve-avenue roundabout ignore lanes, drifting in and out of radius; the arch's spandrels are extruded shapes cut out with `absarc` |
| Deep sea | Kelp is 8 to 11 nested groups, each swaying a little later than the one below, so it looks like it's moving with the current; jellyfish tentacles are line segments updated every frame |
| Outer space | 1,600 ice rocks in one `InstancedMesh`; the ring's UVs are remapped by radius so the stripes are concentric |
| Home | The golden retriever uses the same path formula, 2.7 units behind the bike; the baby's head uses `atan2` to keep facing the pelican |
| Wet market | Requested by muse. Price signs are written in code in a hand-lettered style; scooters ride the aisle against the pelican; the iron window cages, air-con units and laundry of the old apartments are all painted onto one facade texture |
| Library | Scene brief written by Kimi (Moonshot AI). About seven thousand books in one `InstancedMesh`. The glowing sentences are drawn on a canvas, sampled point by point into `Points`, then drift up and scatter. One lamp lights per lap: lamps lit = `floor(laps) % 13`. Open books on the floor lift their pages based on the pelican's angular distance. There is also a hidden M◯◯N easter egg. Kimi also caught a bug: editing the `#` in the URL by hand didn't change scene; a `hashchange` listener fixed it |
| Rain | This stop was on the list of next destinations I suggested after the third scene. Nobody picked it until Kimi asked. 3,200 raindrops in an `InstancedMesh`, each one's position and colour recomputed every frame: bright white inside the headlight's `SpotLight` cone, warm yellow under a street lamp, dim blue-grey everywhere else. Plus six street-lamp cones, spreading ripples in puddles, warm light from the library door, and an orange cat under a bench |
| Vinyl | The pelican rides a giant record spinning the other way, like a treadmill. Grooves and label are drawn in canvas. The rainbow sheen is a `createConicGradient` layer that does not rotate, so it stays put while the record turns, like a real reflection. **When you press play**, a Web Audio `AnalyserNode` reads the soundtrack: the low end pushes the speaker cones and 64 equalizer bars follow the spectrum. Without music, a synthetic 96 BPM beat takes over, which also keeps recordings deterministic |
| Snow globe | The glass is a translucent sphere with its bottom cut off by the base; two curved highlights are what make it read as a sphere. All 1,800 flakes are functions of time: every 24 seconds a hand reaches in and shakes it, the flakes are thrown up to their own heights within 0.8 s, swirl around the centre (the swirl decays exponentially), fall at their own speeds and settle. Each flake's radius is clamped by its height so it stays inside the glass. During a shake the whole globe, village included, wobbles slightly while the pelican keeps pedalling |
| Botanical garden | The conservatory is a hemisphere of glass squashed to 0.78× height, with 24 white iron ribs along the meridians (`TubeGeometry` along a curve) and 4 rings of latitude. Four kinds of cactus: saguaros are cylinders with right-angled arms, golden barrels are spheres with 12 spines, prickly pears are squashed spheres, and agaves are the all-purpose feather arranged as a rosette. Eight beds around the outside take turns with tulips, hydrangeas, lavender and daisies; cherry petals reuse the falling-leaf code from Kyoto |
| Venice | Requested by GPT Sol. To be honest: cycling is banned in historic Venice, so this pelican is breaking the rules, and the pigeons are keeping it quiet. A paved square (campo) in the middle, ringed by a canal whose surface moves with vertex waves; four arched bridges cross it. 22 pastel palazzi, with gothic windows, green shutters and window boxes painted in canvas. Four gondolas circle the canal, their gondoliers in striped shirts and straw hats, rowing. Thirty pigeons peck at the ground; when the pelican rides close (angular distance under 0.55, within 2.6 of the lane) they flap up into the air, then settle back once it has passed. A brick bell tower stands in the distance |
| Homecoming | The last stop. It reuses the island and sea from the first scene with a night-time ENV, so it really is the same island. The lighthouse's two beams are open cones with a gradient that fades away from the lamp. The blue bioluminescent plankton are 2,600 additively blended points, recoloured every frame by wave fronts rolling toward the shore, brightest near the sand. Warm light at the lighthouse door, two pelican friends waiting, one of them waving a wing. The pelican wears nothing at all, just as it did at the start |

---

## 6. Camera

- **Auto camera**: the camera orbits in the pelican's own frame of reference, with distance and height drifting on sine waves of different periods, so no two stretches look the same.
- **Night market, wet market, library**: the camera slides along the arc of the aisle instead, a fixed 4.2 units ahead of or behind the pelican, with the radius clamped between 5.2 and 6.9 so it never passes through a stall or a shelf.
- **Manual mode**: as soon as you drag, OrbitControls takes over, and the camera still moves along with the pelican.

---

## 7. Recording the video

1. Load the page in headless Chromium (SwiftShader software rendering) with `__CAPTURE = true`;
2. Call `renderAt(t)` frame by frame and export each as JPEG with `toDataURL`;
3. Two processes render in parallel: 97.5 s × 30 fps = 2,925 frames;
4. Assemble with ffmpeg: fade to dark and back between scenes, add Chinese and English scene titles with `drawtext` in Noto Sans CJK, and lay Crystal's music on top;
5. Also encode an 18 MB version to fit under Discord's 20 MB limit for free accounts.

---

## 8. How it was checked, and what was caught

After every scene, at least two screenshots: one wide shot and one from the follow camera. Things they actually caught:

- Washed-out colours: the output was sRGB but the material colours weren't converted, so the sRGB output was removed.
- On the Moon and in space, the pelican went black when backlit, so the ambient light was raised.
- The night-market follow camera went behind the stalls, so it was restricted to the lane.
- The roundabout scene crashed on load: the Paris code ran before the vehicle functions were defined, so the code was reordered. The wet market later hit the same problem. That is the price of one unmodularised file.
- The golden statue looked dark: too metallic, with no environment map to reflect, so metalness went down and a little emissive went in.
- Flickering stripes on snowy peaks: the snow caps overlapped the mountain surface exactly (z-fighting), so the caps were made slightly larger.
- The aurora's edges looked cut off, so it now fades out horizontally.
- The dog followed too closely and its head ended up in the pelican's tail, so it was moved back.
- Venice: a café umbrella blocked the follow camera, so the umbrellas are now closed; and the pigeons were nearly the size of the pelican's head, so they were scaled down.

---

## 9. Known weaknesses

- **Three.js r128 is old.** It was chosen because it still ships a UMD build that loads with a plain `<script>` tag.
- **Everything in one file, no modules.** Great for sharing, not for long-term maintenance. Better: one module per scene, with the helpers and the hero pulled out.
- **Mobile performance.** Scenes are now built on first visit, but once visited they stay in memory. The rain updates over three thousand drops per frame, which may still stutter on older phones. Shadow resolution and particle counts could be lowered per device.
- **No collision detection.** Cars don't hit the pelican because everyone stays in their own lane, not because of any real physics.

---

## 10. Who did what

- **Crystal**: where each stop goes, what the pelican wears, whether the penguins and polar bears get to meet. Her feedback ("it looks like a balloon", "it has a little crown on its head") directly shaped how the pelican looks. She also made the music.
- **Claude**: all the code, modelling, textures, animation, video and these notes.

The whole thing is completely useless, and making it was a joy.

It left from a small island, rode through nineteen places, and came back to the same island. This time it was night, the lighthouse was lit, and someone was waiting at the door.
