---
title: "Instructor Notes"
---

The quickest way to teach this workshop is by providing students with a
server they can play around with. This can for example be done by
setting up a virtual machine in Microsoft Azure, as is discussed in the
[second episode](../02-setup-vm/index.html) of this workshop, preferably
with an Ubuntu environment (as `apt` is used as an example and some of the screenshot show an Ubuntu environment).

There is an instructor package
[instructor_setup.tar.gz](../files/instructor_setup.tar.gz)
available. This contains a number of useful scripts, as well as a skeleton directory and file structure for the
user accounts of the participants (this helps them get up to speed quickly.

The workshop assume some standard versions of Java, Python and R are
available. It is useful if the teacher installs this software before the
start of the workshop. The script `configure_server.sh` can be used to
perform this on a fresh Azure installation of Ubuntu 18.04.

A second step is to create user accounts for the users. The script 
`create_users.sh` can be used for this purpose. It will use the 
directory `base` (also in the package) as the skeleton for newly 
created user accounts, and create a bunch of numbered user accounts 
with some prefix (e.g. `test1`, `test2`, etcetera). Random passwords 
are generated and saved into a `.csv` file that can be used to 
communicate a username and password combination to each user.

> ## Example
>
> Suppose we want to create 60 users prefixed by `test` with
> random passwords of length 12 and save their passwords to the
> file `workshop-users.csv`, you run the following command:
> ```
> $ ./create_users.sh -n 60 -p test -f workshop-users.csv -l 12 -c
> ```
> {: .language-bash }
>
> Note that some options are optional: the default prefix is `test`,
> and the default password length is `12`. The `-c` flag is required
> to actually create the users and set the password. If ommitted,
> only a csv-file with username/password combinations will be created.

Finally, during the workshop it may happen that a users accidentally
deletes all the files during one episode, which may make things tricky
during another episode. The script `reset_user.sh` can be used to
copy the `base` directory to the home directory of the user,
recreating the deleted files and giving the correct permissions.

To restore all the `base` files to the home account of a user, run

```
$ sudo ./reset_user -u username
```
{: .language-bash }

Optionally, you can add the `-c` flag, which will first wipe the home directory before the `base` files are copied. In most cases this flag
should not be required.


{% include links.md %}
