.. _update-modes:

============
Update Modes
============

This is the documentation for the threaded version of the library.
In the past, the library could be ran in four distinguishable modes:

- With no extra threads at all.
- With an extra thread that receives everything as soon as possible (default).
- With several worker threads that run your update handlers.
- A mix of the above.

However, since the core rewrite to use ``asyncio``, this is no longer
possible. The library will always spawn the following threads:

- A thread to receive items from the network.
- A thread to send items across the network.
- A thread to handle incoming updates.
- A temporary thread to automatically reconnect, if enabled.

This simplifies how the library works, so developing merging changes
becomes easier, and users can benefit from quick merges with new features.

The only thing you need to know now is that if you want to receive updates,
you shouldn't let your script finish. You can use `run_until_disconnected
<telethon.client.updates.UpdateMethods.run_until_disconnected>` method for
this purpose:

.. code-block:: python

    from telethon import TelegramClient

    client = TelegramClient(...).start()

    # Some setup and preparation...

    # Now you want to listen for updates and not let your script finish.
    # For this you can run the client until it gets disconnected or if
    # Ctrl+C is pressed.
    client.run_until_disconnected()
