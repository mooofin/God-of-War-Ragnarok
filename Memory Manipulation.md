In windows there are two primary ways to play around with another process memory:

Use CreateRemoteThread to run code in the context of the target process

Use Read/writeprocessmemory to directly meddle with the taget process.

If cheatEbgine(or any other given process) is running with administrative pivilagaes this is easily achievable.pretty sure you don't even need admin to target most non-admin processes.
Every program has a memory space that is isolated in that another program will never accidentally write into its memory space. Most operating systems, however, give you the ability to hook into another process’s memory for debugging purposes.

The way cheat engine works is off of a concept called offsets. The exact memory addresses of the running game will change after every launch, so instead of figuring those out we just figure out the “offset”. In essence, get the base address of the process then add X to it. Depending on what X is, you get different values.

This way, you can reuse the same offsets and simply hook into the programs with an additions
