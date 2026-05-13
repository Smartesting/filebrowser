# Deployment & Smoke Test

## Build from Source (required for mutation scenarios)

```bash
# 1. Build frontend
cd frontend
pnpm install
pnpm run build
cd ..

# 2. Build Go backend
go mod download
go build -o filebrowser .

# 3. Generate bcrypt password hash
python3 -c "import bcrypt; print(bcrypt.hashpw(b'adminadminadmin', bcrypt.gensalt()).decode())"

# 4. Run
./filebrowser --password '<bcrypt-hash>'
```

## Credentials

- **URL**: http://localhost:8080
- **Username**: admin
- **Password**: adminadminadmin

## Reset

```bash
pkill filebrowser
rm -f filebrowser.db
```

Then re-run step 4 above.

## Smoke Test

1. Open http://localhost:8080
2. Log in as admin/adminadminadmin
3. Verify the file listing page loads
4. Create a folder via the UI or API
5. Upload a file to the folder
6. Verify the file appears in the listing
7. Delete the file and folder
8. Log out

## Build Notes

- **Must build from source** to create mutation scenarios. The official Docker image (`filebrowser/filebrowser`) is a pre-built binary that cannot be modified.
- Frontend uses `pnpm` + `vite`; backend is Go 1.22+.
- Uses BoltDB (`filebrowser.db`) for user/settings storage.
- Quick setup auto-creates the admin user only when the database is missing; using `--password` skips random password generation.
- Binary defaults to `127.0.0.1:8080`.
