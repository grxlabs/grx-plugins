# Identity, pending records and merges: checklist

How persons are created, merged and kept apart, and every tool below: https://business.gymrankx.com/docs/intelligence/concepts/people-and-identity.md

1. list_intel_pending (optionally with source_id): records pending identity grouped by identifier set, open conflicts, counts per kind.
2. Find why a record is pending: the mapping missed an id column (fix it with references/mapping.md: publish_intel_mapping already reprocesses the records pending identity), the roster that carries the id is not imported yet (they come back on their own when it arrives), or only a phone came.
3. The user knows who the person is: link_intel_pending with the raw_ids and the person id.
4. A conflict (ids of two persons with different CPFs): resolve_intel_identity_conflict, merge or choose, only with what the user or the data says.
5. A wrong merge: list_intel_merges, then undo_intel_merge.
6. Follow the run of every write that queues one.

Report how many records were linked, resolved or still pending.
