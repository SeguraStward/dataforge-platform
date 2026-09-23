# Context
Devices send telemetry(device_id, timestamp, reading), and retries happen because of timeouts and network failures, so we must not create duplicate events(idempotency). Two devices, or many readings per device, must not be confused with each other.

# Options
- Identity by a device-supplied event_id. Pros: never collides. Cons: depends on every device implementing it correctly, and retries must reuse the same ID.
- Identity by device_id + high-precision timestamp (chosen). Pros: no device changes needed. Cons: collisions are possible if a device measures faster than its timestamp precision.
- Adding reading to the key (rejected). A retry with a corrected value would be stored as a new event, and identical real readings would be dropped.
- Duplicate handling: silently ignore, overwrite, or 409 (chosen for a different reading). Ignoring hides bugs, and overwriting destroys data and depends on arrival order.

# Decision

- Identity is device_id (string) + a timezone-aware UTC timestamp at microsecond precision (PostgreSQL timestamptz), plus a UNIQUE constraint on that pair.
- Write flow: INSERT . . . ON CONFLICT DO NOTHING. Inserted-> 201. Not inserted -> read the stored row and compare. Same -> 200. Different -> 409.
- Insert first, never "check then insert", because the check-then-insert version has a race between two simultaneous requests.
- Layers: the timestamp sanity check and the " are these two events equal?" comparison are domain (only the events' own data). The flow above is application, using a repository interface. The UNIQUE constraint and the ON CONFLICT SQL are infrastructure.

# Consequences 
- Good: atomic and race-free, retries are safe, and conflicts are visible.
- Bad: an extra SELECT on duplicates, the reliance on timestamp precision ( a known limit of this design), and float equality risk when comparing readings.