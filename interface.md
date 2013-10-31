
The Common Connect-Four Interface
=================================

This document describes The Common Connect-Four Interface (CCFI),
a communication protocol to connect a Connect-Four engine and an application.

General Requirements
--------------------

* A CCFI engine will be a standard executable for the host operating system.
  All communication will be carried out through standard input and output using
  ASCII encoding.

* The engine must always be ready to process input.

* All commands will end with a single '\n', appropriate for the host operating
  system. Neither the engine nor the calling program will use '\n' during communication
  for any purpose other than to indicate the end of a command.

* Engines may not carry out any computation until commanded, and must cease
  promptly after being commanded.

* Engines and calling programs must attempt to handle unexpected or badly formed
  commands gracefully. If an unknown command is seen, it should be ignored and the
  rest of the command should be processed.


Board Conventions
-----------------

A standard board size of 6 rows and 7 columns will be assumed. The bottom row will
be row 0 and the top will be row 5. The furthest left column will be column 0 and
the furthest right column will be column 6.

Position Format
---------------

The position used to describe the Connect-Four board is based on the FEN notation
used in chess. The players will be represented by the ASCII tokens 'x' and 'o'.
The player with the token 'x' moves first.

* The format first lists piece placement. Each row is described from
  row 0 to row 5; each column is described from column 0 to column 6. If a location
  is occupied, it will be denoted using the ASCII token of the occupant. Unoccupied
  locations are noted by the number of blank locations. A single unoccupied location
  is denoted as '1'. Each row will be separated by a single '/'.

* Following whitespace, the player to move is denoted using their token.

As an example, the following would be the position if the first player had played
in column 1:
`1x5/7/7/7/7/7 o`

Application to Engine
---------------------

* `newgame`
  This command informs the engine that a new game is being started.

* `position ...`
  The command position will be followed by a position format string as described above.
  The command `position` will be separated from the string by exactly 1 space character.
  The engine should prepare to calculate on the given position.

* `go`
  Informs the engine that it should begin calculating on the given position.

* `quit`
  Quit as soon as possible.

Engine to Application
---------------------

* `bestmove <col#>`
  Sent by the engine when it has completed searching.

* `info ...`
  The engine can use the `info` command to send useful information to the application
  while searching. The command should be followed by arbitrary information the engine
  thinks would be of interest. The `info` command may only be sent while a search is underway,
  and the last `info` must be sent before the engine sends `bestmove`. An example of useful
  information would be the engine evaluation of the current score.
