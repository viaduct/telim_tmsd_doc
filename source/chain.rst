Chain
=============

Chain is a TMP implementation which groups related types together.

There are 5 major types in TMSD, ``Item``\ (or ``Cont``),
``ItemType``\ (or ``ContType``), ``Track``, ``TrackInfo``, and ``Mimic``.
They are abstract types, and intended to be inherited. Their subclasses
have matching, another subclass from different ancestor: for example,
if there's ``Obj[Item]``, which is subclass of ``Item``, and there are
``ItemType``\ 's subclass ``ObjType``, ``ItemTrack``\ 's subclass
``ObjTrack``, ``ItemMimic``\ 's subclass ``ObjMimic``, and so on. Chain has
type informations and can be used to find another, related type which
has given ancestor.

The example below is the actual implementation of ``Chain<Obj>``. See
``chain.h`` for full details.

.. code-block:: cpp

	// Default Chain template, which will be specialized.
	template <typename T>
	struct Chain
	{
	};

	// Concrete, specialized template, which has all type infomations.
	template <>
	struct Chain<Obj>
	{
		using Type = ObjType;
		using Data = Obj;
		using Track = ObjTrack;
		using TrackInfo = ObjTrackInfo;
		using Mimic = ObjMimic;
	};

	// Providing ObjType will get the Chain<Obj> template implementation.
	template <>
	struct Chain<typename Chain<Obj>::Type> : Chain<Obj>
	{
	};

	// Providing ObjTrack will get the Chain<Obj> template implementation.
	// And so on...
	template <>
	struct Chain<typename Chain<Obj>::Track> : Chain<Obj>
	{
	};

	template <>
	struct Chain<typename Chain<Obj>::TrackInfo> : Chain<Obj>
	{
	};

	template <>
	struct Chain<typename Chain<Obj>::Mimic> : Chain<Obj>
	{
	};
