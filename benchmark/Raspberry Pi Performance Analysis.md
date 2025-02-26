# Raspberry Pi Performance Analysis

This guide covers several methods to test the performance of a Raspberry Pi, including temperature monitoring, CPU frequency, memory utilization, I/O read/write speed, and network performance.

## Temperature

To check the temperature of the CPU on a Linux system:

```bash
cat /sys/class/thermal/thermal_zone0/temp
```

Or to use `vcgencmd`, which is a command-line utility specific to Raspberry Pi devices:

```bash
vcgencmd measure_temp
```

## CPU

To monitor the current CPU frequency using the system's interface, shown in `KHz`:

```bash
cat /sys/devices/system/cpu/cpu0/cpufreq/scaling_cur_freq
```

Alternatively, to get the CPU frequency using `vcgencmd`, shown in `Hz`:

```bash
vcgencmd measure_clock arm
```

To test CPU performance:

```bash
sysbench cpu run
```

## Memory

To check the total, used, and free memory, shown in `MB`:

```bash
free -m
```

To test memory performance:

```bash
sysbench memory run
```

## I/O

To test the write speed:

```bash
dd if=/dev/zero of=/tmp/testfile bs=1M count=1024 oflag=direct
```

To test the read speed:

```bash
sudo /bin/bash -c "echo 3 > /proc/sys/vm/drop_caches"
dd if=/tmp/testfile of=/dev/null bs=1M count=1024 iflag=direct
```

To test disk performance:

```bash
sudo hdparm -Tt /dev/sda
```

## Network

`iperf3` is a tool for active measurements of the maximum achievable bandwidth on IP networks.

```bash
sudo apt update
sudo apt install iperf3
```

Set one device as the server:

```bash
iperf3 -s
```

Run the client on the Raspberry Pi:

```bash
iperf3 -c server_ip_address
```

This measures the bandwidth between the Raspberry Pi and the server.

## Conclusion

These methods provide a comprehensive set of tools to evaluate the performance of a Raspberry Pi across various parameters. They can be particularly useful for benchmarking before and after performance tweaks or hardware upgrades.
