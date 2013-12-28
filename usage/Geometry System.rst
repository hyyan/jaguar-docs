Geometry System
===============

Jaguar uses a very simple geometry system to make things easier for
Jaguar and for the user . and this gemotry system is composed of 
three basic objects which are :

- `Coordinate`_  
				Composed of two integer *(x,y)* where x and y could be 
				negative numbers.
				
- `Dimention`_	 
				Composed of two integer *(width,height)*
				
- `Box`_ 
				 Composed of two objects *(Dimention,Coordinate)*
				 where dimension is a **Dimesnion Object** and
				 *coordinate* is a **Coordinate Object**

The following example demonstrates the usage of these objets.

.. code-block:: php
	
	<?php

	use Jaguar\Coordinate,
		Jaguar\Dimension,
		Jaguar\Box;

	$geometry = array(
		$c = new Coordinate(4, 3),
		$d = new Dimension(100, 100),
		$b = new Box($d, $c)
	);


	foreach ($geometry as $object) {
		echo sprintf("%s\n",(string) $object);
	}

**Output :**

.. code-block:: none

	Jaguar\Coordinate[x=4,y=3]
	Jaguar\Dimension[width=100,height=100]
	Jaguar\Box[Jaguar\Dimension[width=100,height=100],Jaguar\Coordinate[x=4,y=3]]



	
.. -----------------------------------------------------
   Links 
   -----------------------------------------------------
   
.. _Coordinate: ../_static/class-Jaguar.Coordinate.html
.. _Dimention: ../_static/class-Jaguar.Dimension.html
.. _Box: ../_static/class-Jaguar.Box.html