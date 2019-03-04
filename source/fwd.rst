Type Aliases and Forward Declarations
============================================

``ItemRtti``
---------------

In file ``item_rtti.h``

.. code-block:: cpp

	using ItemRtti = std::uint16_t;

``Item::itemRtti`` is a integer which is intended to hold user-defined
RTTI(Run-Time Type Information) id. TMSD does not try to set or initialize
this value, but will use it to compare whether 2 arbitrary items are in
same type or not.

This value is determined by ``ItemType::itemRtti`` value when an ``Item``
instance is constructed.

``MemberId``
-----------------

In file ``member_id.h``

.. code-block:: cpp

	using MemberId = std::uint16_t;

``Cont::memberId`` is a integer used as id of ``Obj`` member.

This value is determined by ``ContType::memberId`` value when a ``Cont``
instance is constructed.

``TrackId``
-------------

In file ``track_id.h``

.. code-block:: cpp

	using TrackId = std::uint16_t;

``Track::trackId`` is a integer which shows the ``Track`` instance's
position.

This value is determined by ``TrackInfo::trackId`` value when a ``Track``
instance is constructed.
