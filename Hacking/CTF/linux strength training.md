
## Task 9
`/home/shared/chatlogs'`
LpnQ:Lucy said that there is an SSH password of james stored somewhere

LpnQ:Sarah need sql database backup, lucy said that sameer knows where it is located

Pqmr:Sameer said that its located in home/shared/sql, he also said that sarah should talk to Michael

KfnP: got Sameer ssh password:`thegreatestpasswordever000`, the sql backup is encrypted with gpg, the password is in home/shared/sql/conf folder, it's a dictionary which need Sameer account. The database passphrase start with ebq.

Inside /home/shared/sql/conf, the JKpN file is around 50MB
JKpN file have a base64 blob that point to `home/sameer/History LB/labmind/latestBuild/configBDB`.
using `grep -iRl ebq` we get three dictionary file. Combine it and take only the word that start with ebq

After we get the backup, we get James entry in employees table:
499996 | 1953-03-07 | James      | vuimaxcullings | M      | 1990-09-27 

His last name is the password, sudo to root.