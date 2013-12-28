Drawables
==========

Jaguar provides a fully-featured drawing API which helps you to draw almost everything
from `pixels`_ to `polygons`_ using the simple `DrawableInterface`_

Drawable 
	every object which implements the `DrawableInterface`_ so it can be 
	drawn on a canvas. for example Rectangle is a drawables which implements the
	`DrawableInterface`_ 
	

Drawing 
---------

The following example demonstrates a random drawing example from the php manual using
Jaguar

.. code-block:: php

	<?php

	use Jaguar\Drawable\Pixel,
		Jaguar\Color\RGBColor,
		Jaguar\Dimension,
		Jaguar\Coordinate,
		Jaguar\Canvas;

	$w = 200;
	$h = 200;

	$canvas = new Canvas(new Dimension($w, $h));
	$pixel = new Pixel();
	$pixel->setColor(RGBColor::fromHex('#f00'));

	$corners[0] = array('x' => 100, 'y' => 10);
	$corners[1] = array('x' => 0, 'y' => 190);
	$corners[2] = array('x' => 200, 'y' => 190);


	for ($i = 0; $i < 100000; $i++) {

		$pixel->setCoordinate(new Coordinate(round($w), round($h)))
				->draw($canvas);

		$a = rand(0, 2);
		$w = ($w + $corners[$a]['x']) / 2;
		$h = ($h + $corners[$a]['y']) / 2;
	}

	$canvas->save('save/random-drawing.png')
			->show();

**Output :**

.. image:: /images/random-drawing.png

Drawing With Styles
-------------------

Drawing shapes and pixels using flat colors is good and nice but Jaguar supports
more than that (thanks to `GD Library`_) , it supports styles.

For now you can think of Jaguar styles as flat color replacement

.. note::
		Not all drawables supports styles some drawables like `Text`_
		for example can not be drawn with styles
		
.. seealso::

		`Drawing texts with drawers`_ 
		
The following example will use the last example of random drawing but with the
`FillStyle`_ this time to demonstrate the usage of styles.

.. code-block:: php

	<?php
	
	use Jaguar\Drawable\Pixel,
		Jaguar\Drawable\Style\FillStyle,
		Jaguar\Dimension,
		Jaguar\Coordinate,
		Jaguar\Canvas;

	$w = 200;
	$h = 200;

	$canvas = new Canvas(new Dimension($w, $h));
	$style = new FillStyle(new Canvas('/images/canvas-1.jpg'));
	$pixel = new Pixel();

	$corners[0] = array('x' => 100, 'y' => 10);
	$corners[1] = array('x' => 0, 'y' => 190);
	$corners[2] = array('x' => 200, 'y' => 190);

	for ($i = 0; $i < 100000; $i++) {

		$pixel->setCoordinate(new Coordinate(round($w), round($h)))
				->draw($canvas,$style);

		$a = rand(0, 2);
		$w = ($w + $corners[$a]['x']) / 2;
		$h = ($h + $corners[$a]['y']) / 2;
	}

	$canvas->save('images/random-darwing-with-style.png')
			->show();
			
**Output :**

.. figure:: /images/canvas-1.jpg	
	
	Draw Style
	
.. figure:: /images/random-drawing-with-style.png		
	
	Drawing Result 

Texts
-------

Texts is such a tough topic , actually it is not that easy as you might think and
the `Gd Library`_ comes with basic supports for texts, But Jaguar provides
a rich `Text`_ Object to help you overcome some limitations.

Although `Text`_ does not support styles but it supports text drawers.
a text drawers can be attached to the text object to draw outlined text for
example.

The following text drawers are supporte :

- `Shadow`_  : draw text with shadow
- `Outline`_ : draw outlined text
- `Plain`_   : the default drawer

Text Drawers
*************

The following example will draw text using both `Shadow`_ and `Outline`_ Drawers
to demonstare the usage of Text Drawers

.. code-block:: php

	<?php
	
	use Jaguar\Canvas,
		Jaguar\Drawable\Text,
		Jaguar\Drawable\Text\Outline,
		Jaguar\Drawable\Text\Shadow,
		Jaguar\Coordinate,
		Jaguar\Font,
		Jaguar\Color\RGBColor;

	$canvas = new Canvas('images/drawables-1.jpg');

	// orange outline where outline width = 2
	$outline = new Outline(RGBColor::fromHex('#f90'), 2);

	// orange shadow where xoffset = yoffset = 1 
	$shadow = new Shadow(RGBColor::fromHex('#f90'), 1, 1);


	// create white text to draw at (x=10,y=10)
	$text = new Text(
			'Jaguar'
			, new Coordinate(10, 320)
			, RGBColor::fromHex('#fff')
	);

	// draw "jaguar" word using both drawer with impact font
	$text->setFont(new Font('/fonts/impact.ttf', 20))
			->setDrawer($outline)->draw($canvas)
			->setDrawer($shadow)->draw($canvas)

			// draw "An Artist Touch" word in both drawers after translating 
			// the text coordinate by "20" for x and "20" for y with animeace2_bld 
			// font.
			
			->setString('An Artist Touch')
			->setCoordinate($text->getCoordinate()->translate(20, 30))
			->setFont(new Font('/fonts/animeace2_bld.ttf', 10))
			->setDrawer($outline)->draw($canvas)
			->setDrawer($shadow)->draw($canvas);

	$canvas->save('/images/text.jpg')->show();

		
**Output :**

.. image:: /images/text.jpg
		
Text Bounding Box
******************

Sometimes you might need to measure the text width and height or you might just want
to draw borders around the text or you might want ot create a simple cpatha project.
so you need a tool to help achive that.

The following texts will demonstrate using text bound feature in `Text`_ object

.. code-block:: php
	
	<?php
	
	use Jaguar\Canvas,
		Jaguar\Drawable\Text,
		Jaguar\Coordinate,
		Jaguar\Drawable\Rectangle,
		Jaguar\Color\RGBColor;

	$canvas = new Canvas('/images/drawables-2.jpg');

	$text = new Text(
			'Jaguar Touch'
			, new Coordinate(250, 250)
			, RGBColor::fromHex('#fff')
	);
	$text->setFontSize(15)->draw($canvas);


	// get bouding box
	$bound = $text->getBoundingBox();

	// draw text border
	$rect = new Rectangle(
			$bound->getDimension()
			, $bound->getCoordinate()
			, RGBColor::fromHex('#fff')
	);
	$rect->draw($canvas);

	$canvas->save('/images/text-bound.jpg')->show();

**Output :**

.. image:: /images/text-bound.jpg

Borders
---------
One of the main important features of the *Drawable* package is *Borders*
which you can do wonders with them.

Borders in Jaguars supports both styles and drawers.for styles any Jaguar
style can be applyed to the border and for drawers Jaguar support three 
drawers which are :

- `BorderIn`_ 
			The default drawer, the border will be drawn over the canvas
			with no dimension changes in canvas

- `BorderFit`_
			The canvas will be resized and the border will drawn around the
			new resized canvas but the result will be a canvas its dimension
			equals the orginal one.
			
- `BorderOut`_
			Border will be drawn out of the canvas and that will lead
			to changes in the canvas dimesnsion

Draw Raw Borders
*****************

The following example will demonstrate how to draw border out using the
border out drawer
	
.. code-block:: php

	<?php

	use Jaguar\Canvas,
		Jaguar\Drawable\Border,
		Jaguar\Drawable\Border\BorderOut,
		Jaguar\Color\RGBColor;

	$canvas = new Canvas('/images/drawables-3.jpg');

	$border = new Border(
			5 // border size
			, new RGBColor(41, 41, 41)
	);

	// draw border out
	$border->setDrawer(new BorderOut())->draw($canvas);
	$canvas->save('/images/border-out.jpg');

**Output :**

.. image:: /images/border-out.jpg

Draw Styled Border
******************

The following example demonstrates drawing styled border using the default
drawer (border in).

.. code-block:: php
	
	<?php

	use Jaguar\Canvas,
		Jaguar\Drawable\Border,
		Jaguar\Drawable\Style\FillStyle;

	$canvas = new Canvas('/images/drawables-4.jpg');

	$border = new Border(15);
	$border->draw(
			$canvas
			, new FillStyle(new Canvas('/images/style.jpg'))
	);

	$canvas->save('/images/border-in-styled.jpg');
	
**Output :**

.. image:: /images/border-in-styled.jpg
.. image:: /images/style.jpg


.. -----------------------------------------------------
   Links 
   -----------------------------------------------------
   
.. _DrawableInterface: ../_static/class-Jaguar.Drawable.DrawableInterface.html
.. _pixels: ../_static/class-Jaguar.Drawable.Pixel.html
.. _polygons: ../_static/class-Jaguar.Drawable.Polygon.html
.. _Gd Library : http://www.libgd.org
.. _FillStyle: ../_static/class-Jaguar.Drawable.Style.FillStyle.html
.. _Text: ../_static/class-Jaguar.Drawable.Text.html
.. _Shadow: ../_static/class-Jaguar.Drawable.Text.Shadow.html
.. _Outline: ../_static/class-Jaguar.Drawable.Text.Outline.html
.. _Plain: ../_static/class-Jaguar.Drawable.Text.Plain.html
.. _BorderIn: ../_static/class-Jaguar.Drawable.Border.BorderIn.html
.. _BorderOut: ../_static/class-Jaguar.Drawable.Border.BorderOut.html
.. _BorderFit: ../_static/class-Jaguar.Drawable.Border.BorderFit.html
.. _Drawing texts with drawers: #text-drawers