# Changelog

## 2026-05-04

### Added

- Added the CFIP sync blacklist field `sync_blacklisted`.
- Added single CFIP blacklist/unblacklist API: `POST /api/cfip/{id}/blacklist`.
- Added batch CFIP blacklist/unblacklist API: `POST /api/cfip/batch/blacklist`.
- Added `X-Internal-Key` support for the internal CFIP sync endpoint: `GET /api/internal/cfip`.
- Added CFIP blacklist controls in the management UI for both list and card views.
- Added CFIP blacklist state display and sorting in the management UI.
- Added import compatibility for `sync_blacklisted` and legacy alias `blacklisted`.

### Changed

- Internal CFIP sync now returns only CFIPs that are not blacklisted.
- VLESS, SS, ARGO, token-based subscriptions, and subscription generation now exclude blacklisted CFIPs.
- Explicit subscription parameter `?cfip=1,2,3` still ignores CFIP status, but now always excludes blacklisted CFIPs.
- Telegram CFIP import now writes `status = 'enabled'` and `sync_blacklisted = 0`.
- Boolean API fields now accept case-insensitive string `true` and numeric/string `1`.

### Compatibility

- Existing CFIP rows remain compatible: `sync_blacklisted IS NULL` is treated as not blacklisted.
- New D1 tables create `sync_blacklisted INTEGER DEFAULT 0`; existing tables are migrated with `ALTER TABLE`.
- `blacklisted` remains a request-body alias for `sync_blacklisted`.

### Notes

- Blacklist update responses include `changes`; `changes = 0` means no matching CFIP row was updated.
