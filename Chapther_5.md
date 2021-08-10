*Odoo 14 Development Cookbook*

**Chapther 5**

# Basic Server-Side Development

## Defining model methods and using API decorators
The `@api.model` decorator is similar to `self` parameter. `self` can referer to arbitrary number of database records, at same way the `@api.model` referers to a generic method callable by another.

`@api.model` acts only on one record.


## Reporting errors to the user
In Odoo, the **RPC** (remote procedure call) layer that answers the calls made by the web client catches all exceptions and, depending on the **exception class**, **triggers** different possible behaviors on the web client.

If the exception is not defined in `odoo.exceptions` will be handled as internal server error.

**UserError** will display an error message in the *user interface*. The code of the recipe raises UserError to ensure that the message is displayed in a user-friendly way. *In all cases, the current database transaction is rolled back.*

## Obtaining an empty recordset for a different model

[...]

## Extending the business logic defined in a model
It is a very common practice in Odoo to divide application features into different modules. 


---

[![Francesco Dattolo](https://i0.wp.com/www.francescodattolo.it/wp-content/uploads/2019/09/cropped-francescodattolo-free_hand-logo-1.png)](https://francescodattolo.it)

*Francesco Dattolo*

*2108081417*