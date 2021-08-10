*Odoo 14 Development Cookbook*

**Chapther 4**

# Application Models
We will examine the use of *new field* and *constraints*, *inheritance* in Odoo.

- Adding relational fields to a model;
- Adding a hierarchy to a model;
- Adding constraint validations to a model;
- Adding dynamic relations using reference fields;
- [Adding features to a model using inheritance](#adding-features-to-a-model-using-inheritance);
- Using abstract models for reusable model features;
- Using delegation inheritance to copy features to another model;

## Technical requirements
Code source
[https://github.com/PacktPublishing/Odoo-14-Development-Cookbook-Fourth-Edition/tree/master/Chapter04](https://github.com/PacktPublishing/Odoo-14-Development-Cookbook-Fourth-Edition/tree/master/Chapter04)

## Defining the model representation and order
Models have structural attributes for defining their behavior. These are prefixed with an underscore. The most important attribute of the model is **_name** , as this defines the internal global identifier.
The _name attribute must be unique across Odoo.

But there are also:
- **_rac_name** is used to set the field that's used as a representation or title for the records;
- **_order** is used to set the order in which the records are
presented;

In short, **_rec_name** is the display name of the record used by *Odoo GUI* to represent that record. By default, the name field is used. In fact, this is the default value for the `_rec_name` attribute, **which is why it's convenient to have a name field in our models**. (It is similar to the `__str__` method in *Django* framework)


## Adding data fields to a model
Many attributes of fields, please check the book. Page of pdf 80.

## Using a float field with configurable precision

Access the decimal precision configurations. To do this, open the Settings top menu and select `Technical | Database Structure | Decimal Accuracy` . We should see a list of the currently defined settings.

When you add a string value to the **digits** attribute of the **field**, Odoo looks up that string in the decimal accuracy model's Usage field and returns a tuple with *16-digit* precision and the number of decimals that were defined in the configuration.

## Adding a monetary field to a model

**Monetary** fields are similar to *float fields*, but Odoo is able to represent them correctly in the user interface since it knows what their currency is through the *second field*.

You can omit the currency_field attribute from the monetary field if you are storing your currency information in a field with the name `currency_id` .

They won't be visible in views until they are added to them, but we can confirm their addition by inspecting the model fields in `Settings | Technical | Database Structure | Models` in **developer mode**.

This is very useful when you need to maintain the amounts in different currencies in the same record.

## Adding relational fields to a model
**Relations between Odoo** models are represented by *relational fields*. 
There are three different types of relations:
- **many-to-one**, commonly abbreviated as m2o
- **one-to-many**, commonly abbreviated as o2m
- **many-to-many**, commonly abbreviated as m2m

**Many-to-one** relation has a **one-to-many** as *reverse relation*.


The `_inherit` attribute we use here is for inheriting an existing model.

**Attributes of relation**:
- index: True - create an index into the database;
- ondelete:
    - set null - sets an empty value on the field;
    - restrict - prevents the related record from being deleted;
    - cascade - causes the linked record to also be deleted; 
- context - *used in client view* - adds variables to the client context when clicking through the field to the related record's view;
- domain - *used in client view* - is a search filter that's used to limit the list of related records that are available;

**One-to-many** fields need a **many-to-one** field in the reference
model. Although **One-to-many fields** are added to models just like other fields, they have no actual representation in the database.

For **many-to-many** relations, Odoo automatically handles the creation of this relation table.



## Adding a hierarchy to a model
**Hierarchies** are represented like a model having relations with the same model. Each record has a parent record in the same model, and many child records. This can be achieved by simply using many-to-one relations between the model and itself.

However, **Odoo** also provides improved support for this type of field by using the nested set model [Nested set mode]( https://en.wikipedia.org/wiki/Nested_set_model ).
**When activated, queries using the child_of operator in their domain filters will run significantly faster.**

## Adding constraint validations to a model
Odoo supports two different types of constraints:
- The ones checked at the database level;
- The ones checked at the server level;

We will add a **database constraint** that prevents *duplicate book titles*, and a **Python model constraint** that prevents release *dates in the future*.

The `_sql_constraints` model attribute accepts a list of constraints to create. Each constraint is defined by a **three-element tuple**. These are listed as follows:
- A suffix to use for the constraint identifier. In our example, we used **name_uniq**, and the resulting constraint name (label) is **library_book_name_uniq**;
- The **SQL** to use in the PostgreSQL instruction to alter or create the database table;
- A **message** to report to the user when the constraint is violated;

The `Python code` uses the decorator **@api.constrains**. If the check fails, a **ValidationError exception** will be raised.

## Adding computed fields to a model
The value of a computed field is calculated or derived from other fields in the same record or in related records.
**It is also possible to make computed fields editable and searchable.**

The definition of a computed field is the same as that of a regular field, except that a compute attribute is added to specify the name of the method to use for its computation.

Computed fields are dynamically calculated at runtime, and because of that, they are not stored in the database and so you cannot search or write on compute fields by default.

The ORM uses **caching** to avoid *inefficiently* recalculating it every time its value is accessed.
### Caching ###
So the `@depends` decorator to detect when its cached values should be invalidated and recalculated at *runtime*.
**Odoo v13** introduced a **new caching mechanism for ORM**. For specific context It uses `@api.depends_context`.

### Attributes ###
The optional `store=True` flag stores the field in the database. *You can think of it as a persistent cache.*

The `compute_sudo=True` flag is to be used in cases in which the computations need to be done with elevated privileges.

## Exposing related fields stored in other models
**Client-side code, unlike server-side code, can't use dot notation to access data in the related tables**. These fields can be made available there by adding them as *related fields*.

## Adding dynamic relations using reference fields
With relational fields, we need to decide the relation's target model (or co-model) beforehand. However, sometimes, we may need to leave that decision to the user and first choose the model we want and then the record we want to link to.
**With Odoo, this can be achieved using reference fields**.

## Adding features to a model using inheritance
One of the most important Odoo features is the ability of module add-ons to extend features that are defined in other module add-ons without having to edit the code of the original feature.

According to the official documentation, Odoo provides three types of inheritance:
- [Class inheritance (extension)](#class-inheritance-extension)
- [Prototype inheritance](#copy-model-definition-using-inheritance)
- [Delegation inheritance](#using-delegation-inheritance-to-copy-features-to-another-model)

### Class inheritance (extension)
When a model class is defined with the `_inherit` attribute, **it adds modifications to the inherited model**, rather than replacing it.
**This means that fields defined in the inheriting class are added or changed on the parent model**. At the database layer, the ORM is adding fields to the same database table.

`_inherit = 'res.partner'`

[Go to **Adding features to a model using inheritance**](#adding-features-to-a-model-using-inheritance)


### Copy model definition using inheritance
**Prototype inheritance**, which is used to copy the entire definition of the existing model.

**Prototype inheritance is executed by using the _name and _inherit class attributes at the same time.**

It is a valid choise for duplicate the defination of the model.
The model is copied into the database.

Prototype inheritance copies all the properties of the parent class. It copies fields, attributes, and methods. If you want to modify them in the child class, you can simply do so by adding a new definition to the child class.


[Go to **Adding features to a model using inheritance**](#adding-features-to-a-model-using-inheritance)

### Using delegation inheritance to copy features to another model

The **Delegation inheritance** is used to inherit the class attributes without duplicating data structures. 

>If you want **to copy a model's definitions without duplicating data structures**, then the answer lies in Odoo's delegation inheritance, which **uses the `_inherits` model attribute (note the additional s )**.

It also supports **polymorphic inheritance**, where we inherit from two or more other models.

The `_inherits` model attribute sets the parent models that we want to inherit from. Its value is a *key-value dictionary*, where the **keys are the inherited models**, and the **values are the field names that were used to link to them**.

**It's important to note that delegation inheritance only works for fields, and not for methods.**

#### Delegation attribute 

Instead of creating an **_inherits dictionary**, you can use the `delegate=True` attribute in the *Many2one* field definition.

```python
class LibraryMember(models.Model):
    _name = 'library.member'
    partner_id = fields.Many2one(
        'res.partner',
        ondelete='cascade', 
        delegate=True)
    [...]
```
[Go to **Adding features to a model using inheritance**](#adding-features-to-a-model-using-inheritance)
## Using abstract models for reusable model features

Abstract models allow us to create a generic model that implements some features that can then be inherited by regular models in order to make that feature available.

As an example, we will implement a simple *archive* feature.

An abstract model is created by a class based on models.AbstractModel , instead of the usual models.Model. It has all the attributes and capabilities of regular models; the difference is that the ORM will not create an actual representation for it in the database.
This means that it can't have any data stored in it. It only serves as a template for a reusable feature that is to be added to regular models.



---

[![Francesco Dattolo](https://i0.wp.com/www.francescodattolo.it/wp-content/uploads/2019/09/cropped-francescodattolo-free_hand-logo-1.png)](https://francescodattolo.it)

*Francesco Dattolo*

*2108021415*

*2108021154*

*2107291717*

*2107281625*
