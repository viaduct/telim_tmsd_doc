``Mimic``
============================================================

API
-------------

``track``
%%%%%%%%%%%%%%

.. code-block:: cpp

	virtual Track* track() const = 0;

Returns owner track instance.

``parent``
%%%%%%%%%%%%%

.. code-block:: cpp

	Mimic* parent() const;

Returns parent mimic object.

The return object must be same with
``track()->parent()->getMimic(this->key())``.

``key``
%%%%%%%%%%%%

.. code-block:: cpp

	int key() const;
	void setKey(int one);

``userPtr``
%%%%%%%%%%%%%%%%5

.. code-block:: cpp

	void* userPtr() const;
	void setUserPtr(void* ptr);

	template <typename T> T* userPtr() const;
	template <typename T, typename Given> void setUserPtr(Given given);

Fetches or sets *userPtr* variable. TMSD never invokes any of these functions,
and it's up to library users how to make use of these.

They are intended to be used for static casting mimic object to user's type.
Read `Stackoverflow <https://stackoverflow.com/a/54819137/9919609>`_ for
more details. 
