``Manager``\ : Root Object
============================

``Manager`` is the topmost object in TMSD library. It takes ownership of
all dynamic objects related to TMSD library, and deleting the manager
ensures that there's no memory leak at last.

One job the ``Manager`` does is constructing and destructing instance of
``Item``. All instances created from ``Manager::create()`` are owned
by ``Manager``, and automatically deleted when ``Manager``\ 's destructor
is called. ``destroy()`` can be used to delete an instance.

TMSD is a library which concentrates at detecting data modification, and
it does not provide any garbage collection feature. Though, TMSD considered
that external garbage collector can be implemented, and provides functions
and callbacks which can be helpful. Read :ref:`Garbage Collection` for
more details.

Files
---------

* ``manager.h``
* ``manager.cpp``
* ``manager.tpp``

API
---------

``create``
%%%%%%%%%%%%%%%%%%

.. code-block:: cpp

	template <tyepname TType>
	typename Chain<TType>::Data* create(TType const* type);

Takes a ``ItemType``, and create ``Item`` instance. The instance is owned
by ``Manager`` and returned. Return object is casted to the matching
``Chain::Data`` type.

``destroy``
%%%%%%%%%%%%%

.. code-block:: cpp

	void destroy(Item* item)

Takes an ``Item`` instance and remove it from the ``Manager``. It will be
deleted as well, and it must not be dereferred.

The instance will have ``Expire_B`` event before it's invalidated.

``setGcObserver``
%%%%%%%%%%%%%%%%%%%%%%

.. code-block:: cpp

	void setGcObserver(GcObserver* obs)

Sets new ``GcObserver`` so that the caller can take
garbage-collection-related events.

DO NOT SET NULLPTR to unset. In most cases, unsetting will not be needed,
and a function for that is not provided. If it's required, call
``setGcObserver(NullGcObserver::null())`` or your own ``GcObserver`` instance
which shows null behavior.
