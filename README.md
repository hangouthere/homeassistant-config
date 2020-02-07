# NFGArmy HQ Home Assistant Configuration

> ***This Repository is under heavy work - Changes WILL be forthcoming!***

This repository is the progress and growth of the [Home Assistant](https://www.home-assistant.io/) configuration utilized at the NFGArmy Headquarters!

Everything from custom control of lighting, to automations, to integrations with 3rd parties are all within the confines of this repo.

Jump right in!

## Organization

For the most part, I follow a typical "[Splitting up the configuration](https://www.home-assistant.io/docs/configuration/splitting_configuration/)" construct: Put things into their own files.

Where I thought it made sense, I opted to use [Packages](https://www.home-assistant.io/docs/configuration/packages/) to keep related configuration together. There may be some light blurring between a few of them, but I did my best to keep them isolated properly.

### Lovelace

Lovelace config is pretty split up, where I also took it a step further. In some cases, I split up the view into individual cards (or a full stack) to make editing easier. It was becoming difficult to keep track, even when split on a *view*-level.

Each view is split, and numbered in a way that allows for ordered loading, with gaps to be able to inject views between others.

### Entity Naming Convention

I've opted to build my own naming convention for the sake of visually grouping, programmatically selecting, and human-memory recall.

A particular naming convention may likely span multiple domains, and starts with the entity_id *name* itself.

Example:
```
<domain>.<convention>_<rest_of_name>
```

| Group    | Convention | Example                                 | Notes                                         |
| -------- | ---------- | --------------------------------------- | --------------------------------------------- |
| HVAC     | hvac       | `sensor.hvac_temp_target`               | Thermostat and Temp tracking                  |
| Presence | presence   | `input_boolean.presence_codex`          | Presence Detection and Handling               |
| Intent   | intent     | `input_boolean.intent_expecting_guests` | Semaphore Intents, meant for Voice Assistants |

## ZWave

### Quick Info

Look up product info here: https://products.z-wavealliance.org/regions/2/categories

My Devices:

* [GE/Jasco Dimmer Switch (46203)](https://products.z-wavealliance.org/products/3323?selectedFrequencyId=2)
* [GE/Jasco Fan Control (14287)](https://products.z-wavealliance.org/products/2506?selectedFrequencyId=2)
* [qolsys IQ Dimmer (QZ2140-840)](https://products.z-wavealliance.org/products/2912?selectedFrequencyId=2)

My switches (get model number) didn't support `transition` from service calls out of the box

Had to add the following to the `COMMAND_CLASS_SWITCH_MULTILEVEL` node:

```
    <Value type="byte" genre="system" instance="1" index="5" label="Dimming Duration" units="" read_only="false" write_only="false" verify_changes="false" poll_intensity="0" min="0" max="255" value="255" />
```

### Status LED Light

The blue Status LED Light on every switch doesn't seem like a big deal during the day, but if you wake up at 3am it's eye piercing, and VERY annoying! Fortunately, this can be changed.

Luckily for me, all my devices support via the same parameter and values.

To set the parameter, simply use `zwave.set_config_parameter` to assign a new paramter:

```
{"node_id": "3", "parameter": "3", "size": "1", "value" : "2"}
```

> **Note:** There are scripts and automations established for this within the repository! 

## Alexa Voice TTS

I'm utilizing a custom component called "Alexa Media Player". Among many things, it allows me to execute TTS against an Echo, and use [SSML](https://developer.amazon.com/en-US/docs/alexa/custom-skills/speech-synthesis-markup-language-ssml-reference.html) to make it fancier.

Project URL: https://github.com/custom-components/alexa_media_player

Install: Checkout, and set up in `Configuration > Integrations`

#### Example SSML

```yaml
message: >
    <voice name="Salli">Hey Nerds, this is just an example of something Codex has been working on</voice>.
    <voice name="Emma">It's part of the Home Automation series that he plans on doing on stream</voice>.
    <voice name="Matthew">So, if you're not already, follow him on Mixer, at mixer dot com, slash nfgCodex</voice>.
    <voice name="Nicole">We cannot wait to hang out with you all again</voice>.
    <amazon:effect name="whispered">PS, we really really miss you</amazon:effect>.
data:
    type: announce
```

#### Example of every voice

```yaml
message: >
    <voice name="Ivy"> This Is A Test in this voice </voice>
    <voice name="Joanna"> This Is A Test in this voice </voice>
    <voice name="Joey"> This Is A Test in this voice </voice>
    <voice name="Justin"> This Is A Test in this voice </voice>
    <voice name="Kendra"> This Is A Test in this voice </voice>
    <voice name="Kimberly"> This Is A Test in this voice </voice>
    <voice name="Matthew"> This Is A Test in this voice </voice>
    <voice name="Salli"> This Is A Test in this voice </voice>
    <voice name="Nicole"> This Is A Test in this voice </voice>
    <voice name="Russell"> This Is A Test in this voice </voice>
    <voice name="Amy"> This Is A Test in this voice </voice>
    <voice name="Brian"> This Is A Test in this voice </voice>
    <voice name="Emma"> This Is A Test in this voice </voice>
    <voice name="Aditi"> This Is A Test in this voice </voice>
    <voice name="Raveena"> This Is A Test in this voice </voice>
    <voice name="Hans"> This Is A Test in this voice </voice>
    <voice name="Marlene"> This Is A Test in this voice </voice>
    <voice name="Vicki"> This Is A Test in this voice </voice>
    <voice name="Conchita"> This Is A Test in this voice </voice>
    <voice name="Enrique"> This Is A Test in this voice </voice>
    <voice name="Aditi"> This Is A Test in this voice </voice>
    <voice name="Carla"> This Is A Test in this voice </voice>
    <voice name="Giorgio"> This Is A Test in this voice </voice>
    <voice name="Mizuki"> This Is A Test in this voice </voice>
    <voice name="Takumi"> This Is A Test in this voice </voice>
    <voice name="Celine"> This Is A Test in this voice </voice>
    <voice name="Lea"> This Is A Test in this voice </voice>
    <voice name="Mathieu"> This Is A Test in this voice </voice>
data:
    type: announce
```

## Support 

While I offer this information for free, if you'd like to throw a few bones our way for Coffee or a new Game, you can do so on our [Donation page](http://rebrand.ly/nfgDono).