## Домашнее задание: ZFS.
### Цель:
#### Научится самостоятельно устанавливать ZFS, настраивать пулы, изучить основные возможности ZFS

### Для выполнения работы используются следующие инструменты:
- VirtualBox - среда виртуализации, позволяет создавать и выполнять виртуальные машины;
- Vagrant - ПО для создания и конфигурирования виртуальной среды. В данном случае в качестве среды виртуализации используется VirtualBox;
- Git - система контроля версий

### Аккаунты:
- GitHub - https://github.com/

### Создание образа 
####  [Vagrantfile](https://github.com/serjb1973/zfs/blob/main/Vagrantfile)
```
vagrant up
```

### 1. Определить алгоритм с наилучшим сжатием.
##  файл лог [zfs1.log](https://github.com/serjb1973/zfs/blob/main/zfs1.log)

```
lsblk
zpool create otus1 mirror /dev/sdb /dev/sdc
zpool create otus2 mirror /dev/sdd /dev/sde
zpool create otus3 mirror /dev/sdf /dev/sdg
zpool create otus4 mirror /dev/sdh /dev/sdi
list
zfs set compression=lzjb otus1
zfs set compression=lz4 otus2
zfs set compression=gzip-9 otus3
zfs set compression=zle otus4
zfs get all | grep compression
for i in {1..4}; do wget -P /otus$i https://gutenberg.org/cache/epub/2600/pg2600.converter.log; done
ls -l /otus*
zfs list
zfs get all | grep compressratio | grep -v ref
```
#### Алгоритм gzip-9 самый эффективный по сжатию. 

### 2. Определение настроек пула
##  файл лог [zfs2.log](https://github.com/serjb1973/zfs/blob/main/zfs2.log)
```
wget -O archive.tar.gz --no-check-certificate 'https://drive.google.com/u/0/uc?id=1KRBNW33QWqbvbVHa3hLJivOAt60yukkg&export=download' 
tar -xzvf archive.tar.gz
zpool import -d zpoolexport/
zpool import -d zpoolexport/ otus
zpool status
zpool get all otus
zfs get all otus
zfs get available otus
zfs get readonly otus
zfs get recordsize otus
zfs get compression otus
zfs get checksum otus
zfs get filesystem_limit otus
```

### 3. Работа со снапшотом, поиск сообщения от преподавателя
##  файл лог  [zfs3.log](https://github.com/serjb1973/zfs/blob/main/zfs3.log)
```
wget -O otus_task2.file --no-check-certificate "https://drive.google.com/u/0/uc?id=1gH8gCL9y7Nd5Ti3IRmplZPF1XjzxeRAG&export=download"
zfs list
zfs receive otus/test@today < otus_task2.file
zfs list
find /otus/test -name "secret_message"
cat /otus/test/task1/file_mess/secret_message
```
