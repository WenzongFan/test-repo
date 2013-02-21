# NFS - Network File System

## transport protocol

NFS -> RPC -> TCP/UDP

RPC: Remote Procedure Call

## NFS Server

### Daemon

* mountd, nfsd - share same ACL DB

		mountd: response mount request
		nfsd: provide file service

### start nfs server:
* Redhat Enterprise:	/etc/rc.d/init.d/nfs
* Fedora		/etc/rc.d/init.d/nfs
* SUSE			/etc/init.d/nfsboot
* Debian		/etc/init.d/nfs-kernel-server, /etc/init.d/nfs-common
* Ubuntu		/etc/init.d/nfs-kernel-server, /etc/init.d/nfs-common

### export list

* /etc/exports

		# export all list in /etc/exports
		$ exportfs -a

### exports file

List file system exported, hosts that will access those fs.

	fs_to_export	host1(options) host2 ...

Example:

	/home/boggs	host1(rw,no_root_squash)	host2(rw)
	/usr/share/man	*(ro)

* specify client host

		hostname, @groupname(NIS group), */?, ipaddr/mask(128.224.162.128/25)

* export options

		ro:	readonly
		rw: 	read and write (default)
		rw=list:	list with rw, other ro
		root_squash:	mapping UID0/GID0 to anonuid/anongid
		no_root_squash: run with root directly, dangerous!!!
		all_squash: 	mapping all uid/gid to anonuid/anongid
		noaccess:	don't access this fs and sub-fs
		wdelay:		delay to write for merge multiple updates
		no_wdelay:	write to disk ASAP
		sync:		response write request before write to disk
		...

Note: run `exportfs -a` after update exports file

### nfsd - provide file service

Adjust nfsd threads number to improve performance

* update startup script
* specify in cmdline

## NFS Client

	# show exported list from server
	# if showmount retuen err message, check portmap, mountd, nfsd
	# statd, lockd on server and hosts.allow/hosts.deny
	$ showmount -e servername

	# mount nfs fs
	$ mount -o rw,hard,intr,bg servername:/home/boggs /mnt/boggs

	# umount nfs
	$ umount 
	$ umount -f

	# check the processes that opened files if nfs fs is busy
	$ lsof

### mount remote fs on startup

/etc/fstab:

	# filesystem	mountpoint	fstype	flags	dump	fsck
	servername:/home /home	nfs	rw,bg,intr,hard,nodev,nosuid	0 0

Mount all nfs fs from fstab:

	$ mount -a -t nfs

## nfsstat

	# show stats info to nfsd process
	$ nfsstat -s

	# show opts to client
	$ nfsstat -c

## NFS File Server

Some special NFS servers/devices:
* EMC NFS servers
* SAN (storage area network)
* NAS (Network attached storage)

## Auto Mount

* /etc/auto.master - master config file

		# dir	map	opts
		/chim	/etc/auto.chim	-secure,hard,bg,intr

* automount - a background process to mount fs while get requst

* autofs - Linux autofs driver reside in kernel

* /etc/init.d/autofs - script to run automount

