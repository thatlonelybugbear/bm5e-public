# Bugbear's Mechanics for 5e

Bugbear's Mechanics adds opinionated automation and quality-of-life improvements to Foundry VTT's D&D5e system: reusable chat cards, damage protections, condition controls, activation triggers, overtime effects, and easier enchantment application.

Requires **Foundry VTT 14.367+ (v14)**, **D&D5e 6.0.2+**, and **libWrapper**. Verified with Foundry 14.369 and D&D5e 6.0.6.

This guide covers everyday setup and use. See the [detailed reference](docs/REFERENCE.md) for configuration keys, formulas, integration hooks, and less common options. The in-game documentation also includes **Reference:** pages with those details.

## Table of Contents

- [Installation and Support](#installation-and-support)
- [Getting Started](#getting-started)
- [Chat Cards and Rolls](#chat-cards-and-rolls)
- [Damage Automation](#damage-automation)
- [Conditions and Token Controls](#conditions-and-token-controls)
- [Activation Triggers](#activation-triggers)
- [Templates and Emanations](#templates-and-emanations)
- [Overtime Effects](#overtime-effects)
- [Enchantments](#enchantments)
- [Midi-QOL Compatibility](#midi-qol-compatibility)

## Installation and Support

**Starting with version 14.606.3, BM5E is distributed through Foundry VTT's Patreon integration and Protected Content system.** Access requires an eligible membership on [Bugbear's Den Patreon](https://www.patreon.com/cw/thatlonelybugbear).

1. Link your Patreon account to your Foundry VTT account.
2. Install BM5E through Foundry's package installer, along with libWrapper.
3. Enable both modules in your world. Updates use Foundry's normal module update controls.

If your membership includes BM5E but you cannot install or update it, contact me through Patreon or Discord. The **Module Support** settings menu links to documentation, the issue tracker, Discord, and Patreon.

You may continue using versions lawfully obtained during your subscription after it ends. Further updates require an active subscription or another entitlement that includes updates. Commercial games and streams are permitted with module credit and a Patreon link; see [LICENSE](https://raw.githubusercontent.com/thatlonelybugbear/bm5e-public/refs/heads/main/LICENSE) for the full terms.

## Getting Started

Open **Game Settings > Module Settings > Bugbear's Mechanics for 5e** and choose the features you want:

| Settings menu | What you can configure |
| --- | --- |
| **Chat & Rolls** | Combined cards, rerolls, automatic damage dialogs, document double-clicks, and popouts. |
| **Token HUD** | A labeled, searchable status palette. |
| **Damage Mechanics** | Damage protections and other damage automations. |
| **Conditions & Effects** | Prone, grappling, falling controls, enchantments, and optional item materials. |
| **Troubleshooting** | Overtime debug logging. |

These menus save changes immediately. Close them with the window's **X**. Personal settings, including Chat & Rolls and Token HUD preferences, follow your user across browsers. **Hide chat-card tags for** is a GM-controlled world setting. See [setting scopes and updates](docs/REFERENCE.md#module-settings) for details.

For a first look, try **Compact combined cards**, then use an attack against a targeted token. For enchantments or overtime effects, start with the examples in BM5E's compendiums and adapt them to your game.

## Chat Cards and Rolls

Enable **Compact combined cards** in **Chat & Rolls** to display linked attack and damage results side by side, with target outcomes and damage controls. Hidden attack results stay hidden. Midi-QOL manages its own attack-card layout.

### Using the card

- Click an attack's divider chevron to fold or expand its row.
- Use the checkmark beside damage to apply it. With **Enable damage restoration**, an applied row offers **Apply again / Restore**.
- Click the top **Attack** or **Damage** button to create another roll. Use the row's dice control to change an existing roll.
- Hold **Shift** to reveal an attack row's trash control. It deletes the attack and its paired damage when permitted. Restore applied damage before deleting if you need to undo it.

### Rerolls

Enable **Show reroll controls** to access these controls on messages you can update:

| Action | Result |
| --- | --- |
| Click an Attack, Save, or Check dice control | Choose Advantage, Normal, or Disadvantage using stored dice; roll a missing extra die when needed. |
| Click a Damage or Healing dice control | Reroll as Normal or Critical. |
| **Shift-click** a dice control | Roll fresh dice in its current mode. |

**Changing a roll does not automatically undo damage or effects already applied.** Use Restore or Remove when needed. See [roll controls](docs/REFERENCE.md#chat-roll-controls) for stored-dice behavior and card updates.

### Automatic damage dialogs

**Auto Roll Damage** offers **Disabled**, **When the attack hits**, and **Always**. Either enabled mode also opens damage dialogs for Save activities. See [automatic damage rules](docs/REFERENCE.md#auto-roll-damage) for timing and reroll handling.

### Other chat options

Chat & Rolls also controls document double-clicks, action labels, result tooltips, tag visibility, and [automatic card popouts](docs/REFERENCE.md#auto-popout).

### Restoring damage and removing effects

Enable **Enable damage restoration** to undo recorded damage with **Restore**. Use **Remove** in the Effects tray to undo recorded effect applications. Players can act only on actors they own, subject to the system's tray settings. See [damage restoration](docs/REFERENCE.md#damage-application-restore) and [effect controls](docs/REFERENCE.md#effect-application-controls) for permissions and application records.

## Damage Automation

Open **Damage Mechanics** to select the automations you want. Enable **Mutable Damage Calculation** for Arcane Ward and zero-HP protections, then select them under **Mutable Damage Automations**.

| Feature | Setup and behavior |
| --- | --- |
| **Arcane Ward** | An owned item with identifier `arcane-ward` tracks ward HP through its uses. Eligible Abjuration casting restores the ward, following the selected 2014 or 2024 rules. |
| **Death Ward** | Recognizes an effect named `Death Ward` or `Protection from Death`, or one originating from an item with identifier `death-ward`. Consumes the protection to leave the actor at 1 HP. |
| **Undying Sentinel** | Requires an owned item with identifier `undying-sentinel` and sufficient configured uses. Leaves the actor at 1 HP and uses its Heal activity when available. |
| **Undead Fortitude** | Requires an owned item with identifier `undead-fortitude`. Offers the Constitution save against qualifying damage; success leaves the actor at 1 HP. |
| **Resilience** | A configurable flag grants a save when damage would reduce positive HP to 0. Success leaves 1 HP. Native emanations can distribute the effect; see [Resilience setup](docs/REFERENCE.md#resilience). |

Resilience requires Mutable Damage Calculation but does not require an item integration to be selected. See [damage automation details](docs/REFERENCE.md#damage-automations) for protection ordering and edition-specific behavior.

**Standard Damage Automations** work without Mutable Damage Calculation:

- **Elemental Adept:** ignores matching spell-damage resistance; it does not change damage-die results of 1 to 2.
- **Massive Damage:** records failed death saves for qualifying lethal damage.

See [standard automation requirements](docs/REFERENCE.md#standard-damage-automations) for setup and qualifying damage.

## Conditions and Token Controls

These options extend the system's existing condition handling.

- **Status palette:** enable **Status HUD Sorting** in **Token HUD** for labeled statuses, sorting, search, and configurable columns and scale.
- **Prone:** enable **Implement Prone Mechanics** to select crawling when Prone is applied and restore the previous movement action when it ends. Click an active Prone token icon to stand, spending half walking speed when enough movement remains.
- **Grappling:** enable **Implement Grapple Mechanics**, then click a token's Grappled icon to attempt a 2014 or 2024 escape. An active GM must be connected; spending the action remains manual. The GM can right-click the icon to edit the escape DC. See [grapple resolution](docs/REFERENCE.md#grapple-mechanics) for DC sources and rerolls.
- **Falling controls:** **Harmless Falling on Right-click** lets the GM land a token without damage using its Falling icon. Normal falling is provided by the system. Optional scene-load reevaluation is available under **Conditions & Effects** and is disabled by default.

## Activation Triggers

Use activation triggers for activities that react to damage, healing, attacks, movement, or creatures entering an area.

1. Set the activity's activation type to **BM5E Triggers**.
2. Open its trigger editor from the Activation Cost controls.
3. Choose the event and any filters, such as hit/miss, damage type, creature type, size, or HP.
4. Choose whether to automatically target the other creature and immediately use the activity, or leave a Triggered chat message for manual use.

**In Range** reacts to turn events or tokens and Regions moving into or out of range. Add the In Range area behavior to the activity that places the template, select a **Triggered Activity** from the same Item, and configure that activity's events and per-token limits.

For a complete example, import **BM5E Spirit Guardians** from **BM5E Overtime Test Items**. Its **Cast** activity places the emanation and uses normal spell resources and concentration. **Guardians Save** handles triggered saves and damage, while a native Apply Active Effect behavior grants Half Speed to eligible creatures in the Region.

For incoming damage reduction, try **Ghostly Command** from the same compendium. It requires Mutable Damage Calculation; reaction costs and timing remain manual. See [trigger options](docs/REFERENCE.md#activation-triggers) for event timing, filters, limits, and damage or movement formulas.

## Templates and Emanations

Choose **Custom**, **Palette**, or **Default** appearance from the Item's area controls. Activities inherit those settings unless **Override Targeting** is enabled.

With Midi-QOL inactive, Self-range emanations are placed on the caster's token and attach unless **Stationary** is selected. Use the casting activity again to **Re-Cast** or **Turn Off**. Concentration-linked emanations end when concentration ends, and removal clears matching Sequencer animations.

Template placement previews eligible targets. Placing confirms them; cancelling restores the previous selection. Moving an already placed Region leaves selected targets unchanged. See [template settings and placement](docs/REFERENCE.md#templates-and-emanations) for Palette snapshots, walls, Levels, elevation, and targeting compatibility.

## Overtime Effects

Overtime effects offer activities at configured timings, such as start-of-turn damage or a saving throw that removes an effect on success.

### Setting one up

1. Start with an example from **BM5E Overtime Test Effects** or **BM5E Overtime Test Items**.
2. Open its Active Effect and add or edit a **BM5E** change. Choose `flags.bm5e.overtime` from the attribute-key suggestions; its change-summary icon opens the overtime editor.
3. Choose an existing activity on the source item, link another activity, or configure a generated Save, Check, Damage, or Heal activity.
4. Choose the timing and outcomes: for example, **end of turn, then saving throw, then remove overtime on success**.

You can also set an item activity's activation cost to **BM5E Overtime** and configure it directly.

### Common options

- **Preserve source DC:** use the source activity's DC for a linked Save or Check, rather than calculating it from the affected actor.
- **Also usable outside combat:** permit outside-combat use while retaining the selected combat timing.
- **No combat trigger:** use for interval-only effects, such as regeneration based on elapsed world time.
- **Outcome counters:** resolve actions after total or consecutive successes/failures, or after a number of triggers.

BM5E offers available overtime activities in combat chat messages. See the [overtime reference](docs/REFERENCE.md#overtime-effects) for serialized configuration, linked activity scaling, outcome examples, and automatic rolling with Midi-QOL.

## Enchantments

Enable **Enable enchantment handling** under **Conditions & Effects** to choose recipients and items from enchantment chat cards. BM5E selects the destination; dnd5e applies the native enchantment.

### Choosing an item

1. Use the Enchant activity with the intended recipients targeted.
2. Check the recipients on its chat card and click the enchantment area.
3. Choose a compatible item for each actor. **Skip** omits a recipient; **Cancel Remaining** stops the remaining pickers.

A unique preferred match is selected automatically. If you do not own a recipient, BM5E sends the GM an application request. Native item restrictions and ownership permissions still apply.

### Preferred identifiers and tags

In the activity's **Bugbear's Mechanics** Behavior fieldset, enter preferred identifiers or tags, such as `longsword`, `shield*`, or `tag:pact-weapon`. Add tags below the item's **Item Identifier** field.

Preferences help choose compatible items; they are not restrictions. See [enchantment matching](docs/REFERENCE.md#enchantments) for wildcard and tag rules, BLT5e tags, and **Apply to all matching items**.

### Limits and stacking

**Maximum enchanted items** limits distinct items across uses; **Maximum enchanted items per use** limits applications from one use. Set either to **0** for unlimited. **Allow enchantment stacks** permits repeated copies on one item. See [enchantment limits and examples](docs/REFERENCE.md#enchantments) before configuring multiple recipients or self-enchantment.

### Undoing enchantments

Use an enchanted item's undo control:

- **Click:** remove one enchantment.
- **Shift-click:** remove all enchantments shown in this tray.
- **Ctrl+Shift-click:** remove all shown enchantments on that item.

Bulk undo requires ownership of all affected items.

### Material restrictions

Enable **Enable item material properties** to assign Wooden, Metal, Glass, or Crystal to weapons and equipment, then use them under the Enchant activity's **Restrictions: Valid Properties**. Materials are assigned manually; enabling the setting requires a reload. See [material restrictions](docs/REFERENCE.md#enchantments) for supported item types.

## Midi-QOL Compatibility

Midi-QOL manages its own attack cards, automatic damage rolls, and template placement. BM5E also supports damage/healing triggers and specific overtime workflows alongside it. See [integration details and limits](docs/REFERENCE.md#midi-qol-compatibility) before relying on a particular workflow.

When reporting a problem, include the Foundry, dnd5e, BM5E, Midi-QOL, and DAE versions, relevant automation settings, and steps to reproduce it.
