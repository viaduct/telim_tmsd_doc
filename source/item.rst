``Item``: Basic Data Unit
================================

``Item`` is an abstract type which holds data.

By itself, it contains no data at all, and that's subclasses' job.

It introduces abstract data-modifying functions such as ``stopRefer`` and
``stopReferAll``. This functions returns informations for what they actually
have done, so that the jobs can be undone.

An ``Item`` instance can be expired, means being deleted in most cases, by
``Manager``. ``Manager`` will call ``emitExpired`` before it becomes
inaccessable, and before it returns ``handleItemEvent`` with *Expire_B*
event type will be called.

API
---------

``(constructor)(Manager* manager)``
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

.. code-block:: cpp

	Item(Manager* manager);

Initializes *manager* variable. This can be fetched by ``manager``.

``manager``
%%%%%%%%%%%%%%%%

.. code-block:: cpp

	Manager* manager() const;

Returns *manager* object which is passed to constructor before.

``stopRefer``
%%%%%%%%%%%%%%%%

.. code-block:: cpp

	virtual void stopRefer(Item* item, StopReferInfo** sri) = 0;
	virtual void stopReferAll(StopReferInfo** sri) = 0;

Creates ``StopReferInfo`` command object and run it right away.

It will look for the same item and add it to stop-refer-target list. Though
there's no matching item, it will still return command object.

Command object must be deleted manually.

``stopReferAll`` stops referring for all items.

``referredItems``
%%%%%%%%%%%%%%%%%%%%%%

.. code-block:: cpp

	virtual size_t referredItemsSize() const = 0;
	virtual std::vector<Item*> referredItems() const = 0;

Returns all other items which the ``Item`` instance is referring.

Any of item in this list instances must not be invalidated.

``referredItemsSize`` returns the number of items of ``referredItems``.
``referredItemsSize() == referredItems().size()`` must be valid.

There can be duplicated items in the list.

``createTrack``
%%%%%%%%%%%%%%%%%%%

.. code-block:: cpp

	virtual ItemTrack* createTrack(
		Track* parent,
		ItemTrackInfo const* trackInfo
		) = 0;

Creates a matching ``ItemTrack`` instance.

*parent* and *trackInfo* are used for ``ItemTrack``\ 's constructor parameter.

Concrete type of return object is matching type of the ``Item`` instance:
for example, if the concrete type is ``Obj``, the return object will be
``ObjTrack``\ 's instance.

``itemType``
%%%%%%%%%%%%%%%%%%%%

.. code-block:: cpp

	virtual ItemType const* itemType() const = 0;

Returns matching ``ItemType``.

Subclass may have its ``ItemType`` instance. This function returns that.

Concrete type of return object is matching type of the ``Item`` instance:
for example, if the concrete type is ``Obj``, the return object will be
``ObjType``\ 's instance.

``itemRtti``
%%%%%%%%%%%%%%%%%%

.. code-block:: cpp

	ItemRtti itemRtti() const;

Returns matching *itemRtti* value.

Return value must be exactly same with ``itemType()::itemRtti()``.

``emitExpire``
%%%%%%%%%%%%%%%%%%

This is intended to be called from ``Manager::destroy``. You may don't need
to use this.

``emit_Refer_``
%%%%%%%%%%%%%%%%%%%%

.. code-block:: cpp

	void emitRefer(Item* referred);
	void emitStopRefer(Item* referred);
	void emitReferred(Item* referring); // Deprecated.
	void emitStopReferred(Item* referring); // Deprecated.

Do not use ``emitReferred`` and ``emitStopReferred``. They are deprecated.

``emitRefer`` and ``emitStopRefer`` need to be called in ``Item``\ 's
subclass whenever it starts or stops referring other items.

``handleItemEvent``

.. code-block:: cpp

	virtual void handleItemEvent(ItemEvent const*);

Whenever an ``Item`` instance wants to propagate its event to its subclass,
``handleItemEvent`` is called. One example is ``ItemEvent`` with ``Expire_B``
type.

Override this in subclasses to listen events.


``gcPtr``
%%%%%%%%%%%%

.. code-block:: cpp

	void* gcPtr() const;
	void setGcPtr(void* one);

This is one pointer-sized data intended to be used for garable collector.
TMSD never invokes these functions.
