# Bugbear's Mechanics for 5e

Bugbear's Mechanics is a Foundry VTT module which introduces some highly opinionated system mechanics!

[Showcase video](https://github.com/user-attachments/assets/7d135cea-8e2b-4143-96b3-af81690d9e64)

## Overtime Effects

You can create effects that trigger automatically at the start or end of a creature's turn.

Create an Applied Active Effect on an Activity, then add an overtime flag:

```text
flags.bm5e.overtime.<TYPE>
```

`<TYPE>` can be one of:

- `damage`
- `heal`
- `check`
- `save`
- `skill`
- `tool`

Configure the overtime effect with comma-separated values.

Timing:

- `turn=start`
- `turn=end`

Roll details:

- `ability=xyz`, such as `str`, `dex`, or `con`
- `ability=str|dex`, `ability=str||dex`, or `ability=str, dex` for multi-ability saves/checks
- `dc=VALUE`
- `@` uses source actor roll data
- `##` uses target actor roll data
- `skill=xyz`, such as `arc`
- `tool=xyz`
- `name=TEXT` adds a label to the generated activity chat flavor

Damage and healing:

- `damage=FORMULA`
- `damageType=TYPE`
- `heal=FORMULA`
- `healingType=healing` or `healingType=temphp`

Save outcomes:

- `onSave=full`
- `onSave=half`
- `onSave=none`

Progressive results:

- `overallFailures=NUMBER`
- `overallSuccesses=NUMBER`
- `consecutiveFailures=NUMBER`
- `consecutiveSuccesses=NUMBER`
- `successiveFailures=NUMBER`
- `onFailure=remove`, `onFailure=prone`, or another status condition
- `successiveSuccesses=NUMBER`
- `onSuccess=remove`, `onSuccess=prone`, or another status condition

Using `remove` deletes the overtime effect entirely.

Compatibility aliases are also supported, including `saveSuccesses`, `saveSuccess`, `successes`, `saveFailures`, `saveFails`, `failures`, `successiveSuccesses`, `successiveSaves`, `successiveFailures`, and `successiveFails`.

### Check Overtime Example

This example triggers at the start of the actor's turn. The actor makes a DC 15 Strength check using Athletics or Acrobatics. After 3 consecutive successes the overtime effect is removed. After 3 consecutive failures the actor gains the Prone condition.

Key:

```text
flags.bm5e.overtime.check
```

Value:

```text
turn=start, ability=str, skill=ath|acr, dc=15, consecutiveSuccesses=3, onSuccess=remove, consecutiveFailures=3, onFailure=prone
```

### Linking an Existing Activity

Instead of creating a pseudo overtime roll, you can reuse an existing Activity.

Copy the Activity UUID from the title bar button and paste it into the effect value:

```text
link=Scene.9gP78CMgo7ZmATt2.Token.3TjlspjtW1JPoYk1.Actor.KBYJmwlbPBToyRY9.Item.DgH1PglrwHrwf9zv.Activity.erT6fai51NJq3AQG
```

The linked Activity becomes the overtime roll.

You can still combine `link` with other values, such as `turn`, `onSuccess`, and `successiveSuccesses`.

At the start and end of a creature's turn, if one or more overtime effects are present on the actor, a chat message is created indicating which overtime effects are available to roll.

If Midi-QOL is active and Activation Cost Automation is set to Auto Trigger, these overtime effects are rolled automatically.
