+++
title = 'yum commands'
date = '2019-10-04'
author = 'fitzi'
tags = [ 'yum', 'linux' ]
draft = false
+++
Equivalent of apt-get update

```bash {hl_lines=0}
yum check-update
```

List configured (enabled) repositories

```bash {hl_lines=0}
yum repolist
```

 **Fixing duplicate packages**

Install yum utils

```bash {hl_lines=0}
yum install yum-utils
```

check for and list duplicates

```bash {hl_lines=0}
package-cleanup --dupes
```

remove duplicates

```bash {hl_lines=0}
package-cleanup --cleandupes
```

Check for any problems with the yum database

```bash {hl_lines=0}
package-cleanup --problems
```