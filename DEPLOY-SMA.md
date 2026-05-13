# Deployment & Smoke Test

## Deploy

```bash
docker run -d --name filebrowser -p 8080:80 filebrowser/filebrowser --password '<bcrypt-hash>'
```

Generate the bcrypt hash:

```bash
python3 -c "import bcrypt; print(bcrypt.hashpw(b'adminadminadmin', bcrypt.gensalt()).decode())"
```

Pass the resulting hash to `--password`.

## Credentials

- **URL**: http://localhost:8080
- **Username**: admin
- **Password**: adminadminadmin

## Reset

```bash
docker stop filebrowser && docker rm filebrowser
```

Then re-run the deploy command above.

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

- Runs as UID 1000 inside the container; avoid host volume mounts unless ownership matches.
- Uses BoltDB (`filebrowser.db`) for user/settings storage.
- Quick setup auto-creates the admin user only when the database is missing; using `--password` skips random password generation.
