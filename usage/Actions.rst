Actions
========

Most of the time actions that you can apply on a canvas is what really matters.
For that Jagaur comes with a rich Actions Framework to make your life easier
and to save your time. and all actions are super fast and non of them is pixel
based.

Transformation 
---------------

The most efficient way to learn actions is to use the `Transformation`_ Object
which acts as Wraper for most used framework actions. 

The following example will demonstrate how to to resize an image and
watermark it using the transformation object 

.. code-block:: php

	<?php

	use Jaguar\Canvas,
		Jaguar\Transformation,
		Jaguar\Coordinate,
		Jaguar\Dimension;

	$actions = new Transformation(new Canvas('/images/actions-1.jpg'));
	$actions->resize(new Dimension(400, 400))
			->watermark(
					new Canvas('/images/jaguar-logo.png')
					, new Coordinate(20, 300)
			)
			->getCanvas()
			->save('/images/transformation-basic.jpg');


**Output :**

.. image:: /images/transformation-basic.jpg

Box Action 
----------------

`Box action` is one of the most interesting actions in Jaguar.this action can
apply any kind of actions on selected area of a canvas 

.. note::

	Some libraries call this action selection

The following example demonstrates the usage of the box action

.. code-block:: php

	<?php

	use Jaguar\Canvas,
		Jaguar\Action\BoxAction,
		Jaguar\Action\Color\Grayscale,
		Jaguar\Box,
		Jaguar\Dimension,
		Jaguar\Coordinate,
		Jaguar\Transformation;

	$canvas = new Canvas('/images/jaguar2.jpg');
	$w = $canvas->getWidth();
	$h = $canvas->getHeight();


	$box1 = new Box(
			new Dimension($w / 2, $h / 2)
			, new Coordinate(0, 0)
	);
	$box2 = new Box(
			new Dimension($w / 2, $h / 2)
			, new Coordinate($w / 2, $h / 2)
	);

	$actions = new Transformation($canvas);
	$actions->apply(new BoxAction(new Grayscale(), $box1))
			->apply(new BoxAction(new Grayscale(), $box2))
			->getCanvas()
			->save('/images/box-action.png');


**Output :**

.. image:: /images/box-action.png

Layers Effect (Overlay)
-----------------------

Jaguar can simulate the layer effect by using the `Overlay`_ action

Overlay has the effect that black background pixels will remain black,
white background pixels will remain white, but grey background pixels
will take the colour of the foreground pixel. 

.. note::

	Jaguar comes prepared with some preset actions which use the overlay 
	action heavily .
	
	Most current preset actions are inspired from Marc Hibbins(http://marchibbins.com/dev/gd)
	but with different preset files
	
	Current Preset Actions :
	,  `Canvas`_
	, `Chrome`_
	, `Dreamy`_
	, `Monopin`_
	, `Velvet`_
	, `Vintage`_
	
The following example demonstrates creating vignette filter using overlay
action with *vignette.gd2* preset which comes with Jaguar resources package

.. code-block:: php
	
	<?php

	use Jaguar\Canvas,
		Jaguar\Util,
		Jaguar\Transformation;

	$transformation = new Transformation(new Canvas('/images/actions-1.jpg'));
	$transformation->overlay(new Canvas(Util::getResourcePath('/Preset/vignette.gd2')))
			->getCanvas()
			->save('/images/overlay-action.jpg');

**Output :**

.. image:: /images/overlay-action.jpg
			
Creating New Actions 
-----------------------

Creating new actions is so easy too , all you need to do is to create a new class
which implements the `ActionInterface`_ or extends `AbstractAction`_.

The following example demonstrates creating RoundedCanvas Action

.. code-block:: php

	<?php
		
	use Jaguar\CanvasInterface,
		Jaguar\Drawable\Arc,
		Jaguar\Drawable\Style\FillStyle,
		Jaguar\Canvas,
		Jaguar\Dimension,
		Jaguar\Coordinate,
		Jaguar\Transformation,
		Jaguar\Action\AbstractAction;

	class CirledCanvas extends AbstractAction
	{

		/**
		 * Apply circled canvas action
		 *
		 * @param \Jaguar\CanvasInterface $canvas
		 */
		protected function doApply(CanvasInterface $canvas)
		{
			$dimension = $canvas->getDimension();
			$new = new Canvas($dimension);
			
			$arc = new Arc($dimension, new Coordinate(
					$dimension->getWidth() / 2
					, $dimension->getHeight() / 2
			));
			$arc->fill(true)->draw($new, new FillStyle($canvas));

			$canvas->setHandler($new->getHandler());
		}

	}

	$canvas = new Canvas(file_get_contents('/images/jaguar2.jpg'));

	$transformation = new Transformation($canvas);
	$transformation->resize(new Dimension(300, 300))
			->apply(new CirledCanvas())
			->getCanvas()
			->save('/images/circled-canvas.png');
			
**Output :**

.. image:: /images/circled-canvas.png

Edge Detections
------------------

Jaguar comes prepared with wide range of algorithms for `Edge Detections`_

Supported edge detections algorithms :

.. hlist::
   :columns: 3

   * GRADIENT_NORTH
   * GRADIENT_WEST		
   * GRADIENT_EAST		
   * GRADIENT_NORTH_EAST		
   * GRADIENT_NORTH_WEST		
   * GRADIENT_SOUTH_EAST		
   * GRADIENT_SOUTH_WEST		
   * LINE_HORIZONTAL		
   * LINE_VERTICAL		
   * LINE_LEFT_DIAGONAL		
   * LINE_RIGHT_DIAGONAL		
   * SOBEL_HORIZONTAL		
   * SOBEL_VERTICAL		
   * PREWITT_HORIZONTAL		
   * PREWITT_VERTICAL		
   * SCHARR_HORIZONTAL		
   * SCHARR_VERTICAL		
   * EMBOSS_NORTH		
   * EMBOSS_NORTH_EAST		
   * EMOBOSS_EAST		
   * EMBOSS_SOUTH_EAST		
   * EMBOSS_SOUTH		
   * EMBOSS_SOUTH_WEST		
   * EMBOSS_WEST		
   * EMBOSS_NORTH_WEST		
   * LAPLACIAN_FILTER1		
   * LAPLACIAN_FILTER2		
   * LAPLACIAN_FILTER3		
   * LAPLACIAN_FILTER4		
   * FINDEDGE

   
The following example will dimonstrate how to use create Pencil Action using
the *LAPLACIAN_FILTER3* edge detection algorithm
 
 .. code-block:: php
 
	<?php
	
	use Jaguar\Canvas,
		Jaguar\CanvasInterface,
		Jaguar\Action\AbstractAction,
		Jaguar\Action\EdgeDetection,
		Jaguar\Action\Color\Negate,
		Jaguar\Action\Color\Grayscale,
		Jaguar\Transformation;

	class PencilAction extends AbstractAction
	{

		/**
		 * Apply Pencil Action
		 * 
		 * @param CanvasInterface $canvas
		 */
		protected function doApply(CanvasInterface $canvas)
		{
			$transformation = new Transformation($canvas);
			$transformation
					->apply(new Grayscale())
					->apply(new EdgeDetection(EdgeDetection::LAPLACIAN_FILTER3))
					->apply(new Negate());
		}

	}

	$transformation = new Transformation(new Canvas('/images\actions-1.jpg'));
	$transformation->apply(new PencilAction())
			->getCanvas()
			->save('/images\pencil-action.jpg')
			->show();
			
**Output :**

.. image:: /images/pencil-action.jpg
.. -----------------------------------------------------
   Links 
   -----------------------------------------------------
   
.. _Transformation: ../_static/class-Jaguar.Transformation.html
.. _Box action: ../_static/class-Jaguar.Action.BoxAction.html
.. _Overlay: ../_static/class-Jaguar.Action.Overlay.html
.. _ActionInterface: ../_static/class-Jaguar.Action.ActionInterface.html
.. _AbstractAction: ../_static/class-Jaguar.Action.AbstractAction.html
.. _Canvas: ../_static/class-Jaguar.Action.Preset.Canvas.html
.. _Chrome: ../_static/class-Jaguar.Action.Preset.Chrome.html
.. _Dreamy: ../_static/class-Jaguar.Action.Preset.Dreamy.html
.. _Monopin: ../_static/class-Jaguar.Action.Preset.Monopin.html
.. _Velvet: ../_static/class-Jaguar.Action.Preset.Velvet.html
.. _Vintage: ../_static/class-Jaguar.Action.Preset.Vintage.html
.. _Edge Detections: ../_static/class-Jaguar.Action.EdgeDetection.html