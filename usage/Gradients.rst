Gradients
=========

Let's face it , gradient is a necessary feature in any graphic library , it 
is important to generate styles and some kind of advanced image effects like
Reflection for example. for that Jaguar provides a simple gradient generator
which supports four kind of gradients which are :

- `CircleGradient`_    
					 Same As Radial Gradient
- `LinearGradient`_     
					 Supports both vertical and horizontal gradients
- `RectangleGradient`_
- `DiamondGradient`_

The following example deomnstrates generating the four kinds of gradients.

.. code-block:: php
	
	<?php

	use Jaguar\Canvas,
		Jaguar\Color\RGBColor,
		Jaguar\Dimension,
		Jaguar\Gradient\LinearGradient,
		Jaguar\Gradient\RectangleGradient,
		Jaguar\Gradient\CircleGradient,
		Jaguar\Gradient\DiamondGradient;

	$canvas = new Canvas();

	$start = RGBColor::fromHex('#000');
	$end = RGBColor::fromHex('#fff');

	$gradients = array(
		'linear-h' => new LinearGradient(LinearGradient::GRADIENT_HORIZONTAL, $start, $end),
		'linear-v' => new LinearGradient(LinearGradient::GRADIENT_VERTICAL, $start, $end),
		'diamond' => new DiamondGradient($start, $end),
		'rectangle' => new RectangleGradient($start, $end),
		'circle' => new CircleGradient($start, $end),
	);


	foreach ($gradients as $name => $gradient) {
		$canvas = $canvas->create(new Dimension(100, 100));
		$gradient->generate($canvas);
		$canvas->save(sprintf('images/%s.png', $name));
	}
	
**Output :**

.. image:: /images/linear-h.png
.. image:: /images/linear-v.png
.. image:: /images/diamond.png
.. image:: /images/rectangle.png
.. image:: /images/circle.png
	
.. _CircleGradient: ../_static/class-Jaguar.Gradient.CircleGradient.html
.. _LinearGradient: ../_static/class-Jaguar.Gradient.LinearGradient.html
.. _RectangleGradient: ../_static/class-Jaguar.Gradient.RectangleGradient.html
.. _DiamondGradient: ../_static/class-Jaguar.Gradient.DiamondGradient.html