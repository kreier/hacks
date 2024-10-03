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

## Markdown

To create a table use this online tool: 
