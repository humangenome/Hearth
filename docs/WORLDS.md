# Worlds and characters

How a Hearth world moves between servers, and how a character gets attached to
a player.

## How Hearth identifies a character

Bellwright restores a returning player from the login identity its network
layer sends. On a Hearth server that identity is the PC's name plus its
machine ID (for example `DESKTOP-ABC123-` followed by 32 letters and digits),
and the world save keys each character on it.

From Hearth v0.1.97 and HearthServer v0.1.97, the character follows your Steam
account instead:

- When you press Connect, Hearth asks Steam for a ticket and the server checks
  it with Steam. The server then gives Hearth a one-use pass for that join.
- The server remembers the first PC each Steam account joined from. Join from
  another PC with a valid pass and the server gives you the character from
  that first PC.
- On its first start with this version, a server reads the last 14 days of its
  own logs and links each Steam ID to the earliest PC that still has a
  character there, so existing characters carry over.
- Without a valid pass (an older Hearth, Steam not answering, or an older
  server), the join works as before: the character belongs to the PC.

Two consequences follow:

- **The name box in Hearth is a display name, not a character picker.** Typing
  the name of a character that already exists in the world does not attach you
  to it. There is no character list, because Hearth never selects a character;
  Bellwright resolves one from the login identity.
- **One character per Steam account on each server.** If you played from two
  PCs and made two characters, you get the one from the PC you used first. The
  other stays in the world. A server admin can switch it: stop the server,
  change that Steam ID's line in `HearthServer\data\identity\steam-map.tsv` to
  the other PC's ID, then start it.

### Importing someone else's world

Importing another player's world works. Their characters come with it, and you
join as a new character in that world. Each of their players gets their
character back by making their first join from the PC they played on before.
All world state (buildings, settlements, map progress, NPCs) carries over; the
personal character does not.

Moving a character to a new PC needs nothing: join from it. There is no
supported way to move a character to a different Steam account. Bellwright
owns the persistent-player lifecycle and Hearth does not rewrite player
records inside a save.

## Moving a world between servers

Use the snapshot path, not a raw file copy.

1. On the source server, take a snapshot (`save snapshot` over RCON, or the
   launcher's world tools) and download it:
   `GET /api/v1/snapshots` to list, `GET /api/v1/snapshots/<id>/download` to
   fetch the zip.
2. On the destination server, upload and swap in one step:
   `POST /api/v1/snapshots/import-restore` with the zip as the request body.
   `POST /api/v1/snapshots` uploads without restoring if you want to stage it
   first, then `POST /api/v1/snapshots/<id>/restore`.

Both admin endpoints are HMAC-signed with a key derived from the server's
`RconPassword`.

### Why not FTP

HearthServer keeps a verified world baseline and an offline-player ledger so
that a crash, a bad rotation, or a Bellwright autosave that drops offline
players cannot silently destroy a populated world. The restore path clears that
baseline and ledger as part of the swap, so the incoming world becomes the new
protected state.

Copying save files in over FTP skips that reset. The protection layer still
holds the previous world's baseline, sees the new world as a regression against
it, and can refuse the launch or restore the old world over your import. Take
the world offline and use the snapshot restore path instead.
