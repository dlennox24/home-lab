# home-assistant

# Restoring a backup

1. Create a fresh install of ubuntu server
1. Move the "Full System Backup" from Duplicati to the `/opt` folder and name it `backup.zip`

   ```shell
   sudo scp nas@192.168.1.X:/path/to/backup.zip /opt/backup.zip
   ```

1. Run the below block to begin the restore

   ```shell
   cd /opt
   sudo wget --no-cache --no-cookies https://github.com/dlennox24/home-lab/archive/refs/heads/main.zip -O main.zip
   sudo wget --no-cache --no-cookies https://raw.githubusercontent.com/dlennox24/home-lab/main/home-assistant/restore.sh -O restore.sh
   sudo chmod +x restore.sh
   sudo ./restore.sh
   ```

1. Visit the Duplicati interface (`http://<ip>:8200`) to restore the backup

   1. Use the `Whole+System+Backup+(GDrive)-duplicati-config.json` file as an import
   1. Add encryption (`AES-256 encryption, built in`) and password to the backup
   1. Verify connection to GDrive folder `backups/ha/duplicati/whole-system-gdrive` via AuthID
   1. Save the backup
   1. Select the dropdown next to the name
   1. Select `Database` and then select `Repair`
   1. Once complete select the dropdown again, then `Restore files`
   1. Select all the sources and verify the `Restore from` is set to the latest backup, then select `Continue`
   1. Ensure `Original location`, `Overwrite`, and `Restore read/write permissions` is selected and click `Restore`

1. Run `sudo docker compose restart` so the containers can pick up the recovered files
