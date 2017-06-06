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

Serial communication at high speeds often requires hardware RTS/CTS flow control.

Consensus was once reached on these functions to support serial flow control.
Nobody ever added them to any official Arduino libraries.  The plan was the
function would compile and return false if the hardware couldn't support the
feature on the requested pin.

### analogRead support for differential mode, prog gain amp, other special features

Many chips have these analog input features.  Everyone seems to agree Arduino's
API ought to allow people to use them.  But how to define the functions or
parameters is undecided.  How users share code between different boards
with different capabilities isn't easy to resolve.

### Print class, more control over formatting

The most common request is an option to print numbers with leading zeros
or space-padded to a certain number of characters.  Many sketches have code
to check if a number is less than 10 and print a space, less than 100 to print
another space, and so on.

Often these conversations turn into talk of printf.  See below....

### SPI library functions

SPI has a block transfer function, but it overwrites the original buffer.
Some boards have a SPI library with transfer functions using 2 buffers.  Some
support passing NULL or nullptr to not use one of the buffers.

### analogWriteFrequency(pin, freq)

Years ago this function was discussed.  Only implemented on some boards.
Many hardware-specific sketches access timer registers only because they
need to change PWM frequency.

### Timer interrupt API

Many libraries which aren't portable between boards access timer hardware
only to set up a periodic interrupt.  An API to accomplish this one task
(eg, IntervalTimer) would go a long way towards solving the most common
hardware incompatibility.

However, even starting to discuss this topic is quite difficult.  The timer
hardware varies dramatically between boards.  PWM and many libraries use
the timers, so any API can also cause other hardware conflicts.  There are
no perfect solutions.  Consensus on good or even least-bad solution is
very difficult.

### Free memory query function

A way to find out how much memory is "free" is widely desired.  But when this
topic is discussed, different definitions of "free" memory come up.  So far
there seems to be little consensus.

### analog sampling API

Arduino's analogRead() is remarkably difficult to use for low-jitter analog
sampling applications.  Likewise for analogWrite to PWM or DAC pins.

### File base class (SD, Bridge, USB Host, SerialFlash)

Lack of a base class for file storage of data tends to cause the Arduino
ecosystem to create SD-only data processing libraries & sketches.

When Arduino Yun added the Bridge library, a conflict over File arose.  The
solution was something of a quick hack.  Defining a useful base class and
changing the widely used libraries may be a very a time-consuming project?





Performance Issues
------------------

Many times the need for better performance in various Arduino APIs and
libraries has been discussed.

### Wire.endTransmission() & Wire.requestFrom() blocking

### analogRead blocking

ChipKit implementation: see 03/22/2017 10:35 PM

### digitalWrite speed/complexity, digitalWriteFast

### Ethernet library

The Ethernet & Ethernet2 library performance is abysmal.

Work (very slowly) in progress...

Several other Ethernet performance issues exist, notably blocking DHCP.

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

Many people want this!

Opinion differ regarding how or even whether it could be made accessible
to novices.  But in the absence of anyone seriously working on this huge
task, any opinions are pretty much a moot point.

### autocomplete

Probably similar to Microsoft IntelliSense, maybe?  Some patches and
progress have been made.

### serial monitor - VT100, ANSI, other terminal emulation

Many times full or even partial terminal emulation (cursor position,
clear screen, colors, etc) has been requested.  Stand alone terminal
emulators like CoolTerm do this, so why can't Arduino?

Performance concerns exist.  Anyone who's used Arduino for a long time
remembers the bad old days when native USB board or even very fast serial
baud rates could cause the Arduino Serial Monitor to exhaust a PC's
processor and/or the Java virtual machine memory limits.



Frequently Rejected Proposals & Ongoing Controversy
---------------------------------------------------

The Arduino Team has made firm decisions on these topics, but many people
strongly disagree with those choices.  Time and time again these topics
are rehashed.

![](nostairway.jpg)

### Compiler warnings

Arduino's position on compiler warnings has chanced somewhat over the
years.  Historically, all warnings were surpressed.  Modern Arduino has
options to enable warnings in File > Preferences.  The default is still
no warnings.

Some people feel very strongly that some or all compiler warnings should
always be visible.  Opinions differ.  Also quite common is a consensus
that some types of warnings are really more annoying than useful.  Many
also agree with Arduino's long-held philosophy of simplicity for beginners,
that too much helpful info isn't actually helpful at all if it becomes
a stumbling block to getting started.

Many developers are unhappy about the number of warnings present in
the core libraries for various boards, and in many widely used libraries.
Various proposals have been made to try exposing library authors to
compiler warning for thier code, without exposing novice users, but
even those ideas are controversial (eg, don't second guess the compiler
or user settings).

### Sketch defines to reconfigure core library

Often in response to the lack of serial buffer size options, a simple
and powerful idea is proposed.  Allow sketches to optionally have a
config.h or similar file, which could override almost anything defined
in the core library.  Using the right gcc command line options for include
paths, this could be implemented pretty easily and it would indeed allow
very powerful control over many settings.

Arduino has (probably very wisely) believed this could have harmful
long-term effects on the Arduino ecosystem.

### Stand-alone Sketches (bundle all libraries used)

Some users desire a way to package the exact libraries they're used,
perhaps even the Arduino core library too, so that all the source code
for their project resides in their sketch folder.  One often stated
reason is sharing their sketch with others, to assure the recipient
gets exactly the same code, even restricted to only the exactly the
same hardware.

Obviously this sort of functionality could have dramatic long-term
consequences for the Arduino ecosystem.  It also could tend to limit
3rd parties and even Arduino from supporting new hardware, or growing
the platform in the future.

### Library dependency detection and build system

Many thorny details exist in the way Arduino Builder detects which
libraries to use.  Likewise, the Arduino Library Manager has some
shortcomings.

Over and over, people dissatisfied with Arduino's build system will
make a suggestion such as "just use make", or a redesign in the model
of Eclipse or Visual Studio.  Often these neglect important features of
Arduino's system.

### String & use of heap memory (malloc/new)

Some people believe dynamic memory allocation should never be used
in any embedded system.  Issues with memory fragmentation and
non-deterministic performance are real concerns.

Arduino's String class has tremendous usability benefits for novice
users.  The matter was decided in vesion 0019 (August 2010) with
much controversy.  Even now, ther seems to be lingering resentment
or disagreement with any API using heap memory.  This sentiment comes
up over and over again...

### printf and/or c++ iostream

Many experienced programmers love printf().  C++ iostream syntax is
also desired by some.

Arduino has always very decidedly resisted making these parts of
the official Arduino API.  The reasoning is their complex syntax
would almost certainly be used in examples published on numerous
websites, making Arduino less approachable for notices.

### elapsedMillis

This C++ class which emulates an integer that automatically
increments is a Teensy extension which many people love.  It is
available for non-Teensy boards as a library.

Arduino decided the concept of a variable changing breaks some
conceptual programming or teaching paradigms, and should not be
included in the official API.  Anecdotal evidence is people do
"get it", but Arduino's position is skepticism.

### Standards Compliance

Some of Arduino's API decisions ignore or contradict published
programming standards, like C99, POSIX, C++11, etc.  Many of
these decisions are based on usability and ease of learning
for beginners.  Some are practical trade-offs to make better use
of the limited resources on microcontroller platforms.

Occasionally non-Arduino developers argue for unconditional
standards compliance, sometimes even with the notion that
intentionally deviating from a published standard is akin to
blasphemy.

### Preemptive RTOS (The Only True Way)

Many conversations even slighly related to concurrency become
dominated by people who believe very strongly that RTOS systems
with preemptive scheduling are the only valid solution.  Productive
discussion is extremely difficult and consensus seems impossible.

The many pitfalls and difficulties of multithreaded programming,
even for experienced programmers, are of course the antithesis
of Arduino's mission to serve beginners.

Arduino does have a Scheduler library, and other 3rd party libraries
and even some 3rd party boards (like some Energia) do support
multithreading.  Wiring (version 2 ?) provided multithreading at
one point.

### Java & IDE Memory Usage

Some people bitterly hate Java.  It does use a lot more memory
than native code.  Some people apparently have a world view where
computer memory, rather than human effort & collaboration, is the
most precious resource.


