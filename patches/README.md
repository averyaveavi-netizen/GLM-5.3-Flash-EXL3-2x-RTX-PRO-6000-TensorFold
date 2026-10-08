# TensorFold v0.6.6 patches

The 86 patches are stored once, by origin: 54 in [miaai-lab/](miaai-lab/) and 32 in
[aevonix/](aevonix/). They apply in global filename order, 0000 through 0087 with 0023
and 0037 omitted because TensorFold already includes their behavior.

`./start.sh` builds them into the image. To apply just the patches to a fresh local
TensorFold v0.6.6 checkout:

```bash
scripts/apply-patches.sh /path/to/TensorFold
```

With no argument the helper clones v0.6.6 and verifies commit
`cb2ebf0540f42604e2759b2ddef497861e928248`. `--list` prints the ordered paths.
Application rejects fuzz, offsets, `.rej` and `.orig` files.

Six patch files were regenerated for the rebase; the other 80 are unchanged.
Mia's AI Lab's patches remain hers. See [the rebase notes](../docs/PORT-v0.6.5.md#rebase-to-v066)
for attribution and the priority-lane integration.

See [the patch table](../docs/PATCHES.md), [validation notes](../docs/NOTES.md),
[NOTICE](../NOTICE) and [LICENSE](../LICENSE).
