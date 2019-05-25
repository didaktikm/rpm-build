# rpm-build
Сборка пакета из source rpm на примере CentOS 7 и пакета midnight commander

#### Скачиваем группу пакетов для сборки RPM

`yum install -y redhat-lsb-core wget rpmdevtools rpm-build createrepo yum-utils gcc`

#### Загружаем SRPM пакет

`yumdownloader --source mc`

#### Устанавливаем пакет. При установке в домашней директории создается дерево каталогов для сборки

`rpm -i mc-4.8.7-11.el7.src.rpm`

#### Заранее поставим все зависимости чтобý в процессе сборки не было ошибок

`yum-builddep rpmbuild/SPECS/mc.spec`

#### Редактируем spec файл 

`vim rpmbuild/SPECS/mc.spec`

#### Для примера добавим опцию которая выключает поддержку виртуальной файловой системы ftp. Добавляем в секцию `%configure` значение `--enable-vfs-ftp`

`%configure      --with-screen=slang \

                --enable-charset \
                
                --without-x \
                
                --with-gpm-mouse \
                
                --disable-rpath \
                
                --enable-vfs-smb \
                
                --enable-vfs-sftp \
                
                --enable-aspell \
                
                --enable-vfs-mcfs \
                
                --enable-vfs-ftp`
                
#### Собираем RPM пакет

`rpmbuild -bb rpmbuild/SPECS/mc.spec`

выхлоп в конце

`Executing(%clean): /bin/sh -e /var/tmp/rpm-tmp.FP0A0z
+ umask 022
+ cd /root/rpmbuild/BUILD
+ cd mc-4.8.7
+ rm -rf /root/rpmbuild/BUILDROOT/mc-4.8.7-11.el7.x86_64
+ exit 0`

#### Устанавливаем собраный пакет

`yum localinstall -y rpmbuild/RPMS/x86_64/mc-4.8.7-11.el7.x86_64.rpm`

#### Проверяем что установился

`mc -V`

`GNU Midnight Commander 4.8.7
Built with GLib 2.56.1
Using the S-Lang library with terminfo database
With builtin Editor
With subshell support as default
With support for background operations
With mouse support on xterm and Linux console
With internationalization support
With multiple codepages support
Virtual File Systems: cpiofs, tarfs, sfs, extfs, ftpfs, sftpfs, fish, smbfs
Data types: char: 8; int: 32; long: 64; void *: 64; size_t: 64; off_t: 64;`

ENJOY!
