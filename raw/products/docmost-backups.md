# Docmost Backup and Restore

> Source: https://docmost.com/docs/self-hosting/backups
> Collected: 2026-11-02
> Published: Unknown

Docmost stores workspace data in PostgreSQL and uploaded files on disk or in object storage.

Data | What to save
PostgreSQL | A database dump containing pages, users, settings, and other workspace data.
Uploaded files | The local storage directory, or your S3 bucket or Azure Blob container.
Configuration | docker-compose.yml, any .env files, and other deployment configuration.

Uploaded files are copied separately. Files changed or deleted during the backup may not match the database snapshot. To avoid this, stop all Docmost instances until both backups finish. Keep PostgreSQL running.
