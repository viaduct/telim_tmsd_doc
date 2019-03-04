``Track``: Placeholder of ``Item`` and its sub-containers
===============================================================

``Track`` is a placeholder type of ``Item``.

API
-----

``(constructor)(Track* parent)``
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

.. code-block:: cpp

	Track(Track* parent);

Initializes *parent* object. If this is a root Track, pass nullptr.

``trackInfo``
%%%%%%%%%%%%%%%%%

.. code-block:: cpp

	virtual TrackInfo const* trackInfo() const = 0;

Returns Track information object.

``parent``
%%%%%%%%%%%%%%%%%

.. code-block:: cpp

	Track* parent() const;

Returns parent object which passes to constructor previously.

``trackId``
%%%%%%%%%%%%%

.. code-block:: cpp

	TrackId trackId() const;

Returns *trackId* variable. The return value must be same with
``trackInfo()->trackId()``.

``start``
%%%%%%%%%%%

.. code-block:: cpp

	public:
		void start();
	protected:
		virtual void start_impl() = 0;

Starts the *Track* object. ``start`` will call ``start_impl`` inside.

When a *Track* object is started, ``stop`` must be called as well. Violation
will lead to undefined behavior.

``mimic`` related methods must be called after ``start`` is called.

``stop``
%%%%%%%%%

.. code-block:: cpp

	public:
		void stop();
	protected:
		virtual void stop_impl() = 0;

Stops the *Track* object and cleanup preparation done in ``start`` method.

``stop`` will call ``stop_impl`` inside.

``stop`` must cleanup all *Mimic* objects. Keep in mind when you are
subclassing.

``mimic``
%%%%%%%%%%

.. code-block:: cpp

	template <typename T> typename Chain<T>::Mimic* getMimic(int key)
	{
		return static_cast<typename Chain<T>::Mimic*>(getMimic(key));
	}
	virtual void mimic(int key, MimicryInfo* mInfo) = 0;
	virtual void stopMimic(int key) = 0;
	virtual void stopMimicAll() = 0;
	virtual Mimic* getMimic(int key) = 0;

Create, destroy, and fetch mimic objects.

``mimic`` creates a mimic object, and if the track instance has children,
call ``mimic`` of child track objects as well.

``stopMimic`` and ``stopMimicAll`` destroys mimic objects, and if the track
instance has children, call ``stopMimic`` or ``stopMimicAll``
of child track objects with same parameters.
