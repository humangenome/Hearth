# Worlds and characters

How a Hearth world moves between servers, and how a character gets attached to
a player.

## How Hearth identifies a character

Bellwright restores a returning player from the login identity its network
layer sends. Hearth's direct join uses the engine's offline identity, which is
the computer's name plus that computer's machine ID (for example
`DESKTOP-ABC123-` followed by 32 letters and digits). Bellwright hashes that
identity to create or restore the persistent player record inside the world
save. Hearth also sends your Steam ID and display name, but Bellwright does not
use them to choose the character.

Two consequences follow from that:

- **The name box in Hearth is a display name, not a character picker.** Typing
  the name of a character that already exists in the world does not attach you
  to it. There is no character list, because Hearth never selects a character;
  Bellwright resolves one from the login identity.
- **A character belongs to the computer that created it.** Joining the same
  world from a different computer, or after renaming your computer, produces a
  new character. The old one is still in the world and comes back when you join
  from the original computer under its original name.

Hearth's per-server character memory in the launcher is the reverse of this: it
remembers which of *your* characters you last used for a saved server. It does
not, and cannot, hand you someone else's character.

### Importing someone else's world

Importing another player's world works. Their characters come with it, still
tied to the computers that made them, and you join as a new character in that
world. All world state (buildings, settlements, map progress, NPCs) carries
over; the personal character does not.

There is no supported way to move a saved character to a different computer or
account. Bellwright owns the persistent-player lifecycle and Hearth does not
rewrite player records inside a save.

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
