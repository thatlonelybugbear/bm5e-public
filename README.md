# Bugbear's Mechanics for 5e

Automation and quality-of-life tools for Foundry VTT's D&D5e system: reusable chat cards, damage protections, condition controls, triggered activities, overtime effects and enchantments.

- **Requires:** Foundry VTT 14.367+ (v14), D&D5e 6.1.0 and libWrapper. Verified for Foundry 14.370.
- **Configuration details:** [Online reference](docs/REFERENCE.md). The in-game Documentation journal contains this compact guide.

## Installation and Support

1. Link an eligible [Bugbear's Den Patreon](https://www.patreon.com/cw/thatlonelybugbear) membership to your Foundry account.
2. Install BM5E and libWrapper through Foundry's package installer.
3. Enable both in your world. Use Foundry's normal controls for updates.

- **Module Support** in settings links to documentation, Discord, Patreon and the issue tracker. Contact me through Patreon or Discord if eligible access is not working.
- You may keep using versions lawfully obtained during your subscription. Further updates require an eligible entitlement.
- Commercial games and streams require module credit and a Patreon link. See [LICENSE](https://raw.githubusercontent.com/thatlonelybugbear/bm5e-public/refs/heads/main/LICENSE) for full terms.

## Getting Started

Open **Game Settings > Module Settings > Bugbear's Mechanics for 5e**:

- **Chat & Rolls:** compact cards, rerolls, automatic damage dialogs, double-clicks and popouts.
- **Token HUD:** searchable statuses with labels and sorting.
- **Damage Mechanics:** damage protections and automation.
- **Conditions & Effects:** prone, grappling, falling, enchantments and item materials.
- Changes save immediately. Personal chat/HUD preferences follow your user across browsers.
- Start with **Compact combined cards** and attack a targeted token. Use the compendium examples for more involved setups.

## Chat Cards and Rolls

- **Compact combined cards** places linked attack and damage results together. Hidden results stay hidden.
- Top **Attack/Damage** buttons create new rolls. Row dice controls change existing rolls; **Shift-click** rolls fresh dice.
- Enable **Show reroll controls** for attack/save/check modes and normal/critical damage rerolls.
- **Auto Roll Damage:** choose Disabled, When the attack hits or Always. Enabled modes also open damage dialogs for Save activities.
- **Show roll results on message buttons** adds equal button widths, colors and roll tooltips. Disable it for the system's appearance.
- Enable **damage restoration** before applying damage to make it restorable. Use **Remove** in the Effects tray to undo recorded effect applications.
- Rerolling does not undo damage or effects already applied. Restore/remove them separately when needed.
- Double-click document icons to open permitted sheets or images.

[Chat controls and permissions](docs/REFERENCE.md#chat-roll-controls)

## Damage Automation

- Enable **Mutable Damage Calculation** for **Arcane Ward**, **Death Ward**, **Undying Sentinel** and **Undead Fortitude**, then select the desired automations.
- **Resilience** lets an effect grant a saving throw when damage would reduce positive HP to zero. It also requires Mutable Damage Calculation.
- **Elemental Adept** and **Massive Damage** are available separately under Standard Damage Automations.
- These features require correctly configured items or effects. See [setup requirements](docs/REFERENCE.md#damage-automations) and [Resilience](docs/REFERENCE.md#resilience).

## Conditions and Token Controls

- **Status HUD Sorting:** labeled conditions with search, sorting and adjustable layout.
- **Prone:** automatically switch to crawling. Click the active Prone icon to stand when enough movement remains.
- **Grappling:** click Grappled to attempt an escape; an active GM is required. Spending the action remains manual.
- **Falling:** the GM can optionally right-click Falling to land harmlessly. Normal falling is handled by the system.

[Condition options](docs/REFERENCE.md#grapple-mechanics)

## Activation Triggers

1. Set an activity's activation type to **BM5E Triggers** and open its trigger editor.
2. Choose an event, filters and whether to offer the activity or use it automatically.

- Events cover damage, healing, attacks, saves, activity use, movement and **In Range**.
- **About to be hit** offers reactions such as Shield. Choose an attack-card button or a separate reaction message.
- **In Range:** add the area behavior to the activity placing the template and select a triggered activity from the same item.
- Start with **Spirit Guardians**, **Ghostly Command** or **Absorb Elements** in the example compendium. Their descriptions explain their setup.

[Trigger setup and options](docs/REFERENCE.md#activation-triggers)

## Templates and Emanations

- Choose **Custom**, **Palette** or **Default** appearance on the item, with optional activity overrides.
- Self emanations can attach to the caster. Use the casting activity again to **Re-Cast** or **Turn Off**. Concentration-linked areas end with concentration.
- **Enable template targeting** previews targets during placement. Confirming selects them; cancelling restores the previous selection.
- If Midi-QOL handles targeting on template draw, BM5E defers to it.

[Template and emanation options](docs/REFERENCE.md#templates-and-emanations)

## Overtime Effects

- Start with an example from **BM5E Overtime Test Effects** or **BM5E Overtime Test Items**.
- Open its Active Effect, choose a **BM5E** change and select `flags.bm5e.overtime`. The change-summary icon opens the editor.
- Choose an activity, timing and outcome: for example, an end-of-turn save that removes the effect on success.
- Alternatively, set an item's activity activation type to **BM5E Overtime**.
- Options include source DCs, outside-combat use, timed intervals and success/failure counters.

[Overtime configuration](docs/REFERENCE.md#overtime-effects)

## Enchantments

- Enable **enchantment handling** under **Conditions & Effects**.
- Use an Enchant activity, check recipients on its card and click the enchantment area to choose their items. Unowned recipients generate a GM request.
- Set preferred item identifiers or tags to speed up selection. Configure limits and stacking in the activity's **Bugbear's Mechanics** fieldset.
- Use an item's undo control to remove an enchantment. **Shift-click** removes all shown; **Ctrl+Shift-click** removes all shown on that item.
- Optional item materials provide additional enchantment restrictions.

[Matching, limits and permissions](docs/REFERENCE.md#enchantments)

## Updating Items from Compendiums

- Right-click a sourced inventory/world item and choose **Update from Compendium (BM5e)**.
- GMs can also update existing copies from a compendium's context menu.
- Choose whether to preserve names, images, descriptions or Active Effect world/module flags. Other customizations reset to the source setup.
- Updates retain item identity and character state, including levels, resources and equipment.

[Preservation choices and bulk updates](docs/REFERENCE.md#updating-items-from-compendiums)

## Midi-QOL Compatibility

- Midi-QOL manages its own attack cards, automatic damage rolls and template placement.
- Supported BM5E damage/healing triggers and overtime workflows can run alongside it. Check the [integration limits](docs/REFERENCE.md#midi-qol-compatibility) for your workflow.
- For bug reports, include module/system versions, relevant settings and steps to reproduce.
