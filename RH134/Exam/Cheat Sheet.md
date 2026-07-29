# RHCSA RH134 Cheat Sheet - RHEL10

## SELinux quick fix flow
```
ls -Z /path
semanage fcontext -a -t correct_t "/path(/.*)?"
restorecon -Rv /path
getsebool -a | grep service
setsebool -P bool on
sealert -a /var/log/audit/audit.log
```

## LVM extend
```
lvextend -r -L +500M /dev/vg/lv  # -r auto grow fs
xfs_growfs /mount   # for xfs
resize2fs /dev/vg/lv  # for ext4
```

## Boot & root recovery
```
GRUB e -> linux line -> rd.break -> ctrl+x
mount -o remount,rw /sysroot
chroot /sysroot
passwd
touch /.autorelabel
exit
exit
```

## Firewall + SELinux port
```
firewall-cmd --permanent --add-port=82/tcp
firewall-cmd --reload
semanage port -a -t http_port_t -p tcp 82
```

## Systemd Timer
```
[Service] + [Timer] file in /etc/systemd/system/
OnCalendar=*:0/5
systemctl daemon-reload
systemctl enable --now mybackup.timer
```

## Logs
```
journalctl -b -p err
journalctl -u sshd --since today
mkdir -p /var/log/journal
/etc/systemd/journald.conf Storage=persistent
chronyc sources
```

## NFS Autofs
```
/etc/exports: /share *(rw)
exportfs -rv
client fstab: server:/share /mnt nfs _netdev 0 0

/etc/auto.master: /misc /etc/auto.misc
/etc/auto.misc: data -rw server:/share
systemctl enable --now autofs
```

## Podman systemd
```
podman run -d --name web -p 8080:80 -v /src:/usr/share/nginx/html:Z nginx
podman generate systemd --new --name web > ~/.config/systemd/user/container-web.service
loginctl enable-linger
systemctl --user enable --now container-web
```

## Tar/Rsync
```
tar czf backup.tar.gz /etc --exclude=/etc/hosts
tar xzf backup.tar.gz -C /tmp
rsync -av --delete -e ssh /data/ user@host:/data/
```
