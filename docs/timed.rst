.. module:: itsdangerous.timed

Signing With Timestamps
=======================

If you want to expire signatures you can use the
:class:`TimestampSigner` class which adds timestamp information and
signs it. On unsigning you can validate that the timestamp is not older
than a given age.

.. code-block:: python

    from itsdangerous import TimestampSigner
    s = TimestampSigner('secret-key')
    string = s.sign('foo')

.. code-block:: python

    s.unsign(string, max_age=5)
    Traceback (most recent call last):
      ...
    itsdangerous.exc.SignatureExpired: Signature age 15 > 5 seconds

Units and limits of ``max_age``
-------------------------------

``max_age`` is a number of **seconds**. The age of a signature is the
difference between :meth:`TimestampSigner.get_timestamp` at unsign time
and the integer timestamp embedded when the value was signed, so it has
one second resolution. A value is rejected with
:exc:`~itsdangerous.exc.SignatureExpired` when its age is greater than
``max_age``.

A few consequences are worth knowing:

-   If ``max_age`` is ``None`` (the default) the age is not checked at
    all. The signature itself is still verified.
-   A timestamp from the future (a negative age, for example because
    the signing and verifying machines' clocks disagree) is also
    rejected with :exc:`~itsdangerous.exc.SignatureExpired` when
    ``max_age`` is given.
-   ``max_age`` limits how long a token is accepted. It is not
    revocation: a token that has not expired yet stays valid until it
    does. Rotate the secret key (see :ref:`key-rotation`) to invalidate
    every outstanding token.

.. autoclass:: TimestampSigner
    :members:

.. autoclass:: TimedSerializer
    :members:
