Color Model
=============

Jaguar supports one color model which is the *RGB* model becuase it is the
only supported model in `GD Library`_ which Jaguar is built on it . 

Beside the RGB model Jaguar supports the GD special colors which is probably 
you will never need to deal with them directly but they are used widely 
inside Jaguar to create new effects and patterns.

The following example demonstartes some of the `RGBColor`_ main methods

.. code-block:: php

	<?php
	
	use Jaguar\Color\RGBColor;

	// transparent(50) red 
	$rgb = new RGBColor(255, 0, 0, 50);

	// also transparent(50) red  but using hex format
	$hex = RGBColor::fromHex('#f00', 50);

	// also transparent(50) red  but using the value of allocated color
	$value = RGBColor::fromValue($hex->getValue());


	echo sprintf(
			"%s\n%s\n%s"
			, (string) $rgb
			, (string) $hex
			, (string) $value
	);

**Output :**

.. code-block:: none

	Jaguar\Color\RGBColor(855572480)[r=255,g=0,b=0,alpha=50]
	Jaguar\Color\RGBColor(855572480)[r=255,g=0,b=0,alpha=50]
	Jaguar\Color\RGBColor(855572480)[r=255,g=0,b=0,alpha=50]

.. -----------------------------------------------------
   Links 
   -----------------------------------------------------
   
.. _Gd Library : http://www.libgd.org
.. _RGBColor: ../_static/class-Jaguar.Color.RGBColor.html