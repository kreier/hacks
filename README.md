# Hacks

A few hacks I keep search the web way too often.

- [Write protected disks and diskpart](#write-protected-disks-and-diskpart)
- [Lower power consumption](#lower-power-consumption)
- [Run ollama on RX 6600 and RX 470](#run-ollama-on-rx-6600-and-rx-470)
- [Markdown](#markdown)
- [Deactivate GUI](#deactivate-gui-on-ubuntu)

## Write protected disks and diskpart

List disks, partitions and volumes. And select them once diskpart is started.

``` sh
list disk
list partition
list volume
select volume 4
delete volume override
```

Remove write protection

``` sh
attributes disk clear readonly
```

Format in fat32 quick

```
select partition 3
format fs=fat32 label="test" quick
```

## Lower power consumption

Check your package C-states. If you're only on C2 this might be the reason your power consumption is rather high. Check with a Linux distribution in superuser mode and `powertop`. You might get better results after executing

``` sh
powertop --auto-tune
```

But for me this affects the interrupts from the keyboard and mouse, the system seems to be very unresponsive. And usually the improvement in powerconsumption is not very large.

## Run ollama on RX 6600 and RX 470

As described in [this blog post](https://major.io/p/ollama-with-amd-radeon-6600xt/) it is possible to run ollama even on AMD graphics cards, even if the specific hardware does not yet have the [HIP SDK support](https://rocm.docs.amd.com/projects/install-on-windows/en/latest/reference/system-requirements.html). Usually the GPU should be found with `> lspci | grep -i VGA`. At first glance with `sudo journalctl --boot -u ollama` it looks like the GPU is not supported. But you can override that with the `HSA_OVERRIDE_GFX_VERSION` setting as explained in detail in this [documentation of ollama](https://github.com/ollama/ollama/blob/main/docs/gpu.md#overrides). Let's do that:

``` bash
> sudo systemctl edit ollama.service
```

And enter

``` sh
### Editing /etc/systemd/system/ollama.service.d/override.conf
### Anything between here and the comment below will become the contents of the drop-in file

[Service]
Environment="HSA_OVERRIDE_GFX_VERSION=10.3.0"
Environment="ROCM_PATH=/opt/rocm"

### Edits below this comment will be discarded
```

With systemd we can reload the unit and restart ollama:

``` sh
> sudo systemctl daemon-reload
> sudo systemctl stop ollama
> sudo systemctl start ollama
```

After that I can use the RX6600 without problems, since its LLVM target is `gfx1032` and close to the overwritten `gfx1030`. Even the older `gfx900` of the Vega 56 would work. But my RX 470 is `gfx803`. Accordingly in ollama it is reported `amd_linux.go:305 msg="amdgpu too old gfx803" gpu=0`. I need to upgrade.

## Markdown 

To create a table use this online tool: [https://www.tablesgenerator.com/markdown_tables](https://www.tablesgenerator.com/markdown_tables)

Other hacks: [https://github.com/im-luka/markdown-cheatsheet](https://github.com/im-luka/markdown-cheatsheet)

## Deactivate GUI on Ubuntu

``` sh
sudo systemctl set-default multi-user.target
```

And revert with

``` sh
sudo systemctl set-default graphical.target
```
