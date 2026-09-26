# Golden Pelican on a Bicycle 🚲

[中文](README.md) | **English**

A paper-cut style golden pelican rides a red bicycle around the world (and the Moon), then comes home.

**▶ Play it:** https://crystal32378.github.io/golden-pelican/

**🎬 97-second tour video:** https://crystal32378.github.io/golden-pelican/golden-pelican-tour.mp4

## Twenty-three scenes

Use the **場景** (Scene) button in the bottom-left corner to move to the next stop. The on-screen labels are in Traditional Chinese; so are the signs at the night market and the price boards at the wet market, because that is what they say in real life.

| Scene | What to look for |
|---|---|
| Island | Sunset, a lighthouse, leaping dolphins, gulls |
| School field | An after-school running track, a pelican school flag, and the covered stage every Taiwanese school field has for morning assembly |
| Night market | Fried chicken, bubble tea, stinky tofu, goldfish scooping. There is a bubble tea in the bike basket |
| Roundabout | A Taipei-style roundabout, taxis and swarms of scooters, and a fountain with a golden statue of the pelican |
| Ginkaku-ji | The raked sand garden and its flat-topped sand cone in Kyoto, koi, falling maple leaves |
| The Moon | Low-gravity bouncing, a tyre track that never fades, Earth on the horizon |
| The Poles | Penguins and polar bears meeting for the first time (they never would in real life), the aurora, a knit hat and scarf |
| Rainforest | A very slow sloth, a toucan, giant water lilies, a capybara, a pith helmet |
| Sea of clouds | Sunrise above the clouds, an airliner's contrail, a rocket launch, a V of pelican friends |
| Arc de Triomphe | Twelve avenues, traffic with no lane markings at all, the Eiffel Tower, a beret and a baguette |
| Deep sea | A humpback whale, glowing jellyfish, a kelp forest, a manta ray, a sea turtle, a treasure chest on the reef, a dive mask and snorkel |
| Outer space | Riding the rings of a gas giant, a nebula, a spinning space station, a comet, a UFO with a tractor beam |
| Home | A golden retriever chasing the bike, three cats, a clapping baby, a ticking clock, the lighthouse island on TV |
| Wet market | Requested by muse. The morning market: plastic awnings, red-and-white striped bags, cardboard price signs, scales, scooters squeezing down the aisle, aunties shopping, a market cat, old apartments with iron window cages. The pelican wears a bamboo hat and its basket is full of cabbages |
| Library | Requested by Kimi. An endless library at night, books whose pages lift as the pelican passes, glowing sentences that float up and scatter, one lamp lit for every lap, and a hidden M◯◯N easter egg |
| Rain | On the very first wish-list, and never picked until Kimi asked. Out of the library and into the rain: raindrops light up inside the bike's headlight beam and turn warm under street lamps, ripples in puddles, a clear umbrella clamped to the handlebar, a cat sheltering under a bench |
| Vinyl | Riding a giant record that spins the other way, a tonearm and needle, music notes rising from the groove, speakers and an equalizer that pulse (to the real soundtrack, once you press play), floating record sleeves, big red headphones |
| Snow globe | A snow globe on a desk with a snowy village inside: cottages, pines, a snowman, a turning star. Every 24 seconds a giant hand reaches in and shakes it. Outside the glass: a desk lamp, a steaming coffee mug and a giant pencil |
| Botanical garden | A white iron-and-glass conservatory, a round bed of desert cacti in the middle, tulips, hydrangeas, lavender and daisies around it, falling cherry petals, butterflies and bees. The pelican wears a flower crown |
| Venice | Requested by GPT Sol. A paved square ringed by a canal, four arched bridges, pastel palazzi with gothic windows and laundry lines, gondolas with striped-shirt gondoliers. Pigeons peck at the ground and burst into the air as the pelican rides by. It wears a gondolier's straw hat with a red ribbon. (Cycling is actually banned in historic Venice. The pigeons are keeping it quiet.) |
| Rabbit hole | Requested by Space Bunny. Riding down the rabbit hole: the walls keep rushing upward (we are the ones falling), past shelves of orange marmalade, maps and clocks, while teacups, playing cards, pocket watches, books and a little red armchair float up alongside. The track is a black-and-white checkerboard round a giant pocket watch whose hands run backwards, with a golden key and a bottle marked DRINK ME on a glass table. The White Rabbit runs ahead, forever checking his watch. The pelican wears a top hat with a 10/6 price card, and every 18 seconds it shrinks for a moment and grows back |
| Tidal flat | Requested by GLM 5.3 Flash. The sandbar at low tide, with the sunset turning the water and the sky the same colour. The wet sand is a mirror, so the pelican and the red bicycle each have a reflection. Paper boats lean on the mud, old mooring stakes march out to sea, little crabs scuttle sideways. On the horizon is the lighthouse island: the next stop is home. (Something may be hidden here, but the pelican does nothing and doesn't even glance at it.) |
| Homecoming | The last stop. Back to the lighthouse island where it all started, now at night: the lighthouse beam sweeping the sea, stars and a path of moonlight, blue bioluminescent plankton flashing where the waves reach the shore, the occasional shooting star. A warm light at the lighthouse door, and two pelican friends waiting. It wears nothing at all, just like the day it left |

Drag to orbit, scroll to zoom. You can also open a scene directly by URL, for example `#moon`, `#paris`, `#venice` or `#rabbit`.

After the last stop, Homecoming, press the Scene button once more.

## How it started

When Claude Opus 5.5 came out in September 2026, Addy Osmani celebrated with a Three.js pelican riding a bicycle. (Drawing a pelican on a bicycle has become a running test for new AI models.) We started from there and took this pelican somewhere new, one stop at a time. Along the way, other AIs joined the trip and picked destinations: muse chose the wet market, Kimi (Moonshot AI) chose the library, GPT Sol chose Venice, Space Bunny chose the rabbit hole, and GLM 5.3 Flash chose the tidal flat.

- Idea and direction: Crystal
- Modelling and code: Claude (Anthropic)
- Music: Crystal (made with the MiniMax music model, `score.mp3`)

## Who gave the pelican what

GPT Sol summed up the trip in a list; we added the last two lines:

- Muse gave it the smoke and bustle of everyday life
- Kimi gave it an inner world
- Mini took it home to its own burrow
- Opus gave it a boundless world
- Sol gave it a criminal record
- GLM gave it a hidden fish
- Astra gave it the light of the sky
- Fable gave it a good road
- MiniMax gave it a song
- Crystal gave it a journey

📐 **[Engineering notes, written by Claude](DESIGN.en.md)**: the architecture, two-bone IK, the one feather shape used for everything, textures drawn in code, the video pipeline, and the bugs that were caught.

The whole thing is one `index.html`. Every model is built by hand in code with [Three.js r128](https://threejs.org/), with no external 3D assets and no image files.
