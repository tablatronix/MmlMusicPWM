# MmlMusicPWM
Arduino library to play MML music using a piezo speaker on an output pin

### Introduction
This library is an addition to the [MmlMusic base library](https://github.com/maxint-rd/MmlMusic). It produces sound on an output pin that can be connected to a piezo speaker, or via an amplifier to a regular speaker.

### Device independant base class
This library is implemented as a child class of the device independant MmlMusic class. That class requires additional code via either a callback function or the implementation of a child class to implement the actual production of sound.

### Support for different MCUs
This library supports playing MML music on different MCUs such as ESP8266, ATmega 328, 168 and ATtiny85. Depending on the MCU, it uses different timers and interrupts to produce the sound and to schedule playback of notes and silences.

### Installation/Usage
The current version can be downloaded as an Arduino library using the Sketch|Library menu. Just add the zipfile library and the enclosed examples should appear in the menu automatically. 

Initialisation outside of Setup():
```
  // include header and initialize class
  #include <MmlMusicPWM.h>
  MmlMusicPWM music();
```

Then to play music, call the play method where you want:
```
music.play("T240 L16 O6 C D E F G");
```

To continue playing the rest of the sequence, regular calls to continuePlaying() must be made, either in a timer interrupt or as part of a loop.
```
if(music.isPlaying()
{
    ...
}
```

See the included examples for more detailed instructions on how to use this library.

### Supported MML Syntax
Short syntax overview:<br>

Command | Description
------------ | -------------
&nbsp;  Tnnn | Set tempo [32-255]. Examples: T120, T240<br>
&nbsp;  Vnnn | Set volume [0-128]. Note: limited effect on PWM-volume. Examples: V1, T120<br>
&nbsp;  Lnn  | Set default note length [1-64]. Examples: L8, L16<br>
&nbsp;  Mx   | Set timing. Mn=default, Ml=legato, Ms=staccato<br>
&nbsp;  On   | Set octave [0-7]. Examples: O6, O7<br>
&nbsp;  A-G  | Play whole note. Example: C<br>
&nbsp;  Ann-Gnn  | Play note of alternative length [1-64]. Example: C4, A16<br>
&nbsp;  Nnn  | Play frequency [0-96]. Example: N48<br>
&nbsp;  #    | Play sharp note. Example: C#<br>
&nbsp;  &plus;    | Alternative for #<br>
&nbsp;  &minus;   | Play flat note. Example: D-<br>
&nbsp;  R    | Rest. Example:  CDEC r CDEC<br>
&nbsp;  P    | Alternative for R. Example:  CDEC p CDEC<br>
&nbsp;  .    | Longer note. Example: CDEC.&nbsp;<br>
&nbsp;  &gt; | shift octave up.  Example: CDE&gt;CDE.&nbsp;<br>
&nbsp;  &lt; | shift octave down.  Example: CDE&lt;CDE.&nbsp;<br>
&nbsp;  , | play multiple tracks  Example: CDE&lt;CDE.,EDC&lt;ECD.&nbsp;<br>

The supported MML-commands are a subset that may not completely cover all available music scores.
If notes seem missing, check your score against the syntax above and replace unknown commands by equivalent supported alternatives. The music notation is case-insensitive. Spaces are not required but can be used for readability.

### Features & limitations
- Playing back multiple tracks is supported by the base library, but not (yet) by this library. Only a single pulse-stream is generated. The documentation of the comma-command above is maintained for future purposes.
- This library was tested in the Arduino IDE v1.6.10 and v1.8.2. The current version of this library supports ESP8266, Atmel ATmega328 and ATmega168 MCUs. Support for ATtiny85 was also added, but since the ATtiny85 has limited resources, available memory limits it usage to simple applications.
- Known bug: when ending the play-string with a number (eg. "T120 O4 G16") the player may read beyond the end of the string and play whatever is next in memory. Workaround: use alternative notation (eg. "T120 O4 L16 G") or an addional terminator (eg. "T120 O4 G16\0").

### Credits
The base library is based on the MusicEngine library by Chris Taylor, ported from mBed to Arduino. It is a follow-up of the [ESP-MusicEngine library](https://github.com/maxint-rd/ESP-MusicEngine).

### Links
- Learn more about Music Macro Language (MML) on wikipedia:<br>
   http://en.wikipedia.org/wiki/Music_Macro_Language<br>
- For downloadable MML music see http://www.archeagemmllibrary.com/<br>
- Extensive MML reference guide (not all commands supported):<br>
   http://woolyss.com/chipmusic/chipmusic-mml/ppmck_guide.php<br>
- Info about using PWM and other methods to generate sound:<br>
   https://developer.mbed.org/users/4180_1/notebook/using-a-speaker-for-audio-output/
