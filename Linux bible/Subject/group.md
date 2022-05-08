Group is used mainly for permission. located in `/etc/group`

use `newgrp group_name` to temporarily set the user primary group to that group, thus setting the file group owner. The owner must be a member of that group first.

It's possible to use newgrp for non group member. You must assign a group password as root first with `gpasswd group_name`. The any user can temporarily set their primary group with the password.

### Creating group
As root, create group with
```bash
groupadd kings
groupadd -g 1325 jokers
```
`-g number` set the gid of the group.

