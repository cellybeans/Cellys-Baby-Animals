# AH-Targeted Baby Animal Patch Example

This package demonstrates the intended CBA + Tamework + optional AH split using Tetrabird as the first populated baby family.

## CBA + Tamework Only

- `Tetrabird_Chick` is a standalone role using vanilla `Template_Animal_Neutral`, matching vanilla babies like `Chicken_Chick`.
- `Tamed_Tetrabird_Chick` is present as a standalone vanilla-style role using `Template_Livestock`, matching vanilla tamed babies like `Tamed_Chicken_Chick`. It is not normally reachable without AH because the wild chick is not tameable.
- `CBA_Tetrabird_Flocks.json` patches any loaded adult `Tetrabird` role so its flock can include `Tetrabird_Chick`.
- The chick is not tameable because its baseline role has `IsTameable: false` and no AH interaction config is present.
- `TameRoleChange` can still be set on the standalone role. That mapping is harmless without AH because the baby is not tameable.

## CBA + Tamework + AH

- `CBA_Tetrabird_AH_Roles.json` switches the wild baby from vanilla `Template_Animal_Neutral` to `AH_Template_Animal_Neutral` and flips `IsTameable: true`.
- `CBA_Tetrabird_AH_Tamed_Roles.json` switches the tamed baby from vanilla `Template_Livestock` to `AH_Template_Livestock_Tamed` and adds the AH command capability flags.
- `CBA_AH_Neutral_*.json` patches AH's actual neutral config files:
  - `AHIntNeutral`
  - `AHBreedNeutral`
  - `AHCompNeutral`
  - `AHHappNeutral`
  - `AHNeedsMain`
  - `AHTraitNeutral`
- `CBA_AH_Neutral_Breeding.json` merges only the Tetrabird lifecycle override into `AHBreedNeutral` instead of copying the whole AH breeding file.

## Adding Another Baby Family

For each new baby family, add the family-specific values in these places:

- Add the standalone wild baby role and give it its correct `TameRoleChange`.
- Add a small flock patch for the adult wild role, because flock arrays are species-specific.
- Add a small AH wild-role patch that switches the baby to the closest AH neutral template and flips `IsTameable` when AH exists.
- Add a small AH tamed-role patch that switches the tamed baby to the closest AH tamed template and adds AH command capability values.
- Add wild/tamed baby role IDs to the appropriate AH interaction and trait patch.
- Add tamed baby role IDs to the appropriate AH companion, happiness, and needs patches.
- Add the tamed baby role ID plus lifecycle family entries to the appropriate AH breeding patch.

The important lesson is that broad AH files should not be copied into CBA just to add one baby. CBA should own standalone baby roles and small optional patches that modify AH's real configs only when AH exists.
