*Odoo 14 Development Cookbook*

**Chapther 1**

# Installing the Odoo Development Environment


## Odoo Ecosystem
Its powerful framework helps the developer to build projects very quickly.

## Odoo Editions
- Community Edition;
- Enterprise Edition;

## Comparison
https://www.odoo.com/it_IT/page/editions
**Affero General Public License version 3 (AGPLv3)**

## Git Repositories

## RunBot
Runbot is Odoo's automated testing environment.

## Odoo app store
https://apps.odoo.com/apps

*Some operations will definitely be easier if you have a GitHub account.*

> Viene nominata anche la libreria wkhtmltopdf, che ho utilizzato per produrre un PDF dall'HTML in Django. (wkhtmltopdf package, which is used in Odoo to print PDF documents such as sale orders, invoices, and other reports) Come anche viene utilizzato nodejs e npm per aggiungere il supporto RTL - rtlcss.

Odoo uses the **psycopg2** Python library to connect with a PostgreSQL database.

## Virtual environments
Python virtual environments, or venv for short, are isolated Python workspaces. These are very useful to Python developers because they allow different workspaces with different versions of various Python libraries to be installed, possibly on different Python interpreter versions.

## Managing Odoo server databases
When working with Odoo, **all the data in your instance is stored in a PostgreSQL database**.

## Storing the instance configuration in a file
```
To generate a configuration file for your Odoo instance, run the following command:
$ ./odoo-bin --save --config myodoo.cfg --stop-after-init
```
*In the Elicocorp docker image the runnable script odoo-bin is:*
```bash
/opt/odoo/sources/odoo/odoo-bin
```

## Activating Odoo developer tools
- Activate developer tools;
*?debug=1*
- Activate developer tools (with assets);
*?debug=assets*
- Activate developer tools (with test assets);



*Francesco Dattolo*
*2107280922*