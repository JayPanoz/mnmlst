# Nightly Builds

**NOT READY FOR PRODUCTION USE. For testing only.**
 
The purpose of this `nightly_builds` folder is to provide with the latest CSS outputs from the LESS’ Design System. Basically…

1. We make sure they compile.
2. We can check if anything is broken.

## About “Experimental”

From now on, advanced stuff will be added in `experimental.less`, a file living in isolation, to make sure they don’t compile in the “core” of mnmlst. 

**Please note** they’ll be released and managed as optional “modules”, they won’t make it into mnmlst in order to prevent bloat.

## Current Modules

**Please note** I can’t guarantee all will make it to v2. They may eventually be released, provided that they prove useful and don’t bloat both the system and the CSS output.

### Design helpers

1. Background modes (sepia, gray and night)
2. Baseline grid (vertical rhythm checker)

### Functional classes

1. Subtle Color Palette Generator
2. Columns Generator for Grids (e.g. image grid)
3. Freeform System Generator (cf. International Style / Swiss Design posters)
4. Slightly better than prettier horizontal rule (using linear gradient)
