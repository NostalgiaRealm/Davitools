# Davitools

Tools to do stuff with Davilex games, like Vakantie Racer

Several Davilex games keep their game files in an archive of two files: an index file (`.ind`) and an image file (`.img`, not a disk image), for example `rcs.ind` and `rcs.img`. Davitools can extract these archives and create new ones.

Look for the archives on the game disc (often in a `data` folder) or in the installation folder. On Linux, mind the casing: `rcs.ind`, `RCS.IND` and `rcs.IND` all occur.

You need PHP 7.4 or newer on the command line ([installation guide](https://www.php.net/manual/en/install.php)). Keep the `src` directory next to the scripts.

## Extracting an archive

```
php rcs-extract.php <index file> <image file> <output directory>
```

**Use a separate output directory.** The `rcs` archives of A2 Racer III Europa Tour and Autobahn Raser II contain an empty entry called `rcs.img`. Extracting one of those into the directory that holds its image file empties the image file without warning if it is writable, and all extracted files then come out empty.

The output directory is created if needed. Existing writable files with the same name as an entry are overwritten.

`Duplicate filename: <name>` means the index file contains that name more than once, ignoring case, which is common in the original archives. Entries with exactly the same name overwrite each other, so only the last one is kept, even if their contents differ. Names that differ only in case also overwrite each other on a case-insensitive file system. Entries called `*empty resitem*` or `*Empty ResItem*` follow the same rules; they do contain data, but cannot be written on Windows because of the `*`.

## Creating an archive

```
php rcs-pack.php <input directory> <index file> <image file>
```

Note that the directory comes first here. For example, to create a new archive in `new` from the files in `extracted`:

```
mkdir new
php rcs-pack.php extracted new/rcs.ind new/rcs.img
```

- Everything in the input directory is packed, sorted by name with uppercase before lowercase, except names starting with a `.`.
- Keep only the files you want in there, and write to a different directory that already exists. Subdirectories are not entered but treated as files, and an index or image file you are writing to is packed too if it already exists in the input directory. Both can corrupt the archive.
- Existing index and image files are overwritten, so keep a copy of the originals.
- File names are stored as ISO-8859-1 (other characters become `?`) and cut off after 20 bytes without warning, even if that removes the extension.

Repacking does not give you the original archive back byte for byte: entries are sorted, only the last of any entries with exactly the same name is left, and garbage after file names in the index file is not copied.

The author tested packing by adding extra music to Vakantie Racer (see [#1](https://github.com/Gymnasiast/Davitools/issues/1)). How other games respond to a new archive is not documented. Davitools does not read or update `.chk` files, such as `rcs.CHK` in A2 Racer 4 and RedCat SuperKarts.

## Which archives work

Only index files that contain file names are supported. For this README, extraction was verified on Linux with PHP 8.5, by comparing the output with the raw archive data, on A2 Racer (`rcs`, `a2mrcs`), A2 Racer II (`rcs`, `mrcs`), A2 Racer III Europa Tour (`rcs` on both CDs), A2 Racer 4 (`rcs`, `menurcs`), Autobahn Raser II (`rcs`), RedCat SuperKarts (`rcs`), RedCat Spookkasteel (`menurcs`) and WK-98 (`rcs`, `wkmrcs`).

These index files from the same games are not supported:

- `rc`, `menurc`, `a2mrc`, `gamespr`, `vbengine` and `wkmrc` only contain offsets. `rcs-extract.php` stops with a `TypeError`.
- `sfx` from A2 Racer 4 is not in this format. `rcs-extract.php` prints nothing and leaves an empty output directory.

## File format

```text
index file
  uint16      number of entries
  for each entry:
    char[20]  file name in ISO-8859-1, ends at the first NUL byte or after 20 bytes
    uint32    offset of the file in the image file

image file
  the contents of all files, one after the other
```

All numbers are little-endian. An entry's size is the next entry's offset minus its own; the last entry runs until the end of the image file. In some games, the bytes after the NUL byte in a file name contain garbage, which is ignored. A supported index file is exactly `2 + 24 × entries` bytes; one that only contains offsets is `2 + 4 × entries` bytes.

## Troubleshooting

`rcs-extract.php` does not check whether the index and image file belong together. A wrong combination can also finish without any error, leaving files with the wrong contents or no files at all.

## License

[MIT](LICENSE)
