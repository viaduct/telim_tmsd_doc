``TrackInfo``: Informations Used to Create ``Track`` Tree
====================================================================

``TrackInfo`` is an abstract type which contains necessary data to form
tree of ``Track`` instances.

``TrackInfo`` itself only has ``trackId`` variable. Other data will be stored
in its subclasses.

API
------

``(constructor)TrackInfo(TrackId id)``
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

.. code-block:: cpp

	TrackInfo(TrackId id);

Initializes *trackId* variable. This can be fetched by ``trackId`` getter.

``trackId``
%%%%%%%%%%%%%

.. code-block:: cpp

	TrackId trackId() const;

Returns *trackId* which is given to constructor before.
