*Odoo 14 Development Cookbook*

**Chapther 3**

# Creating Odoo Add-On Modules

How an add-on module is structured and the typical incremental workflow to add components to it. 
- Creating and installing a new add-on module
- Completing the add-on module manifest
- Organizing the add-on module file structure
- Adding models
- Adding menu items and views
- Adding access security
- Using the scaffold command to create a module

## Technical requirements
Code source
[https://github.com/PacktPublishing/Odoo-14-Development-Cookbook-Fourth-Edition/tree/master/Chapter03](https://github.com/PacktPublishing/Odoo-14-Development-Cookbook-Fourth-Edition/tree/master/Chapter03)

## What is an Odoo add-on module?
There are two main purposes for these modules. Either you can add new apps/business logic, or you can modify an existing application. **Put simply, in Odoo, everything starts and ends with modules.**

Every module has *main application domain* - **Main app** - and *sub applications modules* - **Extension apps**. As example Main app, **Accounting** has as extension app, **Sale & Purchase Vouchers**, **Account Analityc Default**, etc.

>If you plan on developing the new application in Odoo, you should create boundaries for various features.

## Creating and installing a new add-on module
- A *module's technical name* must be a **valid Python identifier**. It must begin with a letter, and only contain letters, numbers, and underscore characters. It is preferable that you only use **lowercase letters** in the module name.
- Make the new module available in your Odoo instance. Log in to Odoo using admin, *enable Developer Mode* in the About box, and in the Apps top menu, select **Update Apps List**.

### New GitHub repository
I created new github repository `https://github.com/francescodattolo/local-addons.git`

I added references to `oca_dependencies.txt` in `addons/oca_dependencies.txt` *(/volumes/odoo/addons)*: 

```
# local-addons - From Odoo 14 development cookbook - 2107281257
local-addons https://github.com/francescodattolo/local-addons.git
```

### New Odoo module
The directory name that's used is the module's technical name. The name key in the module manifest is its title.

New module can be updated in an existing odoo database with the **Update Module List** menu

### Completing the add-on module manifest
[RST syntax](https://docutils.sourceforge.io/docs/user/rst/quickstart.html) or simple text, is used for *description* field in *manifest* file. 

To add an **icon** for the module, choose a **PNG image** to use and copy it to `static/description/icon.png` .

In the manifest file there are a few more keys that are frequently used.

The manifest file is a regular *Python dictionary*, with keys and values. The relevant keys are:

- name;
- summary;
- description;
- author;
- website;
- category;
- version;
- depends;
- data - a list of relative paths for the data files to load during module installation or upgrade;
- demo;

Other keys that are frequently used:

- licence;
- application - If this is `True` , the module is listed as an application. Usually, this is used for the **central module** of a functional area;
- auto_install;
- installable;
- external_dependencies;
- {pre_init, post_init, uninstall}_hook - This is a Python function hook that's called during installation/uninstallation;


### Organizing the add-on module file structure
### Adding models
Create library_book into the database.
The file views.py into the views directory can not be empty.

>Models have a few generic attributes prefixed with an underscore. The most important one is _name , which provides a unique internal identifier that will be used throughout the Odoo instance. The ORM framework will generate the database table based on this attribute. In our recipe, we used _name = 'library.book' . Based on this attribute, the ORM framework will create a new table called library_book . Note that the ORM framework will create a table name by replacing . with _ in the value of the _name attribute.

Changes to Odoo models are activated by upgrading the module. The Odoo server will handle the translation of the model class into database structure changes.

### Adding menu items and views
When a new model is added in Odoo, the user doesn't have any access rights by default.
We must define access rights for the new model in order to get access. In our example, we haven't defined any access rights, so the user doesn't have access to our new model.


**Data files can be placed anywhere inside the module directory**, but the convention is for the user interface to be defined inside a `views/` subdirectory. Usually, the name of these files is based on the name of the model. In our case, we are creating the user interface for the library.book model, so we created the `views/library_book.xml` file.

In general, data records are defined using a <record> tag, and we created a record for the `ir.actions.act_window` model in our example.
```
<record id='library_book_action' model='ir.actions.act_window'>
```

Similarly, **menu items** are stored in the `ir.ui.menu model`.
```
<record id="library_base_menu" model='ir.ui.menu model'>
...
```
However, there is a *shortcut* tag called `<menuitem>` available in Odoo, so we used this in our example

Finally are added two views, one for form change detail object and another for list - tree - view.
To display the values of the fields `author_id` in *tree view* (this field is **many2many** relation), I added to the *custom tree (list)* the following row:
`<field name="author_ids"/>`
then i decorated the field as show below:
`<field name="author_ids" widget="many2many_tags"/>`
**widget="many2many_tags"**
                

```
<field name="author_ids" widget="many2many_tags"/>
```

At end overrided the search view.

###Adding access security
When adding a new data model, you need to define who can create, read, update, and delete records.

- Create a file called security/groups.xml;
    - It defines a new security group by creating a res.groups record;
- Create a file ir.model.access.csv:
    - This file associates permissions on models with groups;

### Using the scaffold command to create a module
```$ ~/odoo-dev/odoo/odoo-bin scaffold my_module```






---

[![Francesco Dattolo](https://i0.wp.com/www.francescodattolo.it/wp-content/uploads/2019/09/cropped-francescodattolo-free_hand-logo-1.png)](https://francescodattolo.it)

*Francesco Dattolo*
*2107281622*
*2107281131*