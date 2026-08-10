# Carbon — One Element, Many Possibilities

An interactive exhibition about carbon, built as a single self-contained HTML file.

You begin with one carbon atom in an empty room. From there you choose what it
becomes: let it disperse into a gas, let the atoms find each other, flatten them
into a plane, curl that plane into a nanotube, close it into a cage, or push the
bonding into three dimensions and grow a diamond. Eleven states, each reachable
from the others, none of them an ending.

**[Live demo →](https://YOUR-USERNAME.github.io/carbon/)**

![Carbon](preview.png)

---

## Everything is generated at runtime

There are no models, no textures, and no image assets in this project. Every
structure is built from its own crystallography when you arrive at it:

| State | How it is generated |
| --- | --- |
| **Atom** | A single sphere, with a Fresnel rim shell |
| **Electron density** | A raymarched sp³ density field — core, valence shell, four tetrahedral lobes |
| **Gas** | Brownian drift as a sum of slow sines on a temperature-scaled time axis |
| **Chains** | Rings, zig-zag chains and a tetrahedral branch point, assembled from a dispersed gas |
| **Graphene** | 2,287 atoms on a hexagonal lattice; every interior atom exactly 3-coordinated, all bonds exactly equal |
| **Graphite** | Five AB-stacked sheets, interlayer tethers drawn only between atoms the stacking actually eclipses |
| **Nanotube** | The same lattice bent by one continuous parameter, from infinite bend radius to exactly one circumference |
| **Fullerene** | Derived from icosahedron edge-thirds: 60 vertices at identical radius, 90 bonds of identical length |
| **Diamond lattice** | Diamond-cubic network, interior coordination exactly 4, bond angle 109.47° |
| **Diamond** | A round brilliant cut from real proportions — facet planes intersected, not modelled |
| **Amorphous carbon** | Random atoms relaxed until no two sit closer than a bond length, then bonded nearest-first with a cap of four |

A few things that were verified numerically rather than by eye:

- **Graphene** — bond lengths identical to five decimal places, interior coordination exactly 3.
- **C60** — all 60 vertices equidistant from the centre to within 1e-11, all 90 bonds identical, 12 pentagons and 20 hexagons resolving with zero mis-ordered edges.
- **Diamond lattice** — bond angle 109.47°, interior coordination exactly 4.
- **Brilliant cut** — watertight at every slider position, all normals outward, default proportions landing on the GIA Excellent band (63.5% depth, 35.1° crown, 41.8° pavilion).
- **Amorphous carbon** — nearest-neighbour distances converge to 0.360 ± 0.002; zero unbonded atoms at any density.

## Two shaders worth a look

**The electron cloud** is a 44-step raymarch through an analytic density function —
a tight 1s core, a valence shell, and four tetrahedral lobes that foreshadow the
sp³ bonding you meet later in the diamond lattice.

**The gemstone** traces the path light actually takes through a brilliant: it
refracts in at the crown, totally internally reflects off two opposing pavilion
facets, and leaves through the top. The pavilion is a cone of revolution, so the
facet a ray meets is found from the ray's own azimuth — no geometry lookup. Fire
comes from running that path three times at 2.417 ± dispersion for red, green and
blue. A `uReturn` term peaks at the ideal cut depth and falls off as a Gaussian,
so sliding away from a good cut drains the fire and the stone goes glassy — which
is exactly what light leakage does to a badly cut diamond.

## How to use it

- **Choose a transformation** from the possibilities placed around the structure.
  Arrow keys work too, in the direction the possibility sits.
- **Drag** to turn the structure, **scroll or pinch** to move closer.
- **The slider** beneath the structure changes one physical property of the
  current state — temperature, interlayer spacing, tube diameter, cut depth.
- **The map** on the right is orientation, not navigation. It shows where you are
  and what is reachable; only states you have already visited can be clicked.

## Running it

It is one file. Open `index.html` in a browser, or serve the folder:

```bash
python3 -m http.server 8000
```

Three.js r128 loads from jsDelivr, with a vendored copy in `vendor/` as a
fallback if the CDN is unavailable.

## Deploying to GitHub Pages

1. Push this folder to a repository.
2. **Settings → Pages → Source: Deploy from a branch**, then pick `main` and `/ (root)`.
3. Add a screenshot named `preview.png` at the root so link previews render, and
   update the demo URL at the top of this file.

`.nojekyll` is included so Pages serves the files as-is.

## Performance

One renderer, one animation loop, one shared sphere geometry. Every lattice is
drawn with instanced meshes — graphene is 2,287 atoms and 3,360 bonds in two draw
calls. The graphite filament net is built once and shared by all five layers.
Camera and structure motion run on critically damped springs, verified to settle
without overshoot.

## License

MIT — see [LICENSE](LICENSE).
