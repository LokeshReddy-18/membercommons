# 📖 Database Admin Panel

A web-based interface to test PostgreSQL database connections and explore the SuiteCRM schema.

## Files

- `index.html` - Main admin interface
- `db-admin.js` - Frontend JavaScript for database operations
- `rust_db_endpoints.rs` - Backend Rust endpoints (✅ **Already integrated in main.rs**)
- `README.md` - This documentation

## Setup

### 1. Database Configuration ✅ **Already Configured**

The database configuration is already set up in the Rust backend (`src/main.rs`) with support for:
- Environment variables (`.env` file)
- Configuration file (`config.toml`)
- Default PostgreSQL connection settings

The backend automatically loads database credentials from:
- `DATABASE_URL` environment variable
- Or falls back to: `postgres://user:password@localhost/suitecrm`

### 2. Backend Server ✅ **Already Implemented**

All database admin endpoints have been integrated into the main Rust backend:

- ✅ Database admin endpoints are implemented in `src/main.rs`
- ✅ All required dependencies are present in `Cargo.toml`
- ✅ Routes are configured and ready to use
- ✅ Security restrictions (SELECT-only queries) are enforced

[The Cargo.toml](https://github.com/localsite/membercommons/blob/claude/cargo.toml) file for each package is called its manifest. It is written in the TOML format. Learn more about the [The Cargo Manifest Format](https://doc.rust-lang.org/cargo/reference/manifest.html).

**Start the server:**
```bash
cargo run -- serve
```

The server will start on `http://127.0.0.1:8081` by default.

### 3. Access Admin Panel

1. Open `sql/admin/index.html` in your web browser
2. Or serve it via HTTP server:
   ```bash
   # From the project root
   python -m http.server 3000
   # Then visit: http://localhost:3000/sql/admin/
   ```

## Features

### Connection Testing
- Tests database connectivity
- Shows connection parameters (without exposing password)
- Displays server information and diagnostics
- Provides troubleshooting help

### Table Listing
- Lists first 10 database tables
- Shows estimated row counts
- Provides table descriptions
- Displays schema information

### Quick Actions
- Check specific tables (users, accounts)
- Execute simple SELECT queries
- Test database health status
- View connection diagnostics

### Logging
- Real-time operation logging
- Request/response tracking
- Error details and debugging info
- Clear log functionality

## API Endpoints ✅ **All Implemented**

The following backend endpoints are available and working:

| Endpoint | Method | Description | Status |
|----------|--------|-------------|---------|
| `/api/db/test-connection` | GET | Test database connection | ✅ Active |
| `/api/db/tables` | GET | List database tables | ✅ Active |
| `/api/db/table/{name}` | GET | Get table information | ✅ Active |
| `/api/db/query` | POST | Execute SELECT query | ✅ Active |
| `/api/health` | GET | Health check | ✅ Active |
| `/health` | GET | Health check (root level) | ✅ Active |

## Response Format

All endpoints return JSON in this format:

```javascript
{
    "success": true,
    "message": "Operation completed",
    "error": null,
    "data": { /* actual data */ }
}
```

## Security Notes

⚠️ **Important Security Considerations:**

1. **Admin Access Only** - This panel should only be accessible to administrators
2. **Query Restrictions** - Only SELECT queries are allowed for safety
3. **Firewall Rules** - Ensure your Azure PostgreSQL allows connections from your IP
4. **SSL Encryption** - All connections use SSL/TLS encryption
5. **No Password Display** - Passwords are never shown in the interface

## Troubleshooting

### Common Issues

**Connection Refused**
- Check if Rust backend is running on port 8081
- Verify `cargo run -- serve` is active

**Database Connection Failed**
- Verify Azure PostgreSQL credentials in .env
- Check Azure firewall rules allow your IP address

**Tables Not Loading**
- Ensure database schema is properly initialized
- Check user permissions on PostgreSQL database
- Verify table names match expected SuiteCRM schema

**CORS Errors**
- Add CORS headers to your Rust backend
- Ensure frontend and backend domains match

### Debug Steps

1. **Check Browser Console** - Look for JavaScript errors
2. **Check Network Tab** - Verify API requests are being made
3. **Check Backend Logs** - Look for Rust server error messages
4. **Test Direct Connection** - Use `psql` to test database connectivity

### Example Manual Test

Test your Azure PostgreSQL connection manually:

```bash
psql "host=model-earth-server.database.windows.net port=5432 dbname=ModelEarthDB user=sqladmin password=your-password sslmode=require"
```

## Development

### Adding New Features

1. Add frontend functionality to `db-admin.js`
2. Add corresponding backend endpoint to `rust_db_endpoints.rs`
3. Update the HTML interface in `index.html`
4. Test thoroughly before deploying

### Testing Checklist

- [ ] Connection test works
- [ ] Tables list properly
- [ ] Individual table queries work
- [ ] Error handling displays correctly
- [ ] Logging captures all operations
- [ ] Security restrictions are enforced

## Production Deployment

For production use:

1. **Restrict Access** - Add authentication/authorization
2. **HTTPS Only** - Serve over secure connections
3. **Rate Limiting** - Prevent abuse of database queries
4. **Audit Logging** - Log all admin operations
5. **Backup Strategy** - Ensure database backups before any operations

Remember: This is a powerful tool that provides direct database access. Use responsibly!