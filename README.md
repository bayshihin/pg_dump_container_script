With this script you can automate the backup of your Postgres database, especially with the additional use of crontab

This part specifies your credentials for authorization with the desired database in Postgres.
```
PGHOST=""
PGUSER=""
PGPASSWORD=""
DATABASE=""
```
In this part of the code, the naming of the backup is set according to the date
```
BACKUP_FILE="db_backup_$(date +'%Y%m%d').sql"
```
Here you can specify the directory where the backup will be saved
```
cd /your/path/to/backup/dir/
```
This command first penetrates inside the container, then executes the pg_dump command there to create a backup. After this, the .sql file will be saved to the specified path for backups
```
docker exec <POSTGRESQL_CONTAINER_NAME> pg_dump -h $PGHOST -U $PGUSER -d $DATABASE > $BACKUP_FILE   # replace <POSTGRESQL_CONTAINER_NAME> with your posrgresql container name
```
In this part, the .sql file is packed into an archive
```
tar -czf $BACKUP_FILE.tar.gz $BACKUP_FILE
```
In this part the .sql file is deleted
```
rm $BACKUP_FILE
```
At this stage, you can configure pushing archives with backups to remote storage
```
cd /your/path/to/localrepo/with/backups
git add .
git commit -m "Added database backup"
git push origin main
```
