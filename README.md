# Bio-Inspired Multifunctional 3D Printed Armor

**Armor That Can Feel Its Own Damage**

**[Website](https://duckieoverload.github.io/vbr/)** &nbsp;·&nbsp; [Interactive slides](https://duckieoverload.github.io/vbr/presentation/) &nbsp;·&nbsp; [PowerPoint deck](https://duckieoverload.github.io/vbr/Bio-Inspired_Armor_Slides.pptx) &nbsp;·&nbsp; [The research camp](https://duckieoverload.github.io/vbr/program.html)

A team research project by **Emmet Muschenetz**, Raina, Jayden, Jaycen and Peggy (the VBR Mechanical Engineering Team), under the direction of [Dr. Yang Yang](https://yanglab.sdsu.edu/category/people/pi/) of the [SDSU Yang Lab](https://yanglab.sdsu.edu/). Part of the 2026 Victory Bell Education & Research (VBR) Research Camp: three weeks of in-person labs followed by six weeks of paper writing.

## About

Body armor can build up internal micro-cracks from repeated impacts with no visible sign, and finding that damage takes bulky, expensive equipment. This project 3D prints armor lattices in conductive carbon-black PLA (CB-PLA) and TPU (CB-TPU), so the structure itself works as a sensor: as cracks form, its electrical resistance changes.

The designs borrow from two animals built to survive large forces: the lattice skeleton of the deep-sea glass sponge (*Euplectella aspergillum*) and the dactyl club of the mantis shrimp (*Odontodactylus scyllarus*). Across four rounds of compression testing, a glass sponge lattice with 1.0 mm diagonal lines and a +0.5 mm spacing offset gave the best balance of strength and electrical response. It was about 48% stronger than a plain grid of the same size. For the flexible TPU parts, a two-arch mantis shrimp design carried the most load (131.67 N).

At the camp's Final Presentation Day, the team received the **Excellence in Engineering Design Award**.

## Contents

| File | Description |
|------|-------------|
| `index.html` | Project landing page |
| `program.html` | How the VBR Research Camp works: Dr. Yang's lab, a typical day, week by week |
| `site.css` | Shared styles for the two pages, matching the PowerPoint deck (dark greys, SDSU red, Kanit and Open Sans) |
| `images/` | Photos, logos and circuit-trace art used by the HTML pages |
| `presentation/` | Interactive Slidev presentation: a web version of the PowerPoint deck, slide for slide, with charts drawn from `presentation/data/charts.json` (exported from the team's workbook) |
| `Bio-Inspired_Armor_Slides.pptx` | The editable PowerPoint deck (dark greys and SDSU red, native charts from the team's data, the hammer demo video), downloadable from the site |
| `images/svg/` | The piezoresistance, nature-to-CAD and CAD lattice/arch figures as standalone SVG files |

The research paper PDF will be added once the final version is ready. Save it as `Bio-Inspired_3D_Printed_Armor.pdf` in the repo root and switch on its card in `index.html` (instructions are in the comment there).

## Running Locally

```bash
cd presentation
npm install
npm run dev
```

## Live Site

https://duckieoverload.github.io/vbr/ (slides at https://duckieoverload.github.io/vbr/presentation/)

## References

- Fernandes et al., "Mechanically robust lattices inspired by deep-sea glass sponges," *Nature Materials* (2021). [doi:10.1038/s41563-020-0798-1](https://doi.org/10.1038/s41563-020-0798-1)
- Behera & Le Ferrand, "Impact-resistant materials inspired by the mantis shrimp's dactyl club," *Matter* (2021). [doi:10.1016/j.matt.2021.07.012](https://doi.org/10.1016/j.matt.2021.07.012)

## Photo Credits

Photos in the PowerPoint deck and the online slides are from [Unsplash](https://unsplash.com/license): Yassine Ait Tahit (section 1), ZMorph All-in-One 3D Printers (section 2), Quilia (section 4), Victoria Druc (honeycomb), and Nichika Sakurai, Claus Giering and Heidi Bruce (mantis shrimp). Dr. Yang's portrait is from his [SDSU faculty profile](https://mechanical.sdsu.edu/people/yang-yang).

## More from Emmet

- [Emmet's GitHub profile](https://github.com/DuckieOverload)
- [Teaching an AI to Identify Particle Collisions](https://duckieoverload.github.io/map/): transfer learning for jet tagging at the LHC, UC San Diego MAP ([repo](https://github.com/DuckieOverload/map))
