# dell-bios-fan-control

Turns BIOS fan control on Dell machines on and off from user space with the SMM calls
`0x34a3` and `0x35a3` — the same pair the kernel's `dell-smm-hwmon` driver uses on
whitelisted models.

With BIOS control disabled the firmware stops overriding the fans, so `fancontrol` or
`i8kctl` can drive them. Enabling it hands them back.

## Build

```
make
```

Nothing beyond a C compiler and libc.

## Usage

Needs root and takes exactly one argument:

```
dell-bios-fan-control 0   # disable BIOS control, the fans are yours
dell-bios-fan-control 1   # enable BIOS control, the firmware takes over again
```

With the i8k module loaded you can then set a speed directly:

```
i8kctl fan 1 1
```

## Machines

Written for the XPS 9560, but the SMM pair is not model specific and works on desktops
too — verified on an OptiPlex 3080 (BIOS 2.35.0, kernel 6.1), where both calls return
successfully.

Many models re-arm the BIOS control on their own, so a machine that goes quiet after
`0` can start overriding the fans again later. That is why [dellfan](https://github.com/mews-se/dellfan)
reapplies it from a systemd drop-in rather than calling this once at boot; it vendors
the tool in `helper/` and installs it when the kernel driver cannot turn the BIOS
control off by itself.

## Differences from the original

* Arguments are matched exactly. With `atoi` a typo parsed as `0` and silently disabled
  the BIOS control.
* The inline SMM assembly declares its memory clobber.
* A failed SMM call warns on stderr instead of passing unnoticed. The exit code is left
  at 0 on purpose — the error heuristic misfires on some BIOSes.
* The Makefile builds with `-O2 -Wall -Wextra`.

## Credits and licence

SMM code lineage: `i8k` by Massimo Dal Zotto, [dellfan](https://github.com/clopez/dellfan)
by Carlos Alberto Lopez Perez, and [dell-bios-fan-control](https://github.com/TomFreudenberg/dell-bios-fan-control)
by Tom Freudenberg, which this repository started from.

GPL-2.0-or-later, per the copyright headers in the source.
