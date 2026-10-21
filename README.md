# Bugbear's Mechanics for 5e

Bugbear's Mechanics is a Foundry VTT module which introduces some highly opinionated system mechanics.

Requires Foundry VTT 14.367+ (v14), D&D5e 6.0.2+, and libWrapper. Verified with Foundry 14.369 and D&D5e 6.0.6.

## Table of Contents

- [Installation and License](#installation-and-license): installation, the upcoming Patreon integration, and permitted use.
- [Module Settings](#module-settings): chat icon double-clicks, damage automation, mutable damage hooks, chat popouts, and status HUD sorting.
- [Chat Roll Controls](#chat-roll-controls): compact-card labels and in-place rerolls.
- [Damage Automations](#damage-automations): Arcane Ward, Death Ward, Undying Sentinel, Undead Fortitude, Elemental Adept, and Massive Damage.
- [Damage Application Restore](#damage-application-restore) and [Effect Application Controls](#effect-application-controls): Apply, Restore, and Remove controls and player permissions.
- [Midi-QOL Compatibility](#midi-qol-compatibility): healing, damage triggers, and reversals.
- [Prone Mechanics](#prone-mechanics), [Grapple Mechanics](#grapple-mechanics), and [Falling Mechanics](#falling-mechanics): token-icon controls and condition automation.
- [Activation Triggers](#activation-triggers): incoming and outgoing damage, healing, attack, movement, and in-range triggered activities.
- [Overtime Effects](#overtime-effects): automatic combat and encounter activity triggers from Active Effects.
- [Enchantments](#enchantments): chat-card target and item selection for dnd5e enchantment activities.

## Installation and License

**Version 14.606.2 is the final release outside Foundry VTT's Patreon integration.** Install or update this version using the existing manifest link provided to supporters. Future releases will use Foundry VTT's Patreon integration and Protected Content system; installation instructions will be updated when that integration is available.

You may continue using versions lawfully obtained during your subscription after it ends. Further updates require an active subscription or another entitlement that includes updates. Commercial games and streams are permitted with module credit and a Patreon link; see [LICENSE](<https://raw.githubusercontent.com/thatlonelybugbear/bm5e-public/refs/heads/main/LICENSE>) for the full terms.

## Module Settings

Settings are grouped into **Chat & Rolls**, **Token HUD**, **Damage Mechanics**, **Conditions & Effects**, and **Troubleshooting** menus. Existing saved preferences are retained. These menus save changes immediately; close them with the window's X. Token HUD changes update an open HUD without saving the main Game Settings form.

**Chat & Rolls** preferences apply to your user, except **Compact combined cards**, which is saved on this client, and **Hide chat-card tags from players**, which is a GM-controlled world setting. These settings do not require a client reload. Label, reroll-control, and result-display changes refresh existing chat cards and open message popouts immediately. Automation and pop-out preferences apply to subsequent uses; document permissions still govern rolls and activity use.

### Module Support

The **Module Support** settings menu opens BM5E documentation, issue tracker, Discord, Ko-Fi, and Patreon links.

### Chat Icon Double Clicks

BM5E adds double-click behavior to chat card document icons.

- Double-click an actor name, item icon, activity icon, or effect icon in chat to open that document sheet. Double-click a target pill to open the target actor's sheet when permitted.
- Item icons in Triggered and Overtime activity lists support double-clicks to open or close the item sheet, subject to the existing setting and visibility rules. Adding separate activity icons is deferred pending native system support. Clicking the item/activity text still uses the activity.
- Effect tray handling is restricted to the effect icon rather than the full effect row.
- If a target actor cannot be viewed, its token image opens instead. Other documents use their image when the sheet is unavailable.
- Double-clicking an already open sheet or image popout closes it.
- Buttons, links, inputs, selects, text areas, and damage multiplier buttons are ignored.
- Single clicks are delayed briefly so normal dnd5e click behavior still works when no double-click follows.
- **Enable chat document double-clicks** is a per-user setting and is enabled by default.

### Chat Roll Controls

BM5E extends existing compact D&D5e chat messages without creating replacement roll messages.

Attack and Damage results, including their expandable rolls and application trays, appear inside the originating usage card. Their separate roll messages are hidden from the chat log. This combined layout is bypassed while Midi-QOL is active.

- Click an Attack, Save, or Check dice control to switch the existing roll between Advantage, Normal, and Disadvantage. Advantage keeps the higher stored d20, Disadvantage keeps the lower, and Normal displays only the first die. Missing extra dice are rolled once; stored results survive reloads and can be reused when switching back. Modifiers and critical/fumble thresholds are preserved.
- **Shift-click** a dice control to roll fresh dice using its current Advantage, Normal, or Disadvantage mode, or its current Normal/Critical damage mode. Fresh d20 rolls replace the stored pool; later mode switches use the new results. The normal activity action creates a separate roll.
- Damage and Healing rolls can be rerolled as Normal or Critical.
- Damage rerolls include every damage part produced by the activity.
- Compact save summaries provide a separate reroll control for each target.
- Dice controls update the existing message and retain its initial total. Damage rerolls increment the reroll count; d20 mode changes reuse stored dice and reevaluate success or failure. Attack mode changes also refresh existing damage multipliers per target: hits use 1 and misses use 0.
- Damage rerolls add or remove the Critical property pill to match the selected mode.
- Concentration rerolls retain the concentration DC rather than adopting the originating spell's save DC.
- Reroll controls are available only when the current user can update the roll message.
- **Show chat action labels** adds text labels to compact Attack, Damage, Save, and Check controls for your user. Users without a saved preference inherit the previous world label preference, or enabled when none was saved.
- With labels enabled, these buttons share the available row width; Template and Resource controls remain compact icon buttons.
- **Hide chat-card tags from players** is a world setting, disabled by default. GMs can enable it under **Chat & Rolls** to hide item and activity tags, such as weapon type, range, and proficiency, from players. GMs still see the tags, and changes apply immediately.
- **Compact combined cards** is off by default and applies immediately on this client. Linked attack and damage results share a row. Target pills show hit/miss and expected damage, with multipliers just below. The checkmark beside damage applies it; after a recorded application, it opens **Apply again / Restore** when **Enable damage restoration** is enabled. The amount is separated from the target name for readability.
- Click the chevron on an attack's dotted divider to fold it into a small **Attack 1**, **Attack 2**, etc. row; click the row to expand it. Automatic folding follows **Collapse Trays in Chat**, including collapse after application except in **Never Collapse** mode. Manual choices are remembered on this client.
- Hold **Shift** to show a trash control beside an attack's chevron, whether expanded or collapsed. It deletes that attack and its paired damage together when you have permission to delete both. Separate damage entries retain their own delete control. Deleting a row does not restore damage already applied; use Restore first if needed.
- Clicking the top **Damage** button again creates a separate damage entry without replacing an existing attack/damage pair. Separate damage uses the same right-hand result column and Apply/Restore checkmark. Use the damage row's reroll control to replace that roll. Ambiguous or shared damage stays separate from individual attacks.
- Damage-history tooltips use the corresponding attack numbers and show **-** where damage has not been rolled. Separate damage entries follow afterward. Reroll menus outline the current advantage or critical mode.
- When **Midi-QOL** is active, BM5E bypasses its combined and compact attack-card layout so Midi-QOL can manage its own cards.
- **Show reroll controls** independently enables clickable dice controls and reroll menus for messages your user can update. Changing it refreshes existing cards and popouts without reloading.
- **Show roll results on message buttons** controls Attack/Damage roll-history tooltips and green Save/Check indicators after a visible roll. It is separate from action labels.
- Attack controls indicate hits, misses, critical hits, and fumbles, including after rerolls. Attack and Damage tooltips show numbered totals and outcomes or Normal/Critical damage modes.
- Open combined-card popouts refresh after attack and damage changes, including remote rerolls, while preserving native application trays and weapon mastery links.
- Reroll dice show Automated Conditions 5e's roll attribution tooltip when permitted by its client preference and world visibility filter.

### Auto Roll Damage

When **Auto Roll Damage** is enabled and Midi-QOL is not active, BM5E listens for attack rolls. If the attack succeeds or crits, BM5E clicks the originating card's damage button automatically. If a reroll changes a miss into a hit, BM5E rolls any missing damage for that attack using its original targets, without duplicating existing linked damage.

Clicking Attack or Damage again creates another roll and application tray without a Reroll/Replace dialog. Use the dice-icon controls to reroll a specific existing roll. Button tooltips use `Attack 1: 15 (Hit)` and `Damage 1: 7 (Normal)` entries; critical hits and fumbles retain their respective outcomes.

For non-attack damage activities, BM5E can roll damage after the activity use. Pure damage activities are left to dnd5e's default damage roll behavior.

### Mutable Damage Calculation

Enable **Mutable Damage Calculation** to provide mutable `dnd5e.preCalculateDamage` and `dnd5e.calculateDamage` hook context required by automations such as Arcane Ward, Death Ward, Undying Sentinel, and Undead Fortitude. libWrapper is a required BM5E dependency.

The post-calculation context includes `damageAfterTempHP`, `wouldDropToZero`, `remainingDamage`, `killedOutright`, and `dropToOneHP()`.

```js
Hooks.on('dnd5e.calculateDamage', (_actor, _damages, _options, context) => {
	if (!context?.killedOutright) context?.dropToOneHP?.();
});
```

Pre-calculation hooks can mutate damage traits and modifications for one calculation:

```js
Hooks.on('dnd5e.preCalculateDamage', (actor, _damages, _options, context) => {
	if (!actor.appliedEffects.some((effect) => effect.name === 'Elemental Ward')) return;
	context.modifications.fire = (context.modifications.fire ?? 0) - 2;
	context.traits.dr.value.add('cold');
});
```

### Damage Automations

The **Mutable Damage Automations** multiselect is available when **Mutable Damage Calculation** is enabled. Arcane Ward absorbs eligible damage before zero-HP protections run. BM5E resolves enabled zero-HP protections in this order: Death Ward, Undying Sentinel, then Undead Fortitude. Only one protection resolves for each damage application.

#### Arcane Ward

- Requires an owned item with the `arcane-ward` identifier; its uses track current ward HP, with a maximum of twice Wizard level plus Intelligence modifier.
- In 2014, level 1 or higher Abjuration spells create or replenish the ward, including rituals and slotless casts. The ward absorbs damage before the Wizard's defenses apply to overflow.
- In 2024, an Abjuration spell must consume a spell slot. Creation fills the ward; later casts restore twice the slot level. An activity on the ward that consumes a spell slot can also restore twice that slot level. Resistance and vulnerability apply before absorption, with immunity applied to overflow.
- A Long Rest empties and deactivates the ward.
- Hover a damage target to see ward absorption, remaining damage, and ward HP without changing native token highlighting. Previews refresh after damage rerolls; recorded applications retain their snapshots after reload.
- Restoring an application also restores ward HP spent by it, including applications fully absorbed by the ward.

#### Death Ward

- Recognizes an Active Effect named `Death Ward` or `Protection from Death`.
- Also recognizes an effect whose origin is an item with the `death-ward` identifier.
- Deletes the consumed effect and leaves the actor at 1 HP.

#### Undying Sentinel

- Requires an owned item with the `undying-sentinel` identifier.
- Triggers only when the item and its selected activity have sufficient uses available.
- Leaves the actor at 1 HP, targets the actor's token, and uses the item's Heal activity when available.
- If no uses are available, BM5E continues to the next eligible protection without attempting to use the item.

#### Undead Fortitude

- Requires an owned item with the `undead-fortitude` identifier.
- Does not trigger for Radiant damage or damage from a Critical Hit.
- Rolls a configurable Constitution saving throw against DC 5 plus the damage taken.
- On a success, the actor drops to 1 HP instead of 0 HP.
- Rerolling the save updates its chat message but does not retroactively revise damage already applied.

#### Standard Damage Automations

The **Standard Damage Automations** multiselect works without enabling **Mutable Damage Calculation**. libWrapper remains a required module dependency.

The Elemental Adept integration applies when the caster owns an item with the `elemental-adept` identifier and its name ends with the damage type, such as `Elemental Adept (Fire)`.

- Applies only to spells.
- Ignores matching damage resistance, but not immunity.
- Does not implement the rule that treats matching damage-die results of 1 as 2. That behavior can be configured with Automated Conditions 5e.

The Massive Damage integration marks all three death saving throw failures when damage reduces an actor to 0 HP and the remaining damage equals or exceeds its maximum HP. It runs only when no enabled zero-HP protection leaves the actor at 1 HP.

### Auto Popout

When **Enable use message pop-outs** is enabled, BM5E can pop out chat cards for repeated-use or multi-roll workflows where the original card is likely to be reused.

Open **Auto pop-out use messages** from **Chat & Rolls** to configure item, activity, actor, and compendium UUID defaults. Each entry can restrict matching by item and activity type. Actor UUID, ID, name, and compendium source entries can match activities on that actor.

The same menu configures Extra Attack feature defaults. Matching features cause melee and ranged weapon attacks to pop out. An activity's **Use message pop-out** Behavior checkbox is an explicit override: enabled always pops out, disabled never pops out, and an untouched checkbox uses the configured defaults.

This behavior is skipped when Ready Set Roll is active.

### Effect Application Outcomes

When **Deselect effect targets by roll outcome** is enabled, BM5E removes missed attack targets and successful save targets from the originating message's Effects tray.

After an attack, missed targets receive a zero multiplier in the Damage tray; hits retain the normal multiplier.

### Damage Application Restore

When **Enable damage restoration** is enabled, BM5E records native damage applications and adds RESTORE/RESTORED controls to damage trays. The label changes from APPLY to APPLIED when damage is recorded and returns to APPLY after restoration.

Initially, only Apply is shown. Once an application is recorded, Apply/Applied and Restore/Restored share one row. Restoring an Arcane Ward application also restores the ward HP spent by it.

Players can apply and restore damage only for actors they own, subject to the system's Allow Player Damage Tray setting. The entire tray is hidden when no owned target is available. An active GM records applications on messages the player cannot update; Restore lists only permitted records. Restore appears only after an application has been recorded.

### Effect Application Controls

The usage card's Effects tray changes APPLY to APPLIED when effects are applied. REMOVE deletes newly applied effects or restores modified effects, then changes to REMOVED. Players can remove effects recorded for their owned actors when Allow Player Effects Tray is enabled. An active GM saves effect application and removal records on chat messages the player cannot update; other recipients' records are preserved. Changes to Allow Player Damage Tray or Allow Player Effects Tray refresh existing chat cards immediately.

The tray initially shows only Apply. After an application is recorded, Apply/Applied and Remove/Removed share one row, matching the damage tray.

### Midi-QOL Compatibility

Mutable Damage Calculation preserves healing when Midi-QOL is active. Damage and healing activation triggers follow actual HP changes, including manual card applications; healing and temporary HP gains do not trigger Damaged. Midi-QOL's **Reverse** and **Reverse All** restore HP without firing Damaged, Healed, or outgoing Damages triggers. Applying damage or healing again still triggers the appropriate event.

BM5E's **Auto Roll Damage** does not run while Midi-QOL is active.

### Status HUD Sorting

When **Enable Status HUD Sorting** is enabled, BM5E replaces the token status palette layout with a labeled, sorted grid.

- Status effects are labeled under their icons.
- Cover, bonus action used, and reaction used are pinned first.
- Other statuses sort alphabetically by label.
- Unknown or custom active effects are kept visible and sorted after known statuses.
- Exhaustion uses dnd5e's exhaustion image for the actor's current exhaustion level.
- Concentration and exhaustion keep their dnd5e management behavior.

**Status Effects sorting** controls whether the grid fills by rows or by columns. **Number of columns** controls the grid width. **HUD scale** scales the palette and adjusts against canvas zoom. **Enable status filter** adds a search box, Escape clears or closes the palette, Enter applies the first visible match, and **Clear effects** removes current actor statuses and custom HUD effects.

The status palette is constrained to the visible viewport and gains vertical scrolling when its configured rows, columns, scale, or current canvas zoom would otherwise place it off-screen.

### Prone Mechanics

When **Implement Prone Mechanics** is enabled, applying Prone selects Crawl for the actor's tokens. Removing Prone restores each token's previous movement action when available, or selects its fastest available movement type. Clicking an active Prone icon on a token stands it up and records half its walking speed in the current combat turn's movement history. The creature must have enough movement remaining. Set the actor flag `flags.bm5e.standingCost` to override that cost in grid spaces; for example, `2` costs 10 feet on a 5-foot grid.

Prone movement updates skip tokens already crawling and guard overlapping updates so duplicate processing does not overwrite the saved movement action. With multiple enabled Active Effects granting Prone, movement is restored after the last is removed. Restoration avoids unchanged updates when no saved movement flag needs clearing.

### Grapple Mechanics

When **Implement Grapple Mechanics** is enabled, actor owners and the active GM can click a token's Grappled icon to start an escape. The rolling user chooses **Acrobatics | Athletics**; closing the dialog with X cancels. Players roll their own escape locally. When the GM starts an escape for an actor with an active player owner, BM5E sends that owner a chat request directly; otherwise, the GM rolls locally. Spending the action remains manual, and the active GM must be connected to resolve escapes.

Under the 2014 rules, escape requires beating the grappler's contested Athletics check. The active GM sends the opposing roll to the grappler's designated active player owner, or rolls locally if none is active. Under the 2024 rules, escape rolls against the originating activity's escape DC, falling back to the grappler's Strength DC. If no DC can be resolved, including for manually assigned Grappled statuses, BM5E queries the active GM with a DC-only dialog. The GM's DC is saved on the effect; afterward, the player chooses their escape skill and whether to roll. Cancelling the GM prompt stops the attempt. GM requests for multiple Grappled effects produce a separate request for each effect.

The GM's DC prompt has **Set** beside the number field, which saves and closes without requesting an escape. **Request Escape** saves a valid DC and continues the attempt, sending a player-owned actor's roll request to its active player owner. Missing or invalid DCs leave the dialog open. The window's X closes without requesting an escape. The active GM can right-click the token's Grappled icon to reopen this dialog with the current DC; if there are multiple grapples, select the effect to edit first. The GM does not choose the player's skill.

Escape rolls retain the effect UUID and an attempt ID, directly or through their system request. The active GM reevaluates failed attempts when either roll is rerolled and removes the associated effect on success. A deleted effect is not recreated by a later reroll, and previously applied HP or effects are not reversed. Activity-applied Grappled effects retain their source actor, tokens, icon path, range, and DC. Under the 2024 rules, the effect also ends when the grappler becomes Incapacitated or either token moves beyond that range.

### Falling Mechanics

GMs can right-click a token's Falling icon to land harmlessly at the system's supporting elevation and level, without a damage card or animation. Left-click retains the system's normal falling action. Enable or disable this behavior with **Harmless Falling on Right-click** in Conditions & Effects.

**Re-evaluate Falling when the active scene loads** optionally reevaluates shared Falling status for linked actors on the active scene when the active GM loads it. It uses the system's last-moved token on that scene, or the first linked token when needed. It does not reconcile multiple linked tokens on the same scene beyond the system's behavior. If D&D5e's **Disable Falling** is enabled, scene activation performs no checks, status changes, or last-moved token updates. The setting is disabled by default.

### Overtime Debug Logging

**Overtime Debug Logging** enables BM5E overtime debug logs. It can be combined with channel flags through `globalThis.bm5e.debug`.

## Activation Triggers

The **BM5E Triggers** activity activation type runs an activity in response to incoming or outgoing damage, healing, attacks, movement, or tokens within an emanation.

Open the trigger editor from the activity's Activation Cost controls, then configure:

- **Event**: Damaged, Damages, Healed, Attacked, Attacks, Movement, or In Range. Forced Movement is available only while BAA5E is active.
- **Actor HP** and **Opponent HP**: independent optional comparisons of current HP to maximum HP, excluding temporary HP. Actor means the triggered activity owner; opponent means each creature involved in the event. Movement has no opponent. Existing HP filters retain their original subject.
- **Opponent Sizes** and **Opponent Types**: optional per-opponent filters. Only matching opponents are included when an event involves several creatures.
- **Attack Outcome**: All, Hit, or Miss for Attacks, evaluated per target; unknown outcomes match only All.
- **Damage Types** or **Healing Types**: optional type filters for the corresponding events, including damage-type filtering on attack events.
- **Source Item Types**: an optional filter for the item that produced a damage, healing, or attack event.
- **Automatically target the other creature**: receiver events target the source creature, source events target the affected creature, and Movement or In Range targets the triggering token.
- **Bypass configuration dialog**: skips activity configuration choices.
- **Automatically roll activity**: immediately uses the activity after creating its Triggered chat message. When disabled, the message waits for user input.

The editor groups Event under **Trigger**, activity options under **Behaviours**, and optional filters under **Toggle Filters**. Filter and reduction toggles retain their configured values when disabled.

All enabled filters must match (**AND**). Multiple choices within one filter match any selected value (**OR**). For example, selecting Tiny and Small sizes plus Opponent HP at or below 50% triggers only for a Tiny or Small opponent whose HP is at or below 50%. Disabled filters do not restrict triggers, and their saved choices return when re-enabled.

**In Range** configures the activity's template as a token-attached radius emanation. Select source or in-range token turn starts and ends or tokens entering and leaving, set the triggers allowed for each token per turn, filter by creature size or type, and optionally include the source token. The activity's template size and units define the radius; its Range field does not. The Affects choice supplies the ally or enemy filter. Use the activity to place its emanation region. Movement events track tokens moving into or out of the region; moving the emanation over a stationary token does not itself fire those events.

**Triggers per Token/Turn** is shared across the selected timings during each combat turn, including encounters not linked to a scene. Creating a trigger offer uses that allowance even if the activity is not used. Outside combat, this per-turn limit does not apply.

**Total triggers before removal** deletes the emanation after the configured number of trigger offers across all tokens and turns, including outside combat. Set it to **0** for unlimited triggers. Each eligible token counts separately; filtered or unavailable activities do not count. The total persists on the emanation, and processing stops when the limit is reached.

Trigger offers and automatic selections check configured consumption using native preparation on an item clone. Unavailable activities are omitted without spending resources. The system validates consumption again when the activity is used.

Triggers respond to native D&D5e damage application and direct Actor HP updates without firing twice for the same change. Matching activities appear in an identifiable Triggered chat message both during and outside combat.

Movement triggers expose the completed movement distance as `@bm5e.distanceTraveled`, the scene's grid distance as `@bm5e.gridDistance`, and the floored number of grid intervals moved as `@bm5e.gridDistanceTraveled`. Distance values use the scene's units and include all completed movement segments. For example, `(@bm5e.gridDistanceTraveled)d4` rolls one d4 per complete grid interval moved.

Forced Movement additionally requires the movement update option `{ bm5eMovementSource: 'forced' }`. Ordinary movement does not fire it. Without active BAA5E, the event is hidden for new configurations, existing Forced Movement selections are disabled and preserved, and no Forced Movement trigger fires.

### Damage Trigger Values

Damage triggers expose the following roll-data paths for each opponent. Use the exact paths, including the `@bm5e.` prefix, in formulas:

- `@bm5e.damageRolled.total`: rolled damage before target defenses.
- `@bm5e.damageIncoming.total`: incoming calculated damage before trigger reductions.
- `@bm5e.damageDealt.total`: final calculated damage after reductions.
- `@bm5e.damageApplied.total`: actual HP and temporary HP lost, capped by what the target had.
- `@bm5e.damageApplied.hpLost`: actual regular HP lost.
- `@bm5e.damageApplied.tempHpLost`: actual temporary HP lost.

The first three maps also expose damage-type values, for example `@bm5e.damageRolled.fire`, `@bm5e.damageIncoming.fire`, and `@bm5e.damageDealt.fire`. Applied damage exposes the HP breakdown above rather than damage-type values. Applied losses are available after damage application, not while calculating an incoming reduction. Trigger and usage messages retain their damage context after reload. Opponent filters are evaluated separately for each creature involved.

Per-damage-type formula examples:

- `floor(@bm5e.damageIncoming.fire / 2)`: half the incoming Fire damage, rounded down. In **Reduce Incoming Damage**, select only Fire so the reduction applies to that type.
- `@bm5e.damageDealt.necrotic + @bm5e.damageDealt.cold`: the final calculated Necrotic and Cold damage combined, for a triggered activity's formula.

Damage-type keys use system identifiers such as `fire`, `cold`, and `necrotic`. Final calculated damage can exceed the HP actually lost; per-type values do not provide a breakdown of applied HP loss.

**Reduce Incoming Damage** on a Damaged trigger supports formulas such as `min(10, ceil(@bm5e.damageIncoming.total / 2))` for Ghostly Command. This requires Mutable Damage Calculation. With Auto Roll disabled, you are prompted before using the activity; declining or having insufficient configured resources leaves damage unchanged. Healing does not trigger the reduction. Selected damage types share one reduction budget; no types selected means all damage types.

The **Ghostly Command** example is available in the **BM5E Overtime Test Items** compendium. Its description covers BM5E configuration only. The Weapon and Spell source filter also accepts non-attack weapon damage; reaction costs and timing are not enforced automatically.

## Overtime Effects

BM5E overtime effects create activities that trigger automatically during combat or encounter timing.

Create an Applied Active Effect on an Activity, then add a BM5E change with this key:

```text
flags.bm5e.overtime
```

When the change type is **BM5E**, the editor uses that key automatically.

D&D5e displays this key as **Overtime**. Selecting another change type clears an automatically populated overtime key, and the BM5E change-summary icon opens the corresponding overtime editor directly.

### Activity Source

The value determines what activity BM5E should roll:

- `action=pseudo` creates a generated activity from the serialized fields.
- `action=link` with `link=ACTIVITY_UUID` reuses an existing activity.
- Created editor activities are stored on the actor's hidden overtime item.
- An item-owned activity with the **BM5E Overtime** activation cost is a standalone overtime activity. Its editor stores configuration directly on that activity.

Supported pseudo activity types:

- `pseudoType=save`
- `pseudoType=check`
- `pseudoType=damage`
- `pseudoType=heal`

Compatibility keys such as `flags.bm5e.overtime.check` are still parsed, but new effects should use `flags.bm5e.overtime`.

### Timing

Use `turn=VALUE`.

Supported values:

- `turnStart`
- `turnEnd`
- `sourceTurnStart`
- `sourceTurnEnd`
- `eachTurnStart`
- `eachTurnEnd`
- `roundStart`
- `roundEnd`
- `encounter`
- `encounterEnd`

Aliases include `start`, `end`, `sourceStart`, `sourceEnd`, `eachStart`, `eachEnd`, `combatStart`, and `combatEnd`.

### Roll Fields

- `name=TEXT` overrides the generated activity name.
- `ability=str` sets the primary ability.
- `abilities=str|dex` allows multiple save or check abilities.
- `skills=ath|acr` allows associated skills for checks.
- `tools=...` allows associated tools for checks.
- `dc=15` sets the save or check DC.
- `damage=2d6` sets damage.
- `damageType=fire` sets the damage type.
- `critical=true` enables critical damage handling.
- `criticalDamage=FORMULA` sets critical damage.
- `heal=1d8` sets healing.
- `healType=healing` or `healType=temphp` sets healing type.
- `onSave=half`, `onSave=full`, or `onSave=none` controls save damage.

### Outcome Fields

Outcomes can apply status effects, apply linked activity effects, or remove the overtime effect.

- `onEachSuccess=prone` applies on every success.
- `onEachFailure=poisoned` applies on every failure.
- `overallSuccessTarget=2` counts total successes.
- `overallFailureTarget=2` counts total failures.
- `onSuccess=removeOvertime` applies when the overall success target is met.
- `onFailure=prone` applies when the overall failure target is met.
- `consecutiveSuccessTarget=2` counts consecutive successes.
- `consecutiveFailureTarget=2` counts consecutive failures.
- `onConsecutiveSuccess=removeOvertime` applies when the consecutive success target is met.
- `onConsecutiveFailure=prone` applies when the consecutive failure target is met.
- `triggerTarget=3` counts non-save/check triggers.
- `onEachTrigger=prone` applies on every trigger.
- `onFinalTrigger=removeOvertime` applies when the trigger target is met.
- `persistent=true` keeps pending overtime resolutions from resolving at combat end.

`remove`, `delete`, and `removeOvertime` remove the overtime effect. Status IDs such as `prone` apply that status with `active: true`. `exhaustion` adds one exhaustion level, up to the D&D5e configured maximum. Linked activity effect IDs apply the matching embedded activity effect to the actor.

### Linked Save and Check DCs

Linked Save and Check activities can use **Preserve source DC**. When enabled, BM5E resolves the source activity's ability, spellcasting, class-source spellcasting, formula, bonus, and scaling data before creating the target overtime activity, then stores the resulting DC as a custom value. Disable it to resolve the DC from the target actor instead.

Compatibility aliases for previous BM5E versions include `overallFailures`, `failures`, `saveFailures`, `saveFails`, `overallSuccesses`, `successes`, `saveSuccesses`, `saveSuccess`, `consecutiveFailures`, `successiveFailures`, `successiveFails`, `consecutiveSuccesses`, `successiveSuccesses`, `successiveSaves`, `onFail`, `onSaveFailure`, `onSaveFail`, `onSaveSuccess`, `onEachFail`, and `triggers`.

### Examples

Check with consecutive outcomes:

```text
action=pseudo; pseudoType=check; turn=turnStart; abilities=str; skills=ath|acr; dc=15; onConsecutiveSuccess=removeOvertime; onConsecutiveFailure=prone; consecutiveSuccessTarget=2; consecutiveFailureTarget=2
```

Save with each-failure and overall success:

```text
action=pseudo; pseudoType=save; turn=turnEnd; abilities=con|wis; dc=13; onEachFailure=poisoned; overallSuccessTarget=1; onSuccess=removeOvertime
```

Linked activity (replace `ACTIVITY_UUID` with the UUID of the activity to use):

```text
action=link; link=ACTIVITY_UUID; turn=turnStart; onFinalTrigger=removeOvertime; triggerTarget=3
```

At the start and end of a creature's turn, if one or more overtime effects are present on the actor, BM5E creates a chat message showing the available overtime rolls. If Midi-QOL is active and Activation Cost Automation is set to Auto Trigger, these overtime effects are rolled automatically.

## Enchantments

BM5E extends dnd5e enchantment activity cards so the user can choose a target and item directly from the chat card.

- Enable these additions with **Enable enchantment handling** in **Conditions & Effects**. This setting gates compact target selection, ownership controls, preferred-item matching, sequential recipient pickers, grouped GM requests, application limits, and Item (Actor) labels.
- Targeted tokens are shown on the enchantment card. Compact cards use an icon-style Targeted/Selected toggle and compact target rows.
- Enchantment trays are visible to GMs, source actor owners, and owners of targets recorded on the message. Players who only own a recorded target see only their own recipients. Message authorship or locally selected tokens do not grant access.
- Without recorded targets, only GMs and source actor owners see the tray; they can use the currently selected token as the recipient.
- Clicking the enchantment drop area processes all checked recipients sequentially, with one compatible-item picker open at a time. Each picker names its actor; Apply continues, Skip omits that actor, and Cancel Remaining (or closing the picker) stops the batch.
- **Preferred item identifier** on the Enchant activity automatically selects a unique compatible match on the chosen actor. Multiple matches appear first in the picker; no match or a blank preference retains normal selection. The same preference applies to GM requests. Dragging another compatible item onto the card remains allowed; self-enchantments retain their normal destination.
- The activity target count limits distinct recipients per use; Maximum Enchanted Items still limits distinct items across the activity (default 1, so increase it or use 0 for multiple recipients). Applied entries show Item (Actor) and remain tracked by native enchantment effects after reload.
- If the user does not own the target actor, BM5E whispers a GM request with an **Apply Enchantment** button.
- A single GM request keeps the original activity, recipient actors, source message, and enchantment profile, and processes its recipients sequentially. Cancelling preserves the remaining recipients in the request. Older single-recipient requests remain supported.
- BM5E passes the source chat message and concentration effect through to dnd5e's enchantment application.
- By default, a matching enchantment from the same activity replaces the previous matching enchantment on that item.
- **Allow Enchantment Stacks** permits repeated enchantments from the same activity on one item.
- **Maximum Enchanted Items** limits how many different items an activity can enchant at once. Use `0` for no limit.
