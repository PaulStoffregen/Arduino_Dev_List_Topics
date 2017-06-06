Arduino Developers List: Frequently Discussed Topics
----------------------------------------------------

As of June 2017.....

TODO: much detail description to fill in, so little time!

Device Feature Requests
-----------------------

These requests seem to have general agreement, but many differing opinions
on the finer details.

### HardwareSerial (and Wire) buffer size config

Serial & Wire have small, fixed-size buffers.  Some applications can benefit
from increasing these buffer sizes.  The open source code can be edited, but
sketches depending on edited core library code aren't easily shared.  The
desire is to increase these buffer sizes from Arduino sketch code.

Consensus seems to have formed around Martino Facchin's RingBuffer addMemory
proposal.  The public API is still very much debated, but some consensus seems
to involve overloading Serial.begin.  Type/class names and syntax for buffers
are uncertain.  Some concerns about publishing internal core lib structures
as public APIs was expressed.  Progress seems to have stalled.

### API for halt, reboot, watchdog, low power modes

Almost all chips have these features, but with hardware differences.
Attempts to define an API have always been bogged down in the many hardware
details.

### Serial.attachRts(pin) and Serial.attachCts(pin)

Consensus was once reached on these functions to support serial flow control.
Nobody ever added them to any official Arduino libraries.  The plan was the
function would compile and return false if the hardware couldn't support the
feature on the requested pin.

### analogRead support for differential mode, prog gain amp, other special features

### Print class, more control over formatting

### SPI library functions

### analogWriteFrequency(pin, freq)

### Free memory query function

### analog sampling API

### File base class (SD, Bridge, USB Host, SerialFlash)




Performance Issues
------------------

Many times the need for better performance in various Arduino APIs and
libraries has been discussed.

### Wire.endTransmission() & Wire.requestFrom() blocking

### analogRead blocking

ChipKit implementation: see 03/22/2017 10:35 PM

### digitalWrite speed/complexity, digitalWriteFast

### Ethernet library

### Lack of interrupt-based serial transmit on some boards

Most boards have this, but some don't.

### SPI.transfer(buf, count) blocking

### serialEvent and events/callbacks in general

This is a large topic, but in a nutshell event function calls can be done
from interrupt context, from yield (if called from blocking code), or when
idle (after loop returns).  If an RTOS is used, thread affinity is also a
design concern.  Explicit attach functions versus weak symbol binding is
an API-level design choice to make.  Consensus seems impossible...



IDE Feature Requests
--------------------

These are the most commonly requested IDE improvements.

### hardware breakpoint debug

### autocomplete

Probably similar to Microsoft IntelliSense, maybe?  Some patches and
progress have been made.

### serial monitor - VT100, ANSI, other terminal emulation




Frequently Rejected Proposals & Ongoing Controversy
---------------------------------------------------

The Arduino Team has made firm decisions on these topics, but many people
strongly disagree with those choices.  Time and time again these topics
are rehashed.

![](nostairway.jpg)

### Compiler warnings

### Sketch defines to reconfigure core library

### Library dependency detection

### String & use of heap memory (malloc/new)

### printf and/or c++ iostream

### elapsedMillis

### Standards Compliance

### Preemptive RTOS (The Only True Way)

### Java & IDE Memory Usage




