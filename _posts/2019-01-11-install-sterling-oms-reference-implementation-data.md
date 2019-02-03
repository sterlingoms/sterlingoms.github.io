---
layout: post
title:  "Installing Sterling OMS Reference Implementation"
categories: [Reference Implementation, Factory Setup]
---
Sterling Order Management System reference implementation can be installed through a click of a checkbox during installation. If you missed to install the reference implementation during the initial install you have an option to install it anytime post install. 

To install the reference implementation navigate to the Sterling OMS installation directory

```sci_ant.cmd -f ycd_load_oms_ref_impl.xml -Drunmasterdata=Y -logfile C:\OMS\logs\refimpl.log```


![Sterling OMS reference Implementation](/assets/images/refimpl.png){:class="img-responsive"}

Post installation validate the list of Organizations created with the SQL query

```
select * from yfs_organization;
```

Additional organizations installed as part of the reference implementation should be retrieved in the query results.