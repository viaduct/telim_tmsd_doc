``ItemType``: Factory for ``Item``
==========================================

``ItemType`` is an abstract factory for ``Item``.

Different from well-known factories, ``ItemType::createItem`` does one more
job: it calls ``Manager::addItem`` to add the newly created instance to
``Manager`` right away. This must be done right after the instance is created.
Keep in mind if you are subclassing ``ItemType``.

``ItemType`` may form tree structure in its subclass. For example, ``ObjType``
has children ``ContType``\ s, and they might have ``ItemType`` instances.
It's allowed to call other ``ItemType``\ 's ``createItem`` method, but
it must be done after ``Manager::addItem`` is called.

API
------------

``(constructor)(ItemRtti)``
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

.. code-block:: cpp

	ItemType(ItemRtti itemRtti);

Initializes *itemRtti*. This value can be fetched by ``itemRtti``.

``itemRtti``
%%%%%%%%%%%%%%%%

.. code-block:: cpp

	ItemRtti itemRtti() const;

Returns *itemRtti* value which is given to constructor before.

``createItem``
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

.. code-block:: cpp

	virtual Item* createItem(Manager* manager) const = 0;

Creates ``Item`` instance and add it to *manager* right away.

``createItem`` may call other ``ItemType``\ 's ``createItem`` inside.

Call ``Manager::addItem`` right after the instance is constructed.
