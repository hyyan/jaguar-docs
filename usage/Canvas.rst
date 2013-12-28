Canvas
=======

`Canvas`_ is the core of Jaguar which makes applying filters , drawing shapes 
and loading images from files or strings possible.it the warpper for all kinds of 
supported formats in Jaguar which are :

- `Jpeg`_ : Support *jpeg* format
- `Png`_  : Support *png* format
- `Gif`_  : Support *gif* format
- `Gd`_   : Support *Gd2* format

Probably you will never need to deal directly with any of those formats but you
will only deal with `Canvas`_ object which is smart enough to guess the right format
for a given image file or image string .
But if you want to declare the formate by your own you can do it is straightforward

Creating New Canvas
--------------------

The folowing example demonstrates creating a png canvas (w=100,h=100) filled
with red color 

.. code-block:: php
	
	<?php
	
	use Jaguar\Canvas,
		Jaguar\Dimension,
		Jaguar\Color\RGBColor;

	// We want png canvas (by default png is the default format )
	$canvas = new Canvas(
		new Dimension(100, 100)
		, Canvas::Format_PNG
	);
	
	$canvas->fill(new RGBColor(255))
		   // save the canvas to the given path
		   ->save('/save/path')
           // send the canvas to browser		   
		   ->show();   

**Output :**

.. image:: /images/red-canvas.png

Loading Canvas From Files Or Strings
-------------------------------------

As you can see Jaguar's canvas can create new image with no effort
and also it can load images from files and string easily.

The following example will demonstrate loading images from a file.

.. code-block:: php
	
	<?php
	
	use Jaguar\Canvas;

	// No need to define the format 
	// canvas will make a guess for you
	// and find the right format
	$canvas = new Canvas('images/canvas-1.jpg');

	$canvas->save('/save/canvas-1.jpg')
			->show();


**Output :**

.. image:: /images/canvas-1.jpg
			

Adding New Foramts
-------------------

Canvas object is not limited to the current formats and it can be extended to 
support more by creating a new canvas which implements `CanvasInterface`_ 
or exteds the `AbstractCanvas`_  and a new format which implements the 
`CanvasFactory`_

The following example demonstrates createing a new format called *Foo*

.. code-block:: php

	<?php
	
	use Jaguar\AbstractCanvas,
		Jaguar\CanvasFactory,
		Jaguar\Canvas;

	/**
	 * Foo Format
	 */
	class Foo extends AbstractCanvas
	{

		protected function doLoadFromFile($file)
		{
			//put your code here
		}

		protected function doSave($path)
		{
			//put your code here
		}

		protected function getToStringProperties()
		{
			return array();
		}

	}

	/**
	 * Foo Format Factory
	 */
	class FooFactory implements CanvasFactory
	{

		public function getCanvas()
		{
			return new Foo();
		}

		public function getExtension($includeDot = true)
		{
			return ($includeDot) ? '.foo' : 'foo';
		}

		public function getMimeType()
		{
			return 'image/foo';
		}

		public function isSupported($file)
		{
			// check if the Foo format can handle
			// the given file
		}

	}

	/* add my new format to the canvas */
	$canvas = new Canvas();
	$canvas->addFactory('Foo', new FooFactory());

	print_r($canvas->getFactories());
	
**Output :**

.. code-block:: none

	Array
	(
		[factory.jpeg] => Jaguar\Factory\JpegFactory Object()
		[factory.gif] => Jaguar\Factory\GifFactory Object()
		[factory.png] => Jaguar\Factory\PngFactory Object()
		[factory.gd2] => Jaguar\Factory\GdFactory Object()
		[Foo] => FooFactory Object()

	)
	
.. -----------------------------------------------------
   Links 
   -----------------------------------------------------
   
.. _Canvas: ../_static/class-Jaguar.Canvas.html
.. _Jpeg: ../_static/class-Jaguar.Format.Jpeg.html
.. _Png: ../_static/class-Jaguar.Format.Png.html
.. _Gif: ../_static/class-Jaguar.Format.Gif.html
.. _Gd: ../_static/class-Jaguar.Format.Gd.html
.. _CanvasInterface: ../_static/class-Jaguar.CanvasInterface.html
.. _AbstractCanvas: ../_static/class-Jaguar.AbstractCanvas.html
.. _CanvasFactory: ../_static/class-Jaguar.CanvasFactory.html