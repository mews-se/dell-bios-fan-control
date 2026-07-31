## dell-bios-fan-control

#### A user space utility to enable or disable BIOS control of the fans on Dell machines.

Originally written for the Dell XPS 9560, but the SMM commands it uses work on
most Dell machines (laptops as well as desktops such as the OptiPlex series).

Allows to control the fans by bios or i8kctl utils.

  

### Usage

To enable SMBIOS control:

<pre>
dell-bios-fan-control 1
</pre>

To disable SMBIOS control:

<pre>
dell-bios-fan-control 0
</pre>

After disabling SMBIOS control of fans you can set fan speed by i8kctl:

<pre>
i8kctl fan 1 1
</pre>

  

### Caveats

* The BIOS of most newer Dell machines (9560, OptiPlex, ...)
  will override the speed you set unless you disable the BIOS control.

* This tool allows to enable or disable bios control of fans to
  use i8kmon instead


### Credits

* All credits belong to: https://github.com/clopez/dellfan
