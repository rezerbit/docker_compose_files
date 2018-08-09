# Backup and restore solution for Docker volume backups

Used https://github.com/blacklabelops/volumerize

### First backup:

```
docker exec -ti <volumerize_container> backup
```

### Restore:

```
docker exec -ti <volumerize_container> restore
```

You can restore from a particular backup by adding a time parameter to the command restore. For example, using `restore -t 3D` at the end in the above command will restore a backup from 3 days ago.
