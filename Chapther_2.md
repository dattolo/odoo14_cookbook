*Odoo 14 Development Cookbook*

**Chapther 2**

# Managing Odoo Server Instances

## Configuring the add-ons path
When Odoo initializes a new database, it will search for `add-on modules` within directories that have been provided in the addons_path configuration parameter.

My config file
`/opt/odoo/etc/odoo.conf`

`addons_path` line 
*addons_path = /opt/odoo/additional_addons/dr-ricambi,/opt/odoo/additional_addons/product-attribute,/opt/odoo/sources/odoo/addons*

**The additional addons must contain at least one sub-directory, which has the minimal structure of an add-on module.**

## Standardizing your instance directory layout
We recommend that your *development* and *production environments* all use a similar directory layout. This standardization will prove helpful when you have to perform maintenance operations, and **it will also ease your day-to-day work**.

By having one virtualenv environment per project, we are sure that the project's dependencies will not interfere with the dependencies of other projects. This comes at the cost of a little disk space.

## Installing and upgrading local add-on modules

### From the web interface
### From the command line
Run the following command:
```
$ odoo/odoo-bin -c instance.cfg -d dbname -i addon1,addon2 \

--stop-after-init
```

You may omit -d dbname if this is set in your configuration file.

Restart the instance.

###Add-on update
When you update an add-on, Odoo checks in its list of available add-on modules for an installed add-on with the given name. It also checks for the reverse dependencies of that add-on (these are the add-ons that depend on the updated add-on). If any, it will recursively update them, too.

## Installing add-on modules from GitHub

Odoo Community Association (**OCA**) collectively maintains several hundred add-ons on GitHub. *Before you start writing your own add-on, ensure that you check that nothing already exists that you can use as is or as a starting point.*

[...]




*Francesco Dattolo*
*2107280952*