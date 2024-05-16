
## Upload you Files on Google Drive with `rclone`

- Visite the [Site](https://rclone.org/drive/#shared-drives-team-drives) for Installation guide and how to set up your google-drive with rclone.
- Start copy data to the Google-Drive with the command : `rclone copy >Folder_Name< GoogleDrive:OffSec -v --drive-copy-shortcut-content `
` :

1. `OffSec` : Is the name of the folder you created on Google-Drive.
2. `GoogleDrive` : The name of the Drive you ceated with rclone
3. `--drive-copy-shortcut-content` : option to copy the files and folders as they are (avoid links)
