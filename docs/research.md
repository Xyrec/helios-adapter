# Viability Assessment: An Open-Source, Rangefinder-Coupled Helios 44-2 → Leica M Conversion

## TL;DR

- **Feasible, with caveats.** An open-source, machinable, rangefinder-coupled Helios 44-2 → Leica M conversion is technically achievable: every dimensional and engineering input needed to scaffold CAD either exists publicly or is directly measurable from cheap, abundant donor parts (Helios 44-2 lenses sell for roughly $20–200 on the used market; Leica M flange specs and RF geometry are documented). The single hardest problem — the 58mm-vs-51.6mm cam translation — is solved math and has already been solved mechanically by several existing products.
- **It has been done commercially but never published as open source.** Camera Obscura Elburg (2025, 10 units, €950), Omnar/Skyllaney, Funleader/Mr. Ding, and the Shoten R50 all RF-couple non-M lenses to Leica M, but none release CAD, drawings, or cam profiles. There is currently **no open-source RF-coupled adapter/lens design in existence** — this project would be the first.
- **The first concrete step is metrology, not CAD:** measure a donor Helios optical block and an M-mount body's roller travel, adopt the ~1.27× cam gear-ratio math below, and prototype the "adapter-with-its-own-helicoid, lens-locked-at-infinity" approach (the Shoten R50 concept, re-cut for 58mm) as the lowest-risk open-source path.

## Key Findings

**1. The core geometry is favorable.** The Helios 44-2 is an M42 lens (flange focal distance 45.46mm); the Leica M flange focal distance is 27.8mm. That leaves roughly 17.7mm of axial space between the M bayonet seating plane and where the M42 lens's reference plane must sit — generous room to house a new bayonet, an RF cam mechanism, and a focusing helicoid. The Helios optical block unscrews cleanly from its native helicoid on a fine thread (a well-documented DIY operation), so a converter can either keep or discard the Soviet helicoid. _(Note: the M flange figure itself carries a small documented ambiguity — see Caveats — which is one more reason to verify by measurement before machining.)_

**2. The 51.6mm problem is real but fully quantified.** The Leica M rangefinder is mechanically calibrated so that the roller (cam follower) moves 1:1 with the focusing extension of a _nominal 50mm / true ~51.6mm_ lens. This 1:1 relationship was established by the 1932 Leitz coupled rangefinder and carried into the M3 in 1954 (per forum contributor Michael Geschlecht on l-camera-forum: "the focussing helical to roller movement is designed to operate at a ratio of 1:1 with a 50mm lens"). A 58mm lens extends farther per unit focus distance. Using the thin-lens extension formula e = f²/(d−f):

- **51.6mm lens:** 2.81mm extension at 1.0m; 4.11mm at 0.7m
- **58mm lens:** 3.57mm extension at 1.0m; 5.24mm at 0.7m
- **Ratio (58mm ÷ 51.6mm) ≈ 1.27**, nearly constant across the coupled range (1.272 at 1m, 1.276 at 0.7m; limiting close-focus ratio (58/51.6)² = 1.263).

So the Helios helicoid moves ~1.27× too far; the design must gear this down by a factor of ~0.787 so the roller sees standard travel. Because the ratio drifts ~0.3% between 1m and 0.7m, a _linearly sloped cam_ alone leaves a small residual error; a profiled cam or a properly pitched secondary helicoid tracks exactly. The independent forum estimate that "the Leica is spec'd by a 3 mm travel of the roller from infinity to 1m" cross-checks the 51.6mm calculation (forum member "giordano" derives ~2.83mm at 1m for a 51.8mm lens), validating the method.

**3. Three proven engineering approaches exist, each demonstrated by a shipping product.**

- **(a) Full rehousing** (Omnar/Skyllaney): the optical block goes into an all-new brass helicoid plus a machined RF cam tuned to the lens's true effective focal length (EFL). Highest quality, hardest to DIY, most parts to machine.
- **(b) Barrel extension + linkage to the existing helicoid** (Camera Obscura Elburg's Helios-M): reuses the Helios's own helicoid, adds internal components that let the RF roller read that motion, plus a click-stop that signals RF decoupling. Retains native 0.5m close focus.
- **(c) Adapter with its own focusing helicoid, lens locked at infinity** (Shoten R50, Funleader Contax kits): the lens is set to infinity and never moved; all focusing happens in the adapter's helicoid, which carries the RF cam. This is the **most open-source-friendly** approach because the cam-to-helicoid relationship lives entirely in one user-fabricated part and is independent of the donor lens's worn helicoid.

**4. RF coupling range is body-dependent and must be designed around ~0.7m.** Most Leica M bodies couple from infinity to 0.7m; the original M3 was designed for a 1.0m minimum; some third-party and older bodies effectively lose coupling around 0.9m with short-lever lenses (a documented issue where a non-Leica lens's focus lever is shorter than Leica's). The Helios focuses natively to 0.5m — below the RF-coupled range. The proven solution (Camera Obscura) is a click-stop/decoupling detent at 0.7m: RF-coupled infinity→0.7m, then a tactile click into an uncoupled 0.7→0.5m close-focus zone. Since these users' bodies have no live view, that below-0.7m zone is scale/estimation focus only and should be clearly engraved.

**5. No open-source precedent for RF coupling specifically, but strong open-hardware precedents for the surrounding practices.** OpenReflex (3D-printed SLR, CC BY-SA), the Archive-663/lensMounts repo (STL + STEP mount models including a Leica M folder, released for reuse), the Mamiya-Press MRF rangefinder on Printables (STL + KiCAD/Gerber + firmware on GitHub), and Lensfun (LGPL library + CC BY-SA database) collectively establish the licensing, file-format, and documentation conventions this project should adopt.

## Details

### Existing solutions — commercial and DIY, but nothing open

- **Camera Obscura Elburg "Helios-M 58/2" (Netherlands, late 2025).** Limited edition of 10 units, €950, each numbered with a certificate showing edition number and original Helios serial. Their published process (cameraobscuraelburg.nl): a prototype "to correct the flange focal distance and add basic rangefinder coupling," then "a new barrel extension in CAD" plus "several internal components to enable interaction between the rangefinder and the existing helicoid," retaining close focus, with "a small click-stop... so you can feel when the lens is no longer rangefinder-coupled." They also converted the aperture to click stops and coated internals with Black 3.0. **No CAD, drawings, or cam data are published** — it is a commercial product description only.
- **Omnar Lenses / Skyllaney Opto-Mechanics (UK).** A one-off conversion service (from 2022, starting around £1,249 / ~$1,600 per DPReview) that rehouses non-RF optical blocks of 8mm–52.4mm EFL into an all-brass "Omnar platform" with a machined RF cam, coupling from 0.67m to infinity. Skyllaney documents the engineering logic explicitly: they reverse-engineer the true EFL of the optical block, because (verbatim) "One size fits all RF cam slopes for 35mm, 50mm lenses are not possible, as even a minor 0.1mm EFL deviation between the RF cam and the optical formula will cause focus accuracy issues." Their conversion list includes a "58mm Biotar" (the Helios's optical ancestor) and a "40mm Biotar." Their stated tuning levers: adjust the cam slope, change the secondary helicoid thread pitch, or modify the optical EFL. **Proprietary; no files released.**
- **Funleader / Mr. Ding Contax G conversion kits (G35/G45/G28).** Non-destructive: the user transfers the original optical block into a purpose-built brass/aluminum RF-coupled helicoid, coupling to ~0.7m. Notably these are **sold as user-installable kits** — the closest existing analogue to a distributable design — but the helicoid itself is a finished manufactured part, not open CAD. Calibration is via stacked gaskets/shims with a documented infinity-check procedure ("if the image is clear at ∞, do not add the gasket; if not clear at ∞, reduce the gasket").
- **Shoten R50 (Focus Studio, Japan, 2023).** A helicoid adapter (M42-LM and PK-LM versions) that RF-couples _any 50mm lens_ to Leica M from 0.7m to infinity, at a suggested retail price of ¥22,500 (≈ $160, per PetaPixel, July 12 2023). The user sets the lens to infinity and focuses with the adapter's own tab; a jeweler's screwdriver adjusts the RF linkage for infinity calibration. **Critically, it is focal-length-specific:** the JE Labs hands-on review (Sept 2023) states it is "designed to take a 50mm (Note: a 35mm, 45mm, 55mm or 58mm, etc. won't focus accurately)." This directly confirms that a Helios-specific (58mm) version needs its own cam/helicoid geometry — which is exactly what this project would compute.
- **DIY community:** Individual hobbyists have hand-made RF cams (one RangefinderForum user "made the RF cam out of an old Tripod Leg" for a Minolta 50mm on an M8), and a Ukrainian specialist ("Roman") recalibrates Soviet Jupiters to Leica standard. These prove home fabrication is possible but are one-offs with no shared design. No open-source RF-coupled adapter/lens design was found on GitHub, Printables, Thingiverse, or Hackaday.

### Leica M mount dimensional data — what's obtainable

- **Published/authoritative:** external bayonet diameter 44mm; 4 tabs; flange focal distance nominally 27.8mm; the RF coupling arm/roller is visible inside the top of the mount throat (Wikipedia, apotelyt, Leica). The M-bayonet patent ("Bajonettvorrichtung für die lösbare Verbindung zweier Kamerateile") was filed by Ernst Leitz GmbH on 10 February 1950 and published 23 October 1952 — a primary source for bayonet geometry.
- **Reverse-engineered / hobbyist-measured:** a Photrio user measured, with a 0.01mm-precision depth gauge, that the rear cam-contact ring protrudes ~6.50mm from the flange plane for M-mount lenses (and ~7.50mm for LTM) at infinity — a usable datum for where the cam surface must sit, though not an official spec.
- **RF mechanism geometry:** the roller sits on an eccentric screw (which sets the infinity "zero") at the end of a pivoting arm whose effective length is set by a second eccentric (which sets the throw ratio); documented across Leica repair/FAQ sources (Nemeth's Leica FAQ; Photrio DIY threads). This tells a designer the roller contact geometry and confirms the body itself has ± adjustment latitude.
- **CAD models:** the **Archive-663/lensMounts** GitHub repo contains a "Leica M" folder with mount/body-mount/cap models in STL and STEP, explicitly shared for reuse (36 stars; license file present). It is a useful starting geometry reference but must be verified against physical measurement — it is not guaranteed to model the RF cam surface.
- **What's genuinely missing / must be measured:** Leica has never published the roller travel, cam-surface tolerance, or the exact reference focal length. These come from (a) consensus hobbyist figures — ~3.0mm roller travel infinity→1m, and a cam depth "not exceeding 0.007 in" (~0.18mm) on 50mm lenses from a post-war Leitz visitor's report quoted on l-camera-forum; (b) direct measurement of a donor body with a depth gauge/dial indicator; and (c) the thin-lens math above (which independently reproduces the ~2.83mm-at-1m figure).

### The reference-focal-length convention

The system standard is **51.6mm** (shared historically with the Nikon rangefinder registration distance). Individual Leica lenses have their true focal length's last digits engraved (e.g., "9" → 51.9mm) and were factory-sorted, so real lenses cluster ~51.6–51.9mm. For a Helios conversion the practical target is: make the cam behave as if the 58mm lens were a 51.6mm lens — i.e., gear the helicoid travel down by ~1/1.27 (≈0.787).

### Helios 44-2 mechanical data (measurable / known)

- Focal length 58mm; 6 elements / 4 groups (double-Gauss Biotar derivative); M42 mount; length 46.5mm; diameter 60.0mm; filter 49mm; weight 222g; 8 straight aperture blades (preset); focus throw ~275°; minimum focus 0.5m (JAPB data sheet, CC BY-SA 4.0).
- **Optical-block separability:** the optical block threads into the rear/helicoid section on a fine thread (measured by one modder as M31×0.5) and bottoms against a thin aluminum spacer ring (~2mm, varies per sample — it was the factory's per-lens infinity shim). Removing/replacing this spacer is the standard DIY infinity adjustment. This means the optical unit can be cleanly transplanted into a new housing and shimmed for infinity.
- **Sample variation:** Soviet QC was loose; each lens's true focal length and required shim vary, so any open design must include user calibration provisions (below).
- **OPAL relevance:** OPAL is a Russian MS-DOS optical-CAD program whose database holds optical prescriptions of some Soviet lenses, with a converter to Zemax. It is relevant only if a builder wants the _optical_ prescription (to verify EFL or model focus behavior); it is **not** needed for the mechanical/RF task, where the true EFL is better obtained by direct bench measurement (as Skyllaney does). The Helios/Biotar heritage (Carl Zeiss Jena Biotar 58/2) is well documented but optically irrelevant to cam design beyond fixing the true EFL number.

### RF coupling range and below-minimum behavior

Design target: RF-coupled infinity→0.7m. Below 0.7m the roller has bottomed out (it reads the same as when no lens is mounted), so the cam should simply _stop driving the roller_ — the Camera Obscura solution is a detent/click-stop at 0.7m after which the helicoid continues into an uncoupled 0.5–0.7m macro zone. Since these users' bodies have no live view, the below-0.7m zone is scale-focus only and should be clearly engraved; the honest design decision is to prioritize accurate coupling from 0.7m to infinity and treat 0.5–0.7m as a bonus zone.

### Calibration provisions for machined DIY copies

Because home-machined copies and loose-tolerance Soviet optics will each differ, the design should bake in user calibration that does not require live view:

- **Infinity shim stack** (Helios's native method; Funleader's gasket method): axial shims between optical block and housing set infinity focus, verified against a distant target.
- **Adjustable cam / eccentric** (7Artisans/TTArtisan method): a rotatable cam ring held by 2–3 screws that the user loosens and nudges to shift RF registration; these lenses ship with a small Allen key precisely for this. This is the most DIY-robust route because it corrects RF registration after assembly.
- **Adapter-side infinity screw** (Shoten R50): a screwdriver-adjustable RF linkage.
- **Body-side latitude:** the M body's own eccentric roller/arm adjustments give a second calibration axis (though correct discipline is to calibrate the lens to the standard, not the body to one lens).
- **Calibration without live view:** ground-glass/tape at the film plane with a loupe, or (Skyllaney's method) a digital full-frame sensor at the correct registration distance compared against a calibrated Messsucher. A film body requires shoot-and-develop iteration, so the design should minimize the number of calibration degrees of freedom (favor approach (c), where only the adapter helicoid + cam matter).

### Open-hardware precedents to model licensing/format on

- **OpenReflex** — 3D-printed 35mm SLR, source under **Creative Commons BY-SA**; establishes the "print + a few hardware-store screws" ethos and license.
- **Archive-663/lensMounts** — mount library distributed in **STL and STEP**, explicitly shared "so you can design something yourself"; demonstrates STEP (parametric, machinable) + STL (printable) dual-format practice, and includes body mounts using a "jam lock" detent instead of a spring — a hint for DIY-robust mechanisms that avoid hard-to-source springs.
- **MRF (Printables)** — a lens-coupled medium-format rangefinder shipped as STL + KiCAD/Gerber + firmware on GitHub, with a documented changelog on wall thickness / light-tightness — a model for tolerance and iteration documentation.
- **Lensfun** — library under LGPL-3.0, database under CC BY-SA 3.0, with a community calibration-submission workflow (Hugin-based). Not mechanical, but the gold standard for a community-contributed calibration database — this project could host a per-sample cam-offset/shim database the same way.

### Realistic conclusion

An open-source machinable design is **feasible**. No missing datum is a true blocker: the flange distances are published, the ~17.7mm working space is generous, the optical block is transplantable and shimmable, the cam gear-ratio is exactly computable (~1.27×) and matches how existing products solve the problem, and calibration methods that work without live view are well established. The chief difficulties are (1) precision — RF focus tolerance for a fast f/2 lens is tight (fractions of a mm at the cam), so tolerances and a calibration procedure must be documented, not just geometry; and (2) the cam is the one part where a naïve linear slope leaves a small residual error, so the design should either specify a profiled cam or a correctly pitched secondary helicoid.

## Recommendations

**Stage 1 — Metrology and math (no CAD yet):**

1. Acquire 2–3 donor Helios 44-2 samples and one M-mount body (or a machinist's M-flange gauge). Measure: the optical-block thread (verify M31×0.5), block length, shoulder position, native infinity shim thickness, and the axial position where the block's reference plane must land for correct FFD. Resolve the 27.80 vs 27.95mm flange ambiguity directly on your target body.
2. Measure the M body's roller travel with a dial indicator from infinity to 0.7m; confirm against the computed 51.6mm figures (2.81mm at 1m, 4.11mm at 0.7m). Record the ~6.5mm cam-contact protrusion datum.
3. Lock in the cam math: target gear ratio ≈0.787 (helicoid → roller), profiled to hold across 0.7m→∞. Decide profiled-cam vs. secondary-helicoid.

**Stage 2 — Choose architecture and prototype:** Adopt **approach (c)** (adapter with its own helicoid, lens locked at infinity) as the reference open-source design — it isolates all RF-critical geometry into user-fabricated parts, is independent of worn donor helicoids, and matches the proven Shoten R50 concept re-cut for 58mm. Offer approach (b) as a "purist" variant that keeps native 0.5m focus. Prototype in 3D-printed PLA/resin for fit-checking, then specify brass/aluminum for the load-bearing helicoid and cam.

**Stage 3 — Documentation and release:** Publish STEP (parametric, for CNC/manual machining) + STL (for printing) + 2D drawings with explicit tolerances on the cam surface and flange; license under CC BY-SA (matching OpenReflex and lensMounts) or a CERN Open Hardware License. Include a no-live-view calibration procedure (shim + adjustable-cam eccentric) and, following Lensfun's model, a community per-sample calibration database.

**Benchmarks that would change the plan:**

- If bench measurement shows the Helios optical block's _true_ EFL differs materially from 58.0mm, recompute the ratio (heed Skyllaney's 0.1mm-EFL warning) — this changes the cam slope, not the approach.
- If prototype focus tests can't hold accuracy at f/2 wide-open across the range with a linear cam, switch to a profiled cam or a secondary helicoid (do not ship a linear-only cam for a fast lens).
- If achievable machining tolerances (especially FDM printing) prove too coarse for RF accuracy, restrict the "printable" claim to the cosmetic barrel and require metal for the helicoid/cam.

## Caveats

- **No official Leica specs.** The roller-travel (~3mm at 1m) and cam-depth (≤0.18mm) figures are knowledgeable-hobbyist/forum consensus plus a post-war visitor's report, not published Leitz documentation; the 0.7m→~4.1mm travel is computed, not quoted. Verify by direct measurement before machining.
- **The flange figure itself is not perfectly settled.** While 27.8mm is the commonly cited M FFD, JAPB notes that "27.80 mm and 27.95 mm are both regularly mentioned," and some reference tables list "27.95 (27.80?)." A ~0.15mm ambiguity is significant at RF tolerances — measure your own body.
- **Commercial cam profiles are undisclosed.** Omnar/Skyllaney and Funleader describe _methods_ (secondary-helicoid pitch changes, sloped cams, floating-lens-block multi-helicoids) but publish no pitch numbers or profiles; this project must derive its own from the math and measurement.
- **Per-sample variation** in Soviet lenses means any single cam/shim spec is only a starting point; individual calibration is mandatory, which is why the design must include user-adjustable provisions.
- **Precision is the real gate, not data availability.** Feasibility hinges on holding sub-0.1mm effective tolerances at the cam for a fast lens; achievable by CNC/manual machining with disciplined calibration but at the edge of hobby FDM printing.
- **Vendor/press sourcing.** Camera Obscura and Shoten internal-mechanism details are drawn from vendor and press descriptions, not independent teardowns; treat them as indicative rather than verified engineering drawings.
- **Legal/trademark caution:** "Leica M" is a trademark; an open-source project should describe compatibility ("M-mount compatible") rather than imply endorsement.
