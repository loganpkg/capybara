<!--
Copyright (c) 2023 Logan Ryan McLintock

Permission to use, copy, modify, and distribute this software for any
purpose with or without fee is hereby granted, provided that the above
copyright notice and this permission notice appear in all copies.

THE SOFTWARE IS PROVIDED "AS IS" AND THE AUTHOR DISCLAIMS ALL WARRANTIES
WITH REGARD TO THIS SOFTWARE INCLUDING ALL IMPLIED WARRANTIES OF
MERCHANTABILITY AND FITNESS. IN NO EVENT SHALL THE AUTHOR BE LIABLE FOR
ANY SPECIAL, DIRECT, INDIRECT, OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES
WHATSOEVER RESULTING FROM LOSS OF USE, DATA OR PROFITS, WHETHER IN AN
ACTION OF CONTRACT, NEGLIGENCE OR OTHER TORTIOUS ACTION, ARISING OUT OF
OR IN CONNECTION WITH THE USE OR PERFORMANCE OF THIS SOFTWARE.

-->
capybara
========

capybara is file system versioning software. It imputes hashes when
{filename, file size, modified time} matches the previous snapshot.
It does not use cascading layers of hard links, and so will work fine
on file systems with hard link limits.

To install, simply place the `capybara` file somewhere in your `PATH`.
You may need to give it executable permission too. It should run on any
POSIX-like system with `sqlite3` installed, including WSL on Windows.

```
Usage:
Unless otherwise stated, all commands need to be run from the source directory:
capybara help
    Print this usage information.
capybara init store_dir
    Sets up the store directory and creates the .capybara_store_location file.
capybara snapshot
    Snapshots the current directory.
capybara vacuum
    Removes unreferenced data.
capybara verify
    Checks the data hashes.
capybara log
    Lists the snapshots.
capybara checkout snapshot
    Recreates snapshot in the checkout subdirectory of the store directory
    using hard links to the data. Do not edit a checkout or it will corrupt
    the store. Copy what you need elsewhere (preserving metadata).
    This can be run from the source directory or the store directory
    (to allow recovery if the source directory disk fails).
```

Caveats
-------
A checkout can have multiple files hard-linked to the same data.
inode level metadata will be arbitrarily set by the last file checked-out,
which will radiate to all hard links, and may not match the snapshot
information held on all files. This occurs because capybara never duplicates
data and uses hard links to speedup checkouts, instead of copying the data.

capybara does not store file ownership information, and so is only suitable
when all of the files belong to the same owner.
