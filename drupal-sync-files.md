**How to add custom sync of files between two drupal instances**

Install linux utitility tools named inotify, which lateer will be used for triggering file sync.

```sudo yum install inotify-tools```

Install rsync for running the sync(Most of the linux os it will be installed by default).

```sudo yum install rsync```

Create the below script. change the source, desitination path and hostname accordingly.

```touch sync_script.sh```

```chmod +x sync_script.sh```

```nohup sync_script.sh &```

```
#!/bin/bash

SOURCE_DIR="/path/to/source"
DEST_DIR="user@<hostname>:/path/to/desitnation"
RSYNC_OPTIONS="-avz --delete" # Customize as needed (e.g., -avz, --delete)

while true; do
	inotifywait -r -e modify,create,delete,move "$SOURCE_DIR"

	echo "File change detected in $SOURCE_DIR. Initiating rsync..."
	rsync $RSYNC_OPTIONS -e "ssh -i ~/.ssh/id_rsa" "$SOURCE_DIR/" "$DEST_DIR/"
	echo "Rsync complete."
done
```

Note: configure above steps to both the instances where sync needs to be happened. alos make sure both instances can be connected with ssh without the key suing pulic key.
