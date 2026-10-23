## v14.606.3

- **First release through Foundry VTT's Patreon integration and Protected Content system.**
  - Link an eligible Patreon account to your Foundry account to install and receive updates through Foundry's package installer.
  - If your membership includes BM5E but you cannot install or update it, contact me through Patreon or Discord for help.
- **Auto Roll Damage** now offers **Disabled**, **When the attack hits**, and **Always**. Existing enabled preferences retain hit-only behavior. Save activities continue opening the damage dialog on use in either enabled mode.
- **Item properties:** Added an optional world setting for **Wooden**, **Metal**, **Glass**, and **Crystal** properties on weapons and equipment. Materials are assigned manually; disabled by default.
- Enchantments:
  - Added multiple preferred item identifiers and `*` wildcards, such as `shield*`. Wildcard pickers show compatible matches; **Apply to all matching items** applies those matches within the configured limits. Blank identifiers or no matches use the normal picker.
  - Item pickers open beside the clicked chat message and explain when no matching identifiers were found.
  - Added **Maximum enchanted items per use**, separate from the overall limit. Set it to **1** for one application per use, including repeated stacks; **0** means unlimited. Trays retain active enchantments from earlier uses.
  - Clearing **Maximum enchanted items** restores its default of **1**; an explicit **0** means unlimited.
  - Self-enchanting activities show **(Self-enchant)** in the chat-card header. Irrelevant limits and matching options are hidden and ignored, preserving their saved values for switching back. Stacking remains available.
  - Row undo controls support **Shift+Click** to undo all enchantments shown and **Ctrl+Shift+Click** to undo all enchantments shown for that item, including earlier stacks and subject to item ownership. Shortcut tooltips appear when more than one enchantment is shown. Removed the separate Undo All button to preserve target layout.
  - Grouped enchantment options under a collapsible **Bugbear's Mechanics** fieldset, with shorter hints, detailed hover tooltips, and clickable labels that focus fields or toggle checkboxes.
  - **Valid Properties** restrictions are available with **Any** item type, allowing one activity to affect matching weapons and equipment. A **(BM5e)** tooltip identifies the addition; native validation remains in use.

## v14.606.2.1 - Hotfix for BM5e v14.606.2

- Improved compatibility between document double-clicks and **Midi-QOL** cards: item descriptions expand normally, and single clicks retain their original target and modifier keys.
- **Hide chat-card tags for** now lets GMs select **GMs**, **Players**, or both. Leave it empty to show tags to everyone. Existing player-only preferences are preserved.

## v14.606.2 - Final Release Outside Foundry Patreon Integration

- Distribution and licensing
  - Final release using the existing distribution method. Future releases will use Foundry VTT's Patreon integration and Protected Content system.
  - Updated license: previously obtained versions remain usable after a subscription ends; further updates require an active entitlement.
  - Commercial games and streams are permitted with module credit and a Patreon link.
- Chat cards and rolls
  - Added **Compact combined cards**, an optional setting for each client. Linked attack and damage results share a row, with target pills showing hit/miss and expected damage. Compact multipliers and an Apply checkmark keep controls close to the results; after a recorded application, the checkmark offers **Apply again / Restore** when damage restoration is enabled.
  - Hold **Shift** to reveal a trash control beside a compact attack's chevron. Deleting the row removes that attack and its paired damage together, subject to your permissions. Deleting a row does not undo damage already applied.
  - Added **Hide chat-card tags from players**, a world setting under **Chat & Rolls**, disabled by default. Enable it to hide item and activity tags from players while keeping them visible to GMs; changes apply immediately.
  - Fixed combined chat cards so rerolling an attack updates only its associated damage, preserving each target's hit or miss. Ambiguous older damage remains unlinked.
  - Fold individual attacks with the chevron on their dotted divider. Collapsed rows show **Attack 1**, **Attack 2**, etc. Automatic folding follows the system's **Collapse Trays in Chat** setting, and your manual choices are remembered on this client.
  - Extra rolls from the Damage button keep existing attack/damage pairs intact and appear as separate compact damage entries. Use a damage row's reroll control to replace its roll. Damage-history tooltips follow attack numbering and show **-** for attacks without damage.
  - Reroll menus outline the current advantage or critical mode. Attack-history tooltips indent target outcomes for easier reading.
  - Combined and compact attack cards are bypassed when **Midi-QOL** is active, leaving its card layout in control.
  - **Auto Roll Damage** now rolls missing damage when a rerolled attack changes from a miss to a hit, using that attack's original targets without duplicating existing damage.
- Updated pt_BR translation by [Kharmans](https://github.com/Kharmans) 🤗

## v14.606.1

- Compatibility and settings
  - Updated compatibility for D&D5e 6.0.6.
  - Grouped settings into **Chat & Rolls**, **Token HUD**, **Damage Mechanics**, **Conditions & Effects**, and **Troubleshooting**. Menus save changes immediately; close them with the window's X. Existing preferences are preserved.
  - All Chat & Rolls preferences are now per-user, without a GM master switch or reload requirement. Label, reroll-control, and result-display changes refresh existing chat cards and popouts. Users without a saved label preference inherit the previous world setting.
  - Token HUD settings update an open HUD immediately and preserve unrelated effect changes.
- Chat cards and rolls
  - Attack, Save, and Check Rerolls switch between **Advantage / Normal / Disadvantage** while retaining the rolled d20 results, including after reload. Extra dice are rolled only when needed; Normal shows the first die and keeps the others available for later switches.
  - **Shift-click** a dice-icon reroll control to roll fresh dice using the current advantage mode, or the current Normal/Critical damage mode. Fresh d20 rolls replace the stored pool without applying advantage twice. Controls remain limited to messages you can update.
  - Rerolls preserve modifiers, critical/fumble thresholds, and the original total in their history. Concentration rerolls retain the concentration DC instead of using the originating spell's save DC.
  - Attack rerolls refresh hit/miss colors, tooltips, and existing per-target damage multipliers, including after reload. Save and Check labels turn green once a visible roll has been made, when result display is enabled.
  - Attack and Damage tooltips show numbered roll history, per-target outcomes, and damage modes. Replacing a roll updates its existing entry. Clicking Damage creates a new roll without a Reroll/Replace dialog; dice-icon controls update existing rolls.
  - Combined chat-card popouts refresh when associated attack or damage messages change, including rerolls from other clients. Individual popouts also rebuild their damage and effect target trays correctly.
  - Preserved AC5e tooltip visibility preferences and native weapon mastery links used by WM5e.
  - Added item-icon double-click support to Triggered and Overtime activity lists.
- Damage and effect application
  - Players can record and restore damage, and record effect Apply/Remove actions, on another user's chat cards through the active GM. Records are limited to owned actors and preserve other recipients' entries.
  - Damage and effect controls respect the system's player-tray settings and actor ownership. Damage Apply is hidden when there are no owned targets; Restore lists only permitted records. Changing player-tray settings refreshes existing cards immediately.
  - Fixed damage-tray crashes when mutable calculations return damage parts without trait metadata, including parts added by Midi-QOL.
  - Fixed healing dealing damage with **Midi-QOL** when Mutable Damage Calculation is enabled. Native healing, defenses, and multipliers are processed before Midi adjustments, including its noCalc path.
  - Midi damage/healing triggers now follow actual HP changes, including manual card applications. Healing and temporary HP gains do not trigger Damaged. **Reverse / Reverse All** restore HP without firing Damaged, Healed, or outgoing Damages triggers; normal reapplications still trigger.
- Arcane Ward
  - Added **2014 and 2024 Arcane Ward** support. An owned item with the identifier `arcane-ward` displays current/maximum ward HP and resets after a Long Rest.
  - **2014:** level 1+ Abjuration spells create or replenish the ward, including rituals and slotless casts. The ward absorbs damage before the Wizard's defenses apply to overflow.
  - **2024:** Abjuration spells replenish the ward when a spell slot is consumed; its Bonus Action activity can also consume a slot to replenish it. Resistance and vulnerability apply before absorption; immunity applies to overflow. Slot tracking includes Mage Armor casts.
  - Hover previews show each target's absorption, remaining damage, and ward HP while preserving native token highlighting. Previews refresh after rerolls and rebuild after reload.
  - Restore returns the ward HP spent by that application, even when the ward absorbed all damage. Application records survive reload and remain available after damage rerolls; Restore appears only after an application is recorded.
- Activation triggers
  - Added outgoing **Damages** and **Attacks** events, outgoing damage-type filters, and source item-type filters for damage, healing, and attack events.
  - Added **In Range** emanations that trigger on source/in-range token turn starts or ends, or when tokens move in or out. Target filters and per-token turn limits apply, including in encounters not linked to a scene.
  - In Range's **Total triggers before removal** deletes the emanation after a configured number of trigger offers across all tokens and turns. Zero means unlimited; filtered or unavailable activities do not count. The counter persists, and the editor uses compact number fields and keeps changes in sync with the current activity.
  - Reorganized the editor into Trigger, Behaviours, and Toggle Filters. Disabled filters and damage reductions retain their configured values. Actor/opponent HP filters are independent, size/type filters apply per opponent, and attack events support damage-type filtering. All enabled filters must match; multiple choices within one filter match any selected value. Existing HP filters retain their original subject.
  - Added an **All / Hit / Miss** filter for outgoing attacks, evaluated per target. Existing triggers default to All; unknown outcomes do not match Hit or Miss.
  - Trigger offers, automatic selections, and incoming damage-reduction prompts skip activities whose configured resources are unavailable, without spending resources during the check. Actual use still validates consumption.
  - Added trigger roll data: `@bm5e.damageRolled.total` (before defenses), `@bm5e.damageIncoming.total` (before trigger reductions), `@bm5e.damageDealt.total` (after reductions), and `@bm5e.damageApplied.total` (actual HP + temporary HP lost). The first three also expose damage-type values, such as `@bm5e.damageIncoming.fire` and `@bm5e.damageDealt.necrotic`; `@bm5e.damageApplied.hpLost` and `@bm5e.damageApplied.tempHpLost` separate the applied losses. Damage context remains available on trigger and usage messages after reload. The README includes per-type formula examples.
  - Added **Ghostly Command** to the Overtime Test Items compendium: a prompted reduction of half the incoming damage, rounded up, capped at 10. Requires Mutable Damage Calculation and filters for Weapon/Spell sources. Declining or lacking resources leaves damage unchanged; healing does not trigger it. Reaction costs and timing remain manual.
  - **Forced Movement** is available only with BAA5E active and requires an explicit forced-movement marker. Existing configurations remain disabled without BAA5E; ordinary movement cannot trigger this event on its own.
- Enchantments
  - Checked recipients are processed sequentially with actor-labelled **Apply / Skip / Cancel Remaining** choices, including GM requests. Recipient counts and enchanted-item limits remain enforced; applied entries show Item (Actor).
  - Added an optional **Preferred item identifier**. A unique compatible match is selected automatically; multiple matches are prioritized in the picker. Normal selection and manual item drops remain available.
  - Trays are visible to GMs, source-actor owners, and owners of recorded targets. Target owners see only their own recipients. Without recorded targets, only GMs and source owners have access; another client's selected token or message authorship does not grant access.
  - Compact cards use the native icon-style Targeted/Selected toggle and compact target rows. These features use the existing **Enable enchantment handling** setting.
- Grapple escapes
  - Added optional **Grapple mechanics**. Click an owned actor's Grappled token icon to attempt a 2014 contested or 2024 DC-based escape. The active GM handles opposing rolls and effect removal; spending the action remains manual. In 2024, grapples also end when the grappler becomes Incapacitated or the tokens move out of range.
  - When an escape DC cannot be resolved, the active GM receives a DC-only prompt before the player chooses Acrobatics or Athletics. **Set** saves the DC and closes without requesting a roll; **Request Escape** saves it and continues. Invalid DCs keep the dialog open; X cancels. Concurrent requests for the same effect share one prompt.
  - The saved DC is reused for later attempts. The active GM can right-click Grappled to edit it.
  - Active player owners receive escape requests in chat and choose **Acrobatics / Athletics**. The GM rolls locally when no player owner is active. Closing a roll-choice dialog cancels the attempt.
  - Escape checks retain their effect and attempt references. Rerolling either check reevaluates an unresolved escape without recreating deleted effects or undoing previously applied changes. The active GM must be connected.
  - Grappled effects retain their source and icon, including when applied by activities. Grapple tracking avoids scanning every scene on movement.
- Prone and Falling
  - Prone and Grappled token-icon controls work on the initial scene draw after reload.
  - Prone movement handling avoids duplicate updates and preserves the previous movement action when a token is already crawling.
  - GMs can right-click Falling to land harmlessly at the system's landing elevation and level, without a damage card. Left-click still plummets; harmless landing currently has no animation.
  - Added an optional setting to refresh linked actors' shared Falling status when a scene loads. The system's **Disable Falling** setting prevents these checks and related updates.

## v14.603.3

- Combined weapon Use, Attack and Damage rolls with their application trays in the originating use chat card, hiding the separate roll messages.
- Updated damage multipliers and effect application targets per attack outcome: missed targets receive no damage and are deselected for effects.
- Fixed damage restoration so APPLIED returns to APPLY after RESTORE, and added APPLY/APPLIED and REMOVE/REMOVED controls for effect application.
- Preserved Automated Conditions 5e roll attribution tooltips on BM5E reroll dice controls.
- Fixed chat target double-clicks to open the target actor sheet when permitted or its token image otherwise, and removed attacker-token hover and double-click behavior from empty roll summary areas.
- Renamed the Prone setting to **Implement Prone Mechanics**. Applying Prone selects Crawl; removing it restores each token's previous movement action or selects its fastest available action. Clicking the Prone icon still spends the required movement to stand.
- Fixed Status HUD Clear Effects to remove active status effects through the D&D5e v6 API.

## v14.603.2

- Added the **Automate Standing from Prone (RAW)** world setting. Interactive Prone status icons stand the token up only when it has enough movement remaining and record half its walking speed in combat movement history;
  - `flags.bm5e.standingCost` on an Actor can override the cost in grid spaces (for example, `2` costs 10 feet on a 5-foot grid).
- Fixed Status HUD exhaustion icons for the D&D5e v6 API.
- Made libWrapper a required dependency and removed obsolete optional fallback handling.

## v14.603.1

- Expanded BM5e Activation Triggers with Attacked and Movement events.
- Added independent trigger options to automatically target the triggering creature, bypass activity configuration, and automatically roll the activity.
- Triggered activities now create identifiable chat messages both during and outside combat, and can wait for user input before rolling.
- Limited the Activation Triggers editor to one window per activity.
- Preserved the triggering target across triggered Attack, Damage, Healing, Save, and Check activities.
- Exposed completed Movement-trigger distance, scene grid distance, and floored grid intervals traveled to roll formulas as `@bm5e.distanceTraveled`, `@bm5e.gridDistance`, and `@bm5e.gridDistanceTraveled`.
- Added Massive Damage as a standard damage automation. It applies only when no zero-HP mutable automation leaves the actor at 1 HP.
- Renamed the damage integration settings to Standard Damage Automations and Mutable Damage Automations and sorted their options by name.

## v14.602.1

- Updated BM5e for D&D5e v6.0 and dropped v5.x compatibility.
  - Updated chat-message, target, activity, damage, healing, enchantment, and overtime handling for the D&D5e 6 data model.
  - Updated Active Effect changes to use `effect.system.changes`, stable change IDs, and the new `EffectChangeConfig` application.
- Fixed hot-reload registration issues.
- Added roll controls to existing compact system chat messages:
  - Attack, save, and check rolls can be rerolled with Advantage, Normal, or Disadvantage.
  - Damage and healing rolls can be rerolled as Normal or Critical, including every damage part belonging to an activity.
  - Compact save summaries provide a reroll control for each target.
  - Rerolls update the existing message, retain the initial total and target DC, track reroll counts, and refresh success or failure.
- Added labels to compact chat-card Attack, Damage, Save, and Check controls.
- Added hit/miss coloring for attack controls and Advantage/Critical coloring for reroll options.
- Relinked configurable chat document double-click handling for actor names, target pills, item icons, activity icons, and effect icons, with image fallback when the user cannot open the document.
- Added a configurable chat document double-click handling for the chat message flavor text, with image fallback when the user cannot open the document.
- Added the BM5e Triggers activity activation type and editor for Damaged and Healed events, resulting HP percentage conditions, and damage or healing type filters. Triggered activities use identifiable turn-style chat messages in combat and execute directly outside combat.
- Added integrated mutable-damage automations for Death Ward, Undying Sentinel, and Undead Fortitude (select the ones you want in the module settings). When multiple protections are present, BM5e resolves Death Ward, then Undying Sentinel, then Undead Fortitude. Undying Sentinel triggers only while its item has uses available.
- Added an independent Integrated Damage Modifiers multiselect, currently offering Elemental Adept. Matching spell damage ignores resistance when the caster owns an `elemental-adept` item named with its damage type, such as `Elemental Adept (Fire)`.
- Updated the BM5e Active Effect value-editor controls to be compatible with the new system change editor, including direct access from BM5e change summary icons.
- Displayed the `flags.bm5e.overtime` attribute as `Overtime` and cleared that automatically populated key when another change type is selected.
- Matched the damage RESTORE button to the compact system APPLY button.
- Deferred save-based damage multipliers to the native system damage application tray.
- Kept detailed Attack and Damage roll results in button tooltips to avoid crowding compact chat cards.
- Deferred concentration and exhaustion condition management to the system and constrained the Status HUD palette to the viewport while preserving HUD scaling.
- Replaced BM5e prototype bookkeeping properties with private WeakMap state without changing libWrapper registrations or fallback method replacements.

- Undead Fortitude save rerolls update the save message but do not retroactively revise damage that has already been applied.

## v14.533.4

- Activities that trigger when their actor takes damage now create their card correctly during combat.
- The activity BM5E Editor now properly shows only when the activity cost is set to Overtime.
- Updated compatibility for Foundry VTT v14.366.

## v14.533.3

- Overtime activities can be marked magical; linked activities inherit their source Item's magical property and can toggle it per overtime activity.
  - To be properly identified by Automated Conditions 5e, make sure you are on v14.533.15 or later of that module.

## v14.533.2

- Assign Status Effects Sorter scaling is now optional
- Added missing packs

## v14.533.1

- Added configurable use-message pop-out defaults:
  - Configure item, activity, actor, and Extra Attack identifiers from the module settings.
  - Import, export, and restore the default identifier configuration.
  - Enable use-message pop-outs per activity from its Behavior settings.
- Added optional enchantment handling:
  - Enable BM5E enchantment behavior from the module settings.
  - Configure stackable enchantments and the maximum number of different enchanted items per activity.
  - Added safer authorization checks for self-enchant and targeted enchantment cards.
- Expanded overtime support:
  - Activities can use the BM5E Overtime activation cost and save their configuration directly on the activity.
  - Overtime attack activities now resolve success and failure outcomes from hit and miss results.
  - Standalone overtime activities can trigger from combat turn messages.
  - Linked Save and Check activities can preserve their source DC, including ability, spellcasting, class-source, custom-formula, bonus, and scaling data.
  - Overtime outcomes that apply Exhaustion now increase the target's exhaustion level, up to the configured D&D5e maximum.
- Added optional effect-application outcome handling to keep effect targets aligned with attack misses and save outcomes.
- Added optional damage-application restoration with APPLY/APPLIED and RESTORE/RESTORED controls.
- Added an optional temporary libWrapper override for mutable D&D 5e damage-calculation hooks.
- Fixed Status HUD layout when filtering is disabled.
- Added Status HUD scaling with canvas zoom compensation.
- Added a one-time repair for BM5E compendium folder assignments cached by Foundry.
- Updated the in-app README introduction and module-settings documentation.

## v13.533.4

- Expanded BM5e overtime activity generation:
  - Overtime feature items are now labeled per actor/token, for example `Randal Overtime Effects`.
  - Generated overtime activities add the `name=` entry to the activity chat flavor.
  - Multi-ability overtime saves/checks accept `ability=str||dex`, `ability=str || dex`, or `ability=str | dex | con`.
- Added separate overtime result counters for total and consecutive roll outcomes:
  - Total removals: `overallSuccesses=3; onSuccess=remove` or `overallFailures=3; onFailure=remove`.
  - Consecutive removals: `consecutiveSuccesses=3; onSuccess=remove` or `consecutiveFailures=3; onFailure=remove`.
  - Per-roll status actions: `onEachSuccess=prone` or `onEachFailure=prone`.
  - Compatibility aliases remain supported: `saveSuccesses`, `saveSuccess`, `successes`, `saveFailures`, `saveFails`, `failures`, `successiveSuccesses`, `successiveSaves`, `successiveFailures`, and `successiveFails`.
- Overtime chat cards now select the associated token before non-midi-qol rolls so follow-up roll and damage actions use the same token; this initial implementation intentionally leaves that token selected.
- Added Status HUD scaling with canvas zoom compensation.
- Added a Module Support settings menu with README, issue tracker, Discord, Ko-Fi, and Patreon links.
- Improved enchantment chat card rendering after reload by using the D&D5e chat render hook surface.
- Hardened enchantment target selection so clicks use the target tray from the rendered card that was clicked.

## v13.533.3

- Removed leftover overtime debug `console.log` calls from activity creation and roll handling.
- Fixed token HUD exhaustion interaction handling to prevent duplicate processing from rapid/secondary click events.
- Added `auxclick` handling for exhaustion status in the token HUD to avoid unintended right-button aux interactions.

## v13.533.2.1

- Fix for selected effects not being visibly marked.

## v13.533.2

- Reworked CSS when the Status Effect Sorter is enabled, to match the Foundry UI layout more closely.

## v13.533.1.1

- Added pt_BR translation by [Kharmans](https://github.com/Kharmans) 🤗

## v13.533.1

- Restrict hit detection to specific elements for double clicking on Chat images mechanics.
- Compatibility bump for v5.3.3

## v13.530.1

- Compatibility bump for D&D5e v5.3.0

## v13.525.3

- Status HUD filter keyboard flow improvements:
  - Opening the Assign Status panel now brings focus to the filter field for quicker keyboard use.
  - Press Enter to activate the first visible matching status effect and keep typing in the filter field.
  - Press Escape to clear the filter, and press Escape again on an empty filter to close the Assign Status panel.
  - While filtering, left/right clicking a status now restores filter focus after the HUD updates so keyboard flow continues smoothly.
  - The status filter now retains focus after toggling a status on/off, allowing for quick successive toggling without needing to re-focus the filter.
- Fixed dnd5e Exhaustion icon refresh when lowering Exhaustion back to 0 from the HUD.
- Improved Status HUD sorting consistency so status ordering is stable and predictable.
- Fixed an issue where the first Status HUD open after world load could show incorrect grid sizing until a setting was changed.
- Improved Assign Status Effects compatibility with modules that inject additional temporary/custom effects like CPR:
  - Added effects now follow the same click behavior as regular status rows (icon or label area).
  - Clear effects now also removes compatible module-added effects alongside regular statuses in one update.
- Improved Auto Popout item detection:
  - Matching is now more resilient across item names/identifiers (including slug-style names like `eldritch-blast`).
  - Detection now respects the expected item type (for example spells vs features) for more reliable behavior.
- Hardened overtime and damage-application handling to reduce edge-case failures during roll/message lifecycle updates (more to come in the next patch).

## v13.525.2.2

- Quick fix for damage rolls and shortcut keys.

## v13.525.2.1

- Quick fix for toggling status effects via the Clear all button of the sorter.

## v13.525.2

- Make sure that using keyboard shortcuts for attack roll advantageModes do not pass themselves on damage rolls when auto rolling damage.
- Adds an active GM warning notification when both Bugbear's Assign Status Effects Sorted and Bugbear's Mechanics 5e are enabled at the same time. Please use the BM5e status HUD sorter option instead.

## v13.525.1

- Assign Status Effects Sorter
  - Per user settings
  - Sorted statuses left to right (row) or top to bottom (column)
  - Status search field
  - Clear all statuses button
- Auto roll damage
  - For attack activities, auto-roll damage only applies on hit/crit (based on the attack result)
- Auto pop out chat cards for quick reuse of attack/damage buttons for a built-in list of items/features. Currently:
  - Extra attack feature (when the actor has an item with identifier `extra-attack`)
  - Eldritch Blast (character level 5+)
  - Guardian of Faith spell
  - Magic Missile spell
  - Scorching Ray
  - Spirit Guardians spell
  - Spiritual Weapon attacks
- Attack buttons show the attack total, outcome (critical/fumble/hit/miss), and reuse count when reusing the same chat card.
- Damage buttons show the damage total and the number of times the damage button has been reused (or replaced) from the same chat card.
- Double clicking in the chat log on Actor/Effect/Item icons will:
  - Open the document if you have permission.
  - Otherwise open the document's image.
  - Double clicking again will close the relevant opened sheet/popout.
- Verified for D&D5e 5.2.5.

## v13.524.2.1

- Reinstate damage application tray functionality for saves

## v13.524.2

- Refactor user permissions for overtimes

## v13.524.1

- Fix for damage overtimes not working
- System v5.2.4 compatibility bump

## v13.523.1

- First pass on Overtime effects
- System v5.2.3 compatibility bump

## v13.521.2

- Remove forgotten notification message.

## v13.521.1

- Initial release
- The system's damage application tray for save related damage rolls, will now always respect the Damage on Save options of the activity, selecting the correct multiplier based on whether the save is a success or not.
