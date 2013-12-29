.. Jgaura - PHP Graphic Library documentation master file, created by
   Hyyan Abo Fakher on Sun Dec 22 18:55:08 2013.

Welcome to Jaguar - PHP Graphic Library's documentation!
========================================================

Jaguar is a PHP5.3 OOP graphic library was built on the Greate `GD Library`_ using 
the best practices to build a stable and fast layer for image manipulation and 
drawing.

**Something to mention**

Jaguar was built on php gd extension and this extension is so famous with its
*"limitations"* in the php community (Which is wrong by the way) like memory and 
transparent problems for example.

Thats's what Jaguar is for , overcome those *"limitations"* and let the programmer
focus on the his/her application logic instead of spending hours trying to prserve
the transparent of an image and finally moves to photoshop to solve the problem.

Installation
-------------

Installation via composer 

Simply add a dependency on hyyan/jaguar to your project's composer.json file. 
Here is a minimal example of a composer.json file that just defines 
a development-time dependency on PHPUnit 3.8:

.. code-block:: json

	{
		"require-dev": {
			"hyyan/jaguar": "1.0.*"
		}
	}
	
For a system-wide installation via Composer, you can run:

.. code-block:: json

	composer global require 'hyyan/jaguar=1.0.*'

.. note::

	Make sure you have *~/.composer/vendor/bin/* in your path If you
	choosed the system-wide installation.
	
Contribute
-----------

Your contributions are more than welcome !

Start by forking `Jaguar repository`_, write your feature, fix bugs, and send a pull 
request. If you modify Jaguar API, please update the API documentation in the
`Jaguar Docs repository`_ 
		   
..  note::
	
	Jaguar uses ApiGen project as documentation generator . take a look at
	the `ApiGen project`_

You can update API documentation by running :

.. code-block:: none

	$ git clone http://www.github.com/hyyan/jaguar-docs
	$ cd jaguar-docs
	$ apigen --config="./aguar-apigen.neon" 
		   --template-config="path/to/apigen/templates/bootstrap/config.neon"
	
.. note::

	Jaguar follows the Symfony standards If you’re a beginner, you will find 
	some guidelines about code contributions at `Symfony`_.
	
.. toctree::
   :maxdepth: 3
   
   usage/Geometry System
   usage/Colors
   usage/Canvas
   usage/Drawables
   usage/Gradients
   usage/Actions
   
Indices and tables
==================

* :ref:`genindex`
* :ref:`search`


.. _Gd Library : http://www.libgd.org
.. _Jaguar repository : http://www.github.com/hyyan/jaguar
.. _Jaguar Docs repository : http://www.github.com/hyyan/jaguar-docs
.. _ApiGen project : http://apigen.org/
.. _Symfony : http://symfony.com/doc/current/contributing/code/patches.html
