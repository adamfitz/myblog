+++
title = 'ansible datatypes'
date = '2019-05-28'
author = 'fitzi'
tags = [ 'ansible' ]
draft = false
+++

Sometimes when troubleshooting an ansible playbook you might want to know the underlying python data type of a variable. For this you can use the type_debug filter, official docs below:

[managing data types](https://docs.ansible.com/ansible/latest/user_guide/playbooks_filters.html#managing-data-types)

```bash {hl_lines=0}
{{ some_variable | type_debug }}
```