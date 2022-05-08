```bash
locate name
```

`locate` from filename. Faster than find. Won't find recently added file(before updatedb).

Limited result based on `/etc/updatedb.conf` file. Also can't view file that you don't have permission to view.

Actually locates the string in the path, not just filename.

`updatedb` to update the database

#### Options
- -i: ignore case