---
title: Configuration
---

This is about the pattern configuration file. FreeSewing (the library) does not require configuration. The configuration documented here is the configuration file for patterns built on top of FreeSewing.

For run-time configuration, see [settings](/settings).

## name

```js
name: "sorcha"
```

A string with the name of your pattern.

## version

    version: "0.3.1"
    

## dependencies

```js
dependencies: {
  front: "back",
  sleeveplacket: ["sleeve", "cuff"]
}
```

An object of `key`-`value` pairs that controls the order in which pattern parts will get drafted.

<Tip>

See [Part dependencies](/advanced/dependencies) for more in-depth information on dependencies.

</Tip>

## inject

```js
inject: {
  front: "back"
}
```

An object of `key`-`value` pairs of parts. The `value` part will be injected in the `key` part.

By *injected* we mean rather than starting out with a fresh part, you'll get a part that has the points, paths, and snippets of the `value` part.

<Tip>

See [Part inheritance](/advanced/inject) for more in-depth information on inject.

</Tip>

## hide

```js
hide: [
  "base"
]
```

An array that lists pattern parts that should be hidden by default. Hidden means that they will be drafted, but not rendered. Typically used for a base part on which other parts are built.

## parts

```js
parts: [
  "front",
  "back"
]
```

An array that lists your (additional) pattern parts. The name must be the key the `pattern.parts` object.

<Tip>

###### This does not need to be an exhaustive list of all parts in your pattern.

This list of parts is needed for the `draft()` method to figure out what parts need to be drafted. So if parts are included in the `dependencies`, `inject`, or `hide` configuration, there's no need to include them here, as we already know of their existence.

</Tip>

## measurements

```js
measurements: [
  "bicepsCircumference",
  "centerBackNeckToWaist"
]
```

An array with the names of the measurements required to draft this pattern.

<Note>

###### Don't just make up names

See [freesewing models](https://github.com/freesewing/models) for a list of measurement names already used in freesewing patterns. It is a [best practice](/do/dont-re-invent-the-wheel) to stick to these names.

</Note>

## options

Options come in 6 varities:

- [Constants](#constants) : A value that can't be changed
- [Booleans](#booleans) : A value that is either `true` or `false`
- [Percentages](#percentages) : A value in percent, with minimum and maximum values
- [Millimeters](#millimeters) : A value in millimeter, with minimum and maximum values
- [Degrees](#degrees) : A value in degrees, with minimum and maximum values
- [Counters](#counters) : An integer value, with minimum and maximum values
- [Lists](#lists) : A list of options with a default

Under the hood, millimeters, degrees, and counters are handled the same way. We use different types because it easier to understand the nature of a given option.

### Constants

If your option is a scalar value (like a string or a number), it will be treated as a constant:

```js
options: {
  collarFactor: 4.8
}
```

Rather than define constants in your code, it's good practice to set them in your configuration file. This way, people who extend your pattern can change them if they would like to.

### Booleans

If your option is either `true` or `false, or **on** or **off** or **yes** or **no**, you can use a boolean:

Your boolean option should be an object with these properties:

- `bool` : Either `true` or `false` which will be the default

```js
options: {
  withLining: { bool: true }
}
```

### Percentages

Percentage options are the bread and butter of freesewing. Almost all your options will probably be percentages. They make sure that your pattern will scale regardless of size, and pass [the ant-man test](https://github.com/freesewing/antman).

Your percentage option should be an object with these properties:

- `pct` : The percentage
- `min` : The minimul that's allowed
- `max` : The maximum that's allowed

```js
options: {
  acrossBackFactor: { 
    pct:  97, 
    min:  93, 
    max: 100 
  }
}
```

<Note>

###### Percentage options will be divided by 100 when loaded

You specify percentages in your config file. For example, `50` means 50%. When your configuration is loaded, those percentages will by divided by 100.

So a percentage of `50` in your config file will be `0.5` when you read out that option in your pattern.

</Note>

### Millimeters

While we recommend using percentages where possible, sometimes that doesn't make sense.  
For those cases, you can use millimeters.

Your millimeter option should be an object with these properties:

- `mm` : The default value in millimeter
- `min` : The minimul that's allowed
- `max` : The maximum that's allowed

```js
options: {
  elasticWidth: { 
    mm:  35, 
    min:  5, 
    max: 80 
  }
}
```

### Degrees

For angles, use degrees.

Your degree option should be an object with these properties:

- `deg` : The default value in degrees
- `min` : The minimul that's allowed
- `max` : The maximum that's allowed

```js
options: {
  collarAngle: { 
    deg:  85, 
    min:  60 
    max: 130 
  }
}
```

### Counters

For a given number of things, use counters. Counters are for integers only. Things like number of buttons and so on.

Your counter option should be an object with these properties:

- `count` : The default integer value
- `min` : The minimal integer value that's allowed
- `max` : The maximum integer value that's allowed

```js
options: {
  butttons: { 
    count: 7, 
    min:   4,
    max:  12 
  }
}
```

### Lists

Use a list option when you want to offer an array of choices.

Your list option should be an object with these properties:

- `dflt` : The default for this option
- `list` : An array of available values options

```js
options: {
  cuffStyle: { 
    dflt: "angledBarrelCuff",
    list: [
      "roundedBarrelCuff",
      "angledBarrelCuff"
      "straightBarrelCuff"
      "roundedFrenchCuff"
      "angledFrenchCuff"
      "straightFrenchCuff"
    ]
  }
}
```

## Extra

Patterns also take these configuration options to facilitate frontend integration:

### design

The name of the designer:

```js
design: "Joost De Cock"
```

### code

The name of the developer:

```js
code: "Joost De Cock"
```

### type

Either `pattern` or `block`.

```js
type: "pattern"
```

### difficulty

A `1` to `5` difficulty score that indicates how hard it is to make the pattern:

```js
difficulty: 3
```

### tags

A set of tags to allow filtering of patterns on the website:

```js
tags: ["underwear", "top", "basics"],
```

### optionGroups

Organises your pattern options in groups. It expects an object where the key is the group title, and the value an array of options:

```js
optionGroups: {
  fit: ["chestEase", "hipsEase", "stretchFactor"],
  style: [
    "armholeDrop",
    "backlineBend",
    "necklineBend",
    "necklineDrop",
    "shoulderStrapWidth",
    "shoulderStrapPlacement",
    "lengthBonus"
  ]
} 
```