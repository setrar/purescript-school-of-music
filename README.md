Purescript School of Music (PSoM)
=================================

[![Latest release](http://img.shields.io/github/release/newlandsvalley/purescript-school-of-music.svg)](https://github.com/newlandsvalley/purescript-school-of-music/releases)
[![Build Status](https://travis-ci.org/newlandsvalley/purescript-school-of-music.svg?branch=master)](https://travis-ci.org/newlandsvalley/purescript-school-of-music)

Try it out [here](http://www.tradtunedb.org.uk:8600/).

This is another attempt at porting the music notation part of the [Haskell School of Music](https://github.com/Euterpea/Euterpea2) (HSoM) to the browser. It consists of a PSoM library (ported from HSoM) together with an editor that runs in the browser. This allows you to enter melodies using a DSL and then play them.

It follows an [abortive attempt in Elm](https://github.com/danigb/elm-school-of-music) in conjunction with danigb.  This failed largely because of the lack of type classes in Elm but also because of the time delays inherent in Elm's port system when requesting that a sound should actually be played.

Supported Instruments
---------------------

PSoM uses instruments from [Benjamin Gleitzman's soundfont library](https://github.com/gleitz/midi-js-soundfonts) which are enumerated in [purescript-midi](https://github.com/newlandsvalley/purescript-midi).  It recognizes all the instruments listed [here](http://gleitz.github.io/midi-js-soundfonts/FluidR3_GM/names.json).  The names differ slightly from those originally used by HSoM - the mapping between the two is shown [here](https://github.com/newlandsvalley/purescript-school-of-music/blob/master/HSoM_INSTRUMENTS.md).  MIDI allows up to 10 such instruments to be available for any given melody.

DSL
---

Melodies are presented to the browser using a [DSL](https://github.com/newlandsvalley/purescript-school-of-music/blob/master/DSL.md) which has been designed to be as close as possible to the API itself.

Editor
------

The editor allows you to enter or load PSoM text and will parse the text after every keystroke. If it is valid, a player will appear, otherwise an error message is shown.

In fact there are two editors which are intended to have identical capabilities - one in Pux and the other in Halogen.  I am still trying to evaluate which UI library I prefer and this seemed to be a good way to decide.  Both are clearly capable of doing the job and they produce javascript output of very similar size.  At the moment, I am veering towards Halogen for the following reasons:

*  It has a well developed concept of self-contained components that can encapsulate their own state.  This means that eventually we should see a market in pluggable Halogen components grow up which should considerably cut down development costs.  Pux, being TEA based, has no such concept.
*  It uses its own VDom representation - it has no need of React.
*  It has been developed commercially and therefore there is a strong incentive to fix bugs quickly.
*  The structure of the application tends to be cleaner because of a better separation  of concerns.
*  Granted, the types are scary but the documentation is so good you can almost take a cookbook approach to building a UI.


Design Questions
----------------

### DSL

What features would make the DSL pleasant and convenient to use?

### Volume

What would be a sensible default value for a note volume (currently 100)?

To Do
-----

*  Allow variable definitions to refer to other variables defined earlier in the same scope
*  Add quickcheck style tests to the PSoM library

The following control mechanisms are unimplemented because they also have not been implemented in HSoM:

*  Custom
*  KeySig
*  Ornamentation
*  Articulation other than Staccato, Legato and Slurred
  

Questions on the HSoM Implementation
------------------------------------


There seem to be various problems surrounding volume in MEvent.  Perhaps it is because I am using only a MIDI backend which has a maximum volume setting of 7F (127).  Firstly, crescendos seem to _start_ at this volume level, although diminuendos are OK.  Secondly, the Loudness implementation seems correctly to set the volume in the context but then to ignore it, taking the volume only of the original note.

The various articulations don't seem to work properly.  Not sure yet if it's a bug in HSoM or in the translation to PureScript.


