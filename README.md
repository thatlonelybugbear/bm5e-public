# Bugbear's Mechanics for 5e

Bugbear's Mechanics adds opinionated automation and quality-of-life improvements to Foundry VTT's D&D5e system: reusable chat cards, damage protections, condition controls, activation triggers, overtime effects, and easier enchantment application.

Latest version requires **Foundry VTT 14.367+ (v14)**, **D&D5e 6.0.2+**, and **libWrapper** and is verified forFoundry 14.369 and D&D5e 6.0.6.

This guide covers everyday setup and use. See the [detailed reference](docs/REFERENCE.md) for configuration keys, formulas, integration hooks, and less common options. The in-game documentation also includes **Reference:** pages with those details.

## Table of Contents

- [Installation and Support](#installation-and-support)
- [Getting Started](#getting-started)
- [Chat Cards and Rolls](#chat-cards-and-rolls)
- [Damage Automation](#damage-automation)
- [Conditions and Token Controls](#conditions-and-token-controls)
- [Activation Triggers](#activation-triggers)
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

These menus save changes immediately. Close them with the window's **X**. Chat preferences generally apply to your user; **Compact combined cards** is saved on this client, and **Hide chat-card tags for** is a GM-controlled world setting. Most chat display changes apply immediately; automation preferences affect subsequent uses.

For a first look, try **Compact combined cards**, then use an attack against a targeted token. For enchantments or overtime effects, start with the examples in BM5E's compendiums and adapt them to your game.

## Chat Cards and Rolls

BM5E adds controls and layout improvements to dnd5e chat cards. Enable **Compact combined cards** in **Chat & Rolls** to display linked attack and damage results side by side, with target outcomes and damage controls. This optional layout is bypassed while Midi-QOL is active.

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

Changes refresh linked cards and open popouts on connected clients. **Changing a roll does not automatically undo damage or effects already applied.** Use the appropriate Restore or Remove control when needed.

### Automatic damage dialogs

**Auto Roll Damage** offers **Disabled**, **When the attack hits**, and **Always**. Choose hit-only to open the damage dialog after a successful attack, or Always to open it after misses too. The default is Disabled.

In either enabled mode, save activities open their damage dialog when used, before saves resolve. Rerolling an attack can open missing damage for an eligible hit without duplicating its existing linked damage.

### Other chat options

- **Enable chat document double-clicks:** open or close document sheets from chat names and icons. Normal single-click actions remain available.
- **Show chat action labels:** add text to compact action buttons.
- **Show roll results on message buttons:** display roll-history tooltips and Save/Check indicators.
- **Hide chat-card tags for:** hide item/activity tags from GMs, players, or both. An empty selection shows them to everyone.
- **Auto pop-out use messages:** configure reusable cards by item, activity, actor, or compendium. An activity's **Use message pop-out** option can override these defaults.

### Restoring damage and removing effects

With **Enable damage restoration**, damage trays record applications and offer **Restore** afterward. Restoring an Arcane Ward application also restores the ward HP it spent.

The usage card's Effects tray offers **Remove** to undo recorded effect applications.

Players can act only on actors they own, subject to the system's **Allow Player Damage Tray** and **Allow Player Effects Tray** settings. An active GM records applications when the player cannot update the chat message.

## Damage Automation

Open **Damage Mechanics** to select the automations you want. Enable **Mutable Damage Calculation** for Arcane Ward and zero-HP protections, then select them under **Mutable Damage Automations**.

| Feature | Setup and behavior |
| --- | --- |
| **Arcane Ward** | An owned item with identifier `arcane-ward` tracks ward HP through its uses. Eligible Abjuration casting restores the ward, following the selected 2014 or 2024 rules. |
| **Death Ward** | Recognizes an effect named `Death Ward` or `Protection from Death`, or one originating from an item with identifier `death-ward`. Consumes the protection to leave the actor at 1 HP. |
| **Undying Sentinel** | Requires an owned item with identifier `undying-sentinel` and sufficient configured uses. Leaves the actor at 1 HP and uses its Heal activity when available. |
| **Undead Fortitude** | Requires an owned item with identifier `undead-fortitude`. Offers the Constitution save against qualifying damage; success leaves the actor at 1 HP. |

Arcane Ward absorbs eligible damage before zero-HP protections. Those protections resolve in this order: **Death Ward > Undying Sentinel > Undead Fortitude**. Only one resolves per application.

**Standard Damage Automations** work without Mutable Damage Calculation:

- **Elemental Adept:** ignores matching spell-damage resistance when the caster has an item with identifier `elemental-adept` whose name ends with the damage type, such as `Elemental Adept (Fire)`. It does not change damage-die results of 1 to 2.
- **Massive Damage:** marks three failed death saves when damage reduces an actor to 0 HP and the remaining damage meets or exceeds its maximum HP, unless an enabled protection prevents that outcome.

See the detailed reference for ward ordering, edition differences, and feature-specific requirements. Rerolling an Undead Fortitude save does not retroactively change damage already applied.

## Conditions and Token Controls

These options extend the system's existing condition handling.

- **Status palette:** enable **Status HUD Sorting** in **Token HUD** for labeled statuses, sorting, search, and configurable columns and scale.
- **Prone:** enable **Implement Prone Mechanics** to select crawling when Prone is applied and restore the previous movement action when it ends. Click an active Prone token icon to stand, spending half walking speed when enough movement remains.
- **Grappling:** enable **Implement Grapple Mechanics**, then click a token's Grappled icon to attempt an escape. BM5E handles the 2014 contest or 2024 DC and removes the associated effect on success. An active GM must be connected; spending the action remains manual. When a Save or Check activity applies Grappled as a rider, its DC is saved for 2024 escape attempts, falling back to the grappler's Strength DC. The GM can right-click the icon to edit a missing or incorrect DC.
- **Falling controls:** **Harmless Falling on Right-click** lets the GM land a token without damage using its Falling icon. Normal falling is provided by the system. Optional scene-load reevaluation is available under **Conditions & Effects** and is disabled by default.

## Activation Triggers

Use activation triggers for activities that react to damage, healing, attacks, movement, or creatures entering an area.

1. Set the activity's activation type to **BM5E Triggers**.
2. Open its trigger editor from the Activation Cost controls.
3. Choose the event and any filters, such as hit/miss, damage type, creature type, size, or HP.
4. Choose whether to automatically target the other creature and immediately use the activity, or leave a Triggered chat message for manual use.

All enabled filters must match. Multiple choices within one filter match any selected value. Configured resource availability is checked before offering an activity; the system checks again when it is used.

**In Range** uses a token-attached emanation. Configure its radius through the activity's template, choose entry/exit or turn timings, and set trigger limits. Use the activity to place the region. Moving the emanation over a stationary token does not itself trigger entry or exit.

For an example of incoming damage reduction, see **Ghostly Command** in **BM5E Overtime Test Items**. Reduction requires Mutable Damage Calculation; reaction costs and timing are not enforced automatically. Forced Movement triggers require BAA5E.

The detailed reference lists damage and movement formula values, filter behavior, and trigger limits.

## Overtime Effects

Overtime effects offer activities at configured timings, such as start-of-turn damage or a saving throw that removes an effect on success.

### Setting one up

1. Start with an example from **BM5E Overtime Test Effects** or **BM5E Overtime Test Items**.
2. Open its Active Effect and add or edit a **BM5E** change. The **Overtime** key is filled automatically; its change-summary icon opens the overtime editor.
3. Choose an existing activity on the source item, link another activity, or configure a generated Save, Check, Damage, or Heal activity.
4. Choose the timing and outcomes: for example, **end of turn, then saving throw, then remove overtime on success**.

You can also set an item activity's activation cost to **BM5E Overtime** and configure it directly.

### Common options

- **Preserve source DC:** use the source activity's DC for a linked Save or Check, rather than calculating it from the affected actor.
- **Also usable outside combat:** permit outside-combat use while retaining the selected combat timing.
- **No combat trigger:** use for interval-only effects, such as regeneration based on elapsed world time.
- **Outcome counters:** resolve actions after total or consecutive successes/failures, or after a number of triggers.

Same-item activity links survive compendium import. Linked spell damage and healing retain the original cast's slot scaling. Generated damage and healing ticks exclude native recipient bonuses to damage or healing performed, while preserving source physical properties such as magical damage. Other bonus providers need to support BM5E's recipient-bonus exclusion flag. Updating an effect without changing its overtime configuration preserves progress and interval timing.

During combat, BM5E creates messages offering available overtime activities. With Midi-QOL's Activation Cost Automation set to Auto Trigger, those activities can roll automatically. See the detailed reference for serialized configuration, timing options, and outcome examples.

## Enchantments

Enable **Enable enchantment handling** under **Conditions & Effects** to choose recipients and items from enchantment chat cards. BM5E selects the destination; dnd5e applies the native enchantment.

### Choosing an item

1. Use the Enchant activity with the intended recipients targeted.
2. Check the recipients on its chat card and click the enchantment area.
3. Choose a compatible item for each actor. **Skip** omits a recipient; **Cancel Remaining** stops the remaining pickers.

A unique preferred match is selected automatically. If you do not own a recipient, BM5E sends the GM an application request. Native item restrictions and ownership permissions still apply.

### Preferred identifiers and tags

In the activity's **Bugbear's Mechanics** Behavior fieldset, enter **Preferred item identifiers or tags**, separated by commas:

| Entry | Matches |
| --- | --- |
| `longsword` | That exact item identifier. |
| `shield*` | Identifiers containing `shield`, including `tower-shield`. |
| `tag:pact-weapon` | The exact, case-sensitive item tag `pact-weapon`. |
| `shield*, tag:pact-weapon` | Either matching identifier or matching tag. |

Add item tags under **Item Identifier: Tags (BM5e)**: type a tag, then click Add. BLT5e tags also participate in matching and appear read-only in BM5E's field. For a pact weapon, add the item tag `pact-weapon` and use `tag:pact-weapon` in the enchantment preference.

With several tag or wildcard matches, the picker shows compatible matches only. Plain identifiers alone put matches first. **Apply to all matching items** applies every compatible match within the configured limits.

**Preferences are not restrictions.** Blank preferences or no matches open the normal picker; another compatible item can still be dragged onto the tray. Tags alone do not apply an enchantment or enforce a single pact weapon.

### Limits and stacking

| Option | Meaning | Default |
| --- | --- | --- |
| **Maximum enchanted items** | Different items enchanted at once, across all uses and recipients. | `1`; blank restores `1`. `0` means unlimited. |
| **Maximum enchanted items per use** | Active applications from one use, including repeated stacks on the same item. | `0` means unlimited. |
| **Allow enchantment stacks** | Allow multiple copies of this activity's enchantment on one item. | Without it, matching enchantments replace the previous matching copy. |

For a Rust Monster, prefer `armor*, shield*`, enable **Allow enchantment stacks**, set the per-use limit to **1**, and use a higher overall limit or **0**. Each use can affect one item, while enchantments from earlier uses remain active and visible in the tray. The activity's target count separately limits recipients.

Self-enchanting activities show **(Self-enchant)** in their card header. They retain stacking and popout options; target-item limits and matching preferences are hidden and ignored, with their saved values preserved.

### Undoing enchantments

Use an enchanted item's undo control:

- **Click:** remove one enchantment.
- **Shift-click:** remove all enchantments shown in this tray.
- **Ctrl+Shift-click:** remove all shown enchantments on that item.

Bulk undo requires ownership of all affected items.

### Material restrictions

The optional **Enable item material properties** world setting adds **Wooden**, **Metal**, **Glass**, and **Crystal** to weapons and equipment. It is disabled by default and requires a reload; assign materials manually.

Choose these under the Enchant activity's **Restrictions: Valid Properties**. With BM5E enchantment handling enabled, properties are also available for **Any** item type, so one activity can affect matching weapons and equipment. Item identifiers do not determine material.

## Midi-QOL Compatibility

BM5E can run alongside Midi-QOL, with specific integrations and limits. Enabling Midi-QOL does not disable all BM5E features.

| Area | What happens with Midi-QOL active |
| --- | --- |
| **Chat cards and automatic damage rolls** | Midi-QOL manages its attack cards and damage-roll workflow. BM5E bypasses its combined/compact attack layout and Auto Roll Damage, regardless of the saved BM5E preferences. |
| **Damage and healing triggers** | BM5E uses HP changes from Midi's damage applications to distinguish damage from healing. Receiving temporary HP alone does not count as taking damage. |
| **Reversing damage** | BM5E recognizes Midi damage-card **Reverse** and **Reverse All** clicks and suppresses damage/healing triggers for the matching HP restoration. Applying damage again can trigger them again. This does not undo activities or effects triggered by the original application. |
| **Overtime checks and saves** | For supported overtime rolls made on the active GM client, BM5E waits for Midi's roll operation to finish before resolving success/failure. This includes optional bonuses and rerolls handled within that operation. |

The overtime integration does **not** cover a later chat-card reroll or a GM changing an already-resolved outcome. Player-client overtime resolution is not covered by this final-result integration. Reversal detection is specific to the damage-card controls above; other undo tools or macros may use different paths.

See the detailed reference for integration surfaces. When reporting a problem, include the Foundry, dnd5e, BM5E, Midi-QOL, and DAE versions, relevant automation settings, and steps to reproduce it.
