# Hacks

A few hacks I keep search the web way too often.

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

## Markdown 

To create a table use this online tool: [https://www.tablesgenerator.com/markdown_tables](https://www.tablesgenerator.com/markdown_tables)

Other hacks: https://github.com/im-luka/markdown-cheatsheet
