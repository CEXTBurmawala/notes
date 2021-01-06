The command `du` "summarizes disk usage of each FILE, recursively for directories, example:

`du -hs /path/to/directory`

- `-h` is to get the numbers "human readable", e.g. get 140M instead of 143260 (size in KBytes)
- `-s` is for summary (otherwise you'll get not only the size of the folder but also for everything in the folder separately)

If you only use `-h`, you can get the size of each file, which you can sort by size:

`du -h | sort -h`

If want to avoid recursively listing all files and directories, you can supply the `--max-depth` parameter to limit how many items are displayed. Most commonly, `--max-depth=1`

`du -h --max-depth=1 /path/to/directory`

source: https://askubuntu.com/questions/1224/how-do-i-determine-the-total-size-of-a-directory-folder-from-the-command-line

