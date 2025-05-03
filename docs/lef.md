LEF stands for "Lightweight Executable Format". It's designed for Animula VM and highly optimized for compact embedded system.

*LEF is still evolving and may have improvements in the future.*

# The design of LEF

<center>
<img src="../img/LEF.png" title="LEF format" alt="LEF format"/>
</center>

The order of the field sizes in the LEF header (see above) must be used to calculate the actual field offsets in the LEF body.
