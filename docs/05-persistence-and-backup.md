# Phase 5: Persistence & Backups

### Volume Strategy
To prevent data loss during container teardowns, the database state was mapped to a named Docker volume: `db-data:/var/lib/mysql`. 

### Disaster Recovery Drill (Tested & Proven)
We successfully executed a manual disaster simulation to validate our backup routine.
1.  **Snapshot:** Executed `mysqldump` to create `backup.sql` (34KB) on the host VM.
2.  **Disaster Event:** Dropped the `Book` table data via interactive shell (`DELETE FROM Book;`). Verified the web UI returned "no books available."
3.  **Restoration:** Piped `backup.sql` directly back into the running `epicbook-mysql` container.
4.  **Result:** The UI immediately populated with the restored catalog. Zero data was permanently lost.
