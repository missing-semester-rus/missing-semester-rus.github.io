---
layout: lecture
title: "Potpourri"
date: 2020-01-29
ready: true
video:
  aspect: 56.25
  id: JZDt-PRq0uo
---

## Содержание

- [Настройка клавиатуры](#настройка-клавиатуры)
- [Daemons](#daemons)
- [FUSE](#fuse)
- [Backups](#backups)
- [API](#api)
- [Common command-line flags/patterns](#common-command-line-flagspatterns)
- [Оконные менеджеры](#оконные-менеджеры)
- [VPN](#vpn)
- [Markdown](#markdown)
- [Hammerspoon (desktop automation on macOS)](#hammerspoon-desktop-automation-on-macos)
- [Booting + Live USBs](#booting--live-usbs)
- [Docker, Vagrant, VMs, Cloud, OpenStack](#docker-vagrant-vms-cloud-openstack)
- [Notebook programming](#notebook-programming)
- [GitHub](#github)

## Настройка клавиатуры

Для Вас, как для программиста, главным способом ввода является клавиатура. Как и практически все на Вашем компьютере, она настраивается (и стоит того, чтобы ее настроить). 

Базовым изменением является переопределение клавиш. 
Обычно для этого потребуется некоторая программа, которая воспринимает нажатие клавиш, перехватывает некоторое действие и заменяет его на другое в соответствии с другой клавишей. Например:
- Заменить Caps Lock на Ctrl или Escape. Мы (инструкторы) крайне рекомендуем эту настройку, так как Caps Lock находится в очень удобном месте и редко используется.
- Заменить PrtSc на play/pause для музыки. Большинство ОС имеют play/pause клавишу.
- Поменять местами Ctrl и Meta (Windows or Command) клавишу.

Также Вы можете присвоить клавишам произвольные команды на свой вкус. Это полезно в контексте основных задач, с которыми Вы сталкиваетесь. Вот некоторые примеры:
- Открыть новое окно терминала или браузера.
- Вставить какой-нибудь специфический текст, например, длинный email адрес или MIT ID номер.
- Включение спящего режима компьютера или монитора.

Существуют еще более сложные модификации, которые Вы можете настроить:
- Переопределение последовательности клавиш, например, нажатие shift 5 раз включает Caps Lock.
- Настройка клавиши на нажатие и удержание, например, Caps Lock переопределен на Esc при быстром нажатии и на Ctrl при удержании. 
- Сделать настройку специфичной для клавиатуры или программного обеспечения. 

Некоторые ресурсы для ознакомления с данной темой:
- macOS - [karabiner-elements](https://pqrs.org/osx/karabiner/), [skhd](https://github.com/koekeishiya/skhd) или [BetterTouchTool](https://folivora.ai/)
- Linux - [xmodmap](https://wiki.archlinux.org/index.php/Xmodmap) или [Autokey](https://github.com/autokey/autokey)
- Windows - [AutoHotkey](https://www.autohotkey.com/) или [SharpKeys](https://www.randyrants.com/category/sharpkeys/)
- QMK - Если Ваша клавиатура поддерживает кастомную прошивку, можно использовать [QMK](https://docs.qmk.fm/) для настройки самого устройства, так что Ваши переопредления будут работать для любого ПК, с которым Вы используете данную клавиатуру. 

## Daemons

You are probably already familiar with the notion of daemons, even if the word seems new.
Most computers have a series of processes that are always running in the background rather than waiting for a user to launch them and interact with them.
These processes are called daemons and the programs that run as daemons often end with a `d` to indicate so.
For example `sshd`, the SSH daemon, is the program responsible for listening to incoming SSH requests and checking that the remote user has the necessary credentials to log in.

In Linux, `systemd` (the system daemon) is the most common solution for running and setting up daemon processes.
You can run `systemctl status` to list the current running daemons. Most of them might sound unfamiliar but are responsible for core parts of the system such as managing the network, solving DNS queries or displaying the graphical interface for the system.
Systemd can be interacted with the `systemctl` command in order to `enable`, `disable`, `start`, `stop`, `restart` or check the `status` of services (those are the `systemctl` commands).

More interestingly, `systemd` has a fairly accessible interface for configuring and enabling new daemons (or services).
Below is an example of a daemon for running a simple Python app.
We won't go in the details but as you can see most of the fields are pretty self explanatory.

```ini
# /etc/systemd/system/myapp.service
[Unit]
Description=My Custom App
After=network.target

[Service]
User=foo
Group=foo
WorkingDirectory=/home/foo/projects/mydaemon
ExecStart=/usr/bin/local/python3.7 app.py
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

Also, if you just want to run some program with a given frequency there is no need to build a custom daemon, you can use [`cron`](https://www.man7.org/linux/man-pages/man8/cron.8.html), a daemon your system already runs to perform scheduled tasks.

## FUSE

Modern software systems are usually composed of smaller building blocks that are composed together.
Your operating system supports using different filesystem backends because there is a common language of what operations a filesystem supports.
For instance, when you run `touch` to create a file, `touch` performs a system call to the kernel to create the file and the kernel performs the appropriate filesystem call to create the given file.
A caveat is that UNIX filesystems are traditionally implemented as kernel modules and only the kernel is allowed to perform filesystem calls.

[FUSE](https://en.wikipedia.org/wiki/Filesystem_in_Userspace) (Filesystem in User Space) allows filesystems to be implemented by a user program. FUSE lets users run user space code for filesystem calls and then bridges the necessary calls to the kernel interfaces.
In practice, this means that users can implement arbitrary functionality for filesystem calls.

For example, FUSE can be used so whenever you perform an operation in a virtual filesystem, that operation is forwarded through SSH to a remote machine, performed there, and the output is returned back to you.
This way, local programs can see the file as if it was in your computer while in reality it's in a remote server.
This is effectively what `sshfs` does.

Some interesting examples of FUSE filesystems are:
- [sshfs](https://github.com/libfuse/sshfs) - Open locally remote files/folder through an SSH connection.
- [rclone](https://rclone.org/commands/rclone_mount/) - Mount cloud storage services like Dropbox, GDrive, Amazon S3 or Google Cloud Storage and open data locally.
- [gocryptfs](https://nuetzlich.net/gocryptfs/) - Encrypted overlay system. Files are stored encrypted but once the FS is mounted they appear as plaintext in the mountpoint.
- [kbfs](https://keybase.io/docs/kbfs) - Distributed filesystem with end-to-end encryption. You can have private, shared and public folders.
- [borgbackup](https://borgbackup.readthedocs.io/en/stable/usage/mount.html) - Mount your deduplicated, compressed and encrypted backups for ease of browsing.

## Backups

Any data that you haven’t backed up is data that could be gone at any moment, forever.
It's easy to copy data around, it's hard to reliably backup data.
Here are some good backup basics and the pitfalls of some approaches.

First, a copy of the data in the same disk is not a backup, because the disk is the single point of failure for all the data. Similarly, an external drive in your home is also a weak backup solution since it could be lost in a fire/robbery/&c. Instead, having an off-site backup is a recommended practice.

Synchronization solutions are not backups. For instance, Dropbox/GDrive are convenient solutions, but when data is erased or corrupted they propagate the change. For the same reason, disk mirroring solutions like RAID are not backups. They don't help if data gets deleted, corrupted or encrypted by ransomware.

Some core features of good backups solutions are versioning, deduplication and security.
Versioning backups ensure that you can access your history of changes and efficiently recover files.
Efficient backup solutions use data deduplication to only store incremental changes and reduce the storage overhead.
Regarding security, you should ask yourself what someone would need to know/have in order to read your data and, more importantly, to delete all your data and associated backups.
Lastly, blindly trusting backups is a terrible idea and you should verify regularly that you can use them to recover data.

Backups go beyond local files in your computer.
Given the significant growth of web applications, large amounts of your data are only stored in the cloud.
For instance, your webmail, social media photos, music playlists in streaming services or online docs are gone if you lose access to the corresponding accounts.
Having an offline copy of this information is the way to go, and you can find online tools that people have built to fetch the data and save it.

For a more detailed explanation, see 2019's lecture notes on [Backups](/2019/backups).


## API

На этом уроке мы много говорили о том, как использовать компьютер эффективно при
решении _локальных_ задач. Вы также обнаружите, что многие из этих уроков
применимы и при работе в сети Интернет. Большинство онлайн-сервисов
предоставляют "API", которые позволяют получить доступ к данным сервисов
"программно". Например, правительство США предоставляет API, с помощью которого
можно узнать прогноз погоды, таким образом вы можете с легкостью получить данные
в командной оболочке (shell).

Большинство API имеют схожий между собой формат. Это структурированные URL-адреса,
часто расположенные на `api.<сервис>.com`, в которых путь и параметры запроса указывают,
какие вернуть данные или какое выполнить действие. Например, чтобы узнать
прогноз погоды некоторого местоположения в США, нужно отправить GET запрос
(например, с помощью утилиты `curl`) на https://api.weather.gov/points/42.3604,-71.094.
Сам ответ содержит множество других URL-адресов, которые позволяют получить
конкретные прогнозы для указанного региона. Обычно ответы форматируются в JSON,
который затем можно передать утилите типа [`jq`](https://stedolan.github.io/jq/),
чтобы извлечь интересующую информацию.

Некоторые API требуют аутентификации, обычно принимающей форму секретного _токена_
(token), который необходимо включить в запрос. Прочтите документацию API сервиса,
который собираетесь использовать, чтобы узнать используемую технологию аутентификации.
"[OAuth](https://www.oauth.com/)" — протокол, который будет вам часто встречаться.
По своей сути OAuth — это способ предоставить токены, которые могут действовать
"от вашего имени" в рассматриваемом сервисе для выполнения определенных задач.
Имейте в виду, что эти токены являются _секретными_, и каждый, кто получит доступ
к вашему токену, сможет совершать любые действия, использующие токен, под _вашей_
учетной записью!

[IFTTT](https://ifttt.com/) — сайт и сервис, завязанный на идее использования API.
Он позволяет объединять события различных сервисов в произвольную цепочку. Стоит
взглянуть!

## Common command-line flags/patterns

Command-line tools vary a lot, and you will often want to check out
their `man` pages before using them. They often share some common
features though that can be good to be aware of:

 - Most tools support some kind of `--help` flag to display brief usage
   instructions for the tool.
 - Many tools that can cause irrevocable change support the notion of a
   "dry run" in which they only print what they _would have done_, but
   do not actually perform the change. Similarly, they often have an
   "interactive" flag that will prompt you for each destructive action.
 - You can usually use `--version` or `-V` to have the program print its
   own version (handy for reporting bugs!).
 - Almost all tools have a `--verbose` or `-v` flag to produce more
   verbose output. You can usually include the flag multiple times
   (`-vvv`) to get _more_ verbose output, which can be handy for
   debugging. Similarly, many tools have a `--quiet` flag for making it
   only print something on error.
 - In many tools, `-` in place of a file name means "standard input" or
   "standard output", depending on the argument.
 - Possibly destructive tools are generally not recursive by default,
   but support a "recursive" flag (often `-r`) to make them recurse.
 - Sometimes, you want to pass something that _looks_ like a flag as a
   normal argument. For example, imagine you wanted to remove a file
   called `-r`. Or you want to run one program "through" another, like
   `ssh machine foo`, and you want to pass a flag to the "inner" program
   (`foo`). The special argument `--` makes a program _stop_ processing
   flags and options (things starting with `-`) in what follows, letting
   you pass things that look like flags without them being interpreted
   as such: `rm -- -r` or `ssh machine --for-ssh -- foo --for-foo`.

## Оконные менеджеры

Большинство из вас привыкли использовать оконный менеджер, позволяющий
перетаскивать окна программ, который по умолчанию поставляется с Windows, macOS
и Ubuntu. Окна можно накладывать друг на друга, изменять их размер, перемещать в
любую часть экрана. Но эти возможности характеризуют лишь один _тип_ оконного
менеджера, который называется "плавающим" (floating). Существует множество
других типов, в особенности для Linux. Распространенной альтернативой является
"плиточный" (tiling) оконный менеджер. Плиточный тип не позволяет окнам
перекрывать друг друга, таким образом окна располагаются на экране, как панели в
tmux. С плиточным менеджером экран всегда заполнен открытыми окнами, располагая
их в соответствии с заданным _макетом_. Если открыто только одно окно, оно будет
занимать весь экран. Открытие второго окна заставляет первое подвинуться, и
теперь окна занимают по половине (или 1/3 и 2/3) экрана. Открывая третье,
существующие окна снова поджимаются, чтобы вместить нового соседа. Как и в
случае с панелями tmux, по окнам можно перемещаться, изменять их размер
переставлять, используя лишь клавиатуру. Их стоит попробовать!

## VPN

VPN (Virtual Private Network) нынче в моде, но не совсем ясно, существует
ли вообще [причина для этого](https://gist.github.com/joepie91/5a9909939e6ce7d09e29).
Вы должны быть осведомлены о том, что VPN делает и что не делает. VPN в 
лучшем случае _реально_ просто способ поменять провайдера, и весь Ваш трафик
будет выглядеть так, как будто он исходит от VPN провайдера вместо Вашей
"настоящей" локации, и сеть, к которой Вы подключены, будет видеть только
этот "зашифрованный" трафик.

Несмотря на привлекательность данной идеи, держите в уме, что когда Вы
используете VPN, все Ваши действия доверяются не Вашему текущему интернет-провайдеру, 
а провайдеру VPN. Все, что может увидеть Ваш интернет-провайдер,
теперь _вместо_ него видит провайдер VPN. И только если Вы доверяете ему 
_больше_, чем интернет-провайдеру, Вы в выигрыше, а в противном случае
результат для Вас сомнителен. Если Вы подключены к какому-нибудь сомнительному
публичному Wi-Fi в аэропорту, тогда скорее всего Вы не особо доверяете соединению,
но будучи дома стоит подумать дважды. 

Также Вы должны знать, что в наши дни большая часть трафика, по крайней
мере деликатного характера, _уже_ зашифрована с помощью HTTPS или TLS. 
И поэтому от того, на "плохой" Вы сети или нет, мало что зависит - оператор
сети будет знать только о том, с какими серверами Вы взаимодействуете, но
не о самих данных, которыми Вы обмениваетесь. 

Обратите внимание, я сказал "в лучшем случае" выше. VPN провайдеры совсем
не гнушаются настраивать свой софт таким образом, чтобы шифрование было 
слабее или даже совсем отсутствовало. Некоторые VPN провайдеры попросту 
злоумышленники (или по крайней мере, оппортунисты), и будут записывать весь
Ваш трафик и вероятно продавать информацию третьим лицам. Выбрать плохого
VPN провайдера обычно намного хуже, чем вообще не использовать VPN.

В крайнем случае, у MIT есть [свой VPN](https://ist.mit.edu/vpn) для студентов,
так что возможно Вам стоит взглянуть. Также если Вы собрались запустить свой,
обратите внимание на [WireGuard](https://www.wireguard.com/).

## Markdown

Скорее всего на карьерном пути Вы столкнетесь с написанием текстов. 
И как правило хочется иметь возможность разметить текст простым
способом. Например, сделать текст жирным или курсивом, добавить
заголовки, ссылки, куски кода. Вместо использования таких тяжелых
инструментов, как Word или LaTeX, предлагаем Вам попробовать облегченный
язык разметки [Markdown](https://commonmark.org/help/).
    
Вероятно Вы уже сталкивались с Markdown или по крайней мере с каким-то
его вариантом. Так или иначе он частично используется и поддерживается
практически везде, даже если не конкретно под своим именем. По своей сути,
Markdown - это попытка конвертировать в код способ, которым люди обычно
размечают текст, когда пишут простые текстовые документы. Акцент (*курсив*)
добавляется при помощи `*` перед словом и после него. Сильный акцент 
(**жирный**) - аналогично с помощью `**`. Строки, начинающиеся с `#`, 
являются заголовками (причем количество `#`отражает уровень подзаголовка).
Любая строка, начинающаяся с `-`, это пункт маркированного списка, а с 
номером и `.` - нумерованного. Бэктик (обратный апостроф) применяется
для выделения строки кода, а если Вам нужно вставить блок кода, используйте
тройные бэктики:

    ```
    код начинается тут 
    ```

Для добавления ссылки поместите _текст_  для ссылки в квадратные скобки,
а сам URL сразу после в круглых скобках: `[name](url)`. Markdown довольно
просто начать пользоваться, и Вы можете использовать его практически везде.
На самом деле, заметки к этой и другим лекциям написаны на Markdown, и Вы 
можете посмотреть на "сырой" Markdown [здесь](https://raw.githubusercontent.com/missing-semester/missing-semester/master/_2020/potpourri.md).


## Hammerspoon (desktop automation on macOS)

[Hammerspoon](https://www.hammerspoon.org/) is a desktop automation framework
for macOS. It lets you write Lua scripts that hook into operating system
functionality, allowing you to interact with the keyboard/mouse, windows,
displays, filesystem, and much more.

Some examples of things you can do with Hammerspoon:

- Bind hotkeys to move windows to specific locations
- Create a menu bar button that automatically lays out windows in a specific layout
- Mute your speaker when you arrive in lab (by detecting the WiFi network)
- Show you a warning if you've accidentally taken your friend's power supply

At a high level, Hammerspoon lets you run arbitrary Lua code, bound to menu
buttons, key presses, or events, and Hammerspoon provides an extensive library
for interacting with the system, so there's basically no limit to what you can
do with it. Many people have made their Hammerspoon configurations public, so
you can generally find what you need by searching the internet, but you can
always write your own code from scratch.

### Resources

- [Getting Started with Hammerspoon](https://www.hammerspoon.org/go/)
- [Sample configurations](https://github.com/Hammerspoon/hammerspoon/wiki/Sample-Configurations)
- [Anish's Hammerspoon config](https://github.com/anishathalye/dotfiles-local/tree/mac/hammerspoon)

## Booting + Live USBs

When your machine boots up, before the operating system is loaded, the
[BIOS](https://en.wikipedia.org/wiki/BIOS)/[UEFI](https://en.wikipedia.org/wiki/Unified_Extensible_Firmware_Interface)
initializes the system. During this process, you can press a specific key
combination to configure this layer of software. For example, your computer may
say something like "Press F9 to configure BIOS. Press F12 to enter boot menu."
during the boot process. You can configure all sorts of hardware-related
settings in the BIOS menu. You can also enter the boot menu to boot from an
alternate device instead of your hard drive.

[Live USBs](https://en.wikipedia.org/wiki/Live_USB) are USB flash drives
containing an operating system. You can create one of these by downloading an
operating system (e.g. a Linux distribution) and burning it to the flash drive.
This process is a little bit more complicated than simply copying a `.iso` file
to the disk. There are tools like [UNetbootin](https://unetbootin.github.io/)
to help you create live USBs.

Live USBs are useful for all sorts of purposes. Among other things, if you
break your existing operating system installation so that it no longer boots,
you can use a live USB to recover data or fix the operating system.

## Docker, Vagrant, VMs, Cloud, OpenStack

[Virtual machines](https://en.wikipedia.org/wiki/Virtual_machine) and similar
tools like containers let you emulate a whole computer system, including the
operating system. This can be useful for creating an isolated environment for
testing, development, or exploration (e.g. running potentially malicious code).

[Vagrant](https://www.vagrantup.com/) is a tool that lets you describe machine
configurations (operating system, services, packages, etc.) in code, and then
instantiate VMs with a simple `vagrant up`. [Docker](https://www.docker.com/)
is conceptually similar but it uses containers instead.

You can also rent virtual machines on the cloud, and it's a nice way to get instant
access to:

- A cheap always-on machine that has a public IP address, used to host services
- A machine with a lot of CPU, disk, RAM, and/or GPU
- Many more machines than you physically have access to (billing is often by
the second, so if you want a lot of computing for a short amount of time, it's
feasible to rent 1000 computers for a couple of minutes)

Popular services include [Amazon AWS](https://aws.amazon.com/), [Google
Cloud](https://cloud.google.com/),[ Microsoft Azure](https://azure.microsoft.com/),
[DigitalOcean](https://www.digitalocean.com/).

If you're a member of MIT CSAIL, you can get free VMs for research purposes
through the [CSAIL OpenStack
instance](https://tig.csail.mit.edu/shared-computing/open-stack/).

## Notebook programming

[Notebook programming
environments](https://en.wikipedia.org/wiki/Notebook_interface) can be really
handy for doing certain types of interactive or exploratory development.
Perhaps the most popular notebook programming environment today is
[Jupyter](https://jupyter.org/), for Python (and several other languages).
[Wolfram Mathematica](https://www.wolfram.com/mathematica/) is another notebook
programming environment that's great for doing math-oriented programming.

## GitHub

[GitHub](https://github.com/) - одна из самых популярных платформ для разработки
открытого программного обеспечения (open-source software). Множество инструментов,
о которых мы говорили, от [vim](https://github.com/vim/vim) до [Hammerspoon](https://github.com/Hammerspoon/hammerspoon),
размещены на GitHub. Довольно легко начать вносить свой вклад в open-source, чтобы
сделать лучше инструменты, которыми мы пользуемся ежедневно. 

Есть два главных способа участия в проектах на GitHub:

- Создать [issue](https://help.github.com/en/github/managing-your-work-on-github/creating-an-issue).
Таким образом можно сообщить о баге или запросить новую функцию. Ни то, ни другое
не требует чтения или написания кода, так что сделать это довольно легко. Качественные
отчеты о багах могут оказаться очень ценными для разработчиков. Участие в уже существующих
обсуждениях тоже может быть полезным.
- Внести свой код с помощью [pull
request](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/about-pull-requests).
Это естественно требует большего вовлечения в проект, нежели issue. Вы можете скопировать 
[fork](https://help.github.com/en/github/getting-started-with-github/fork-a-repo) 
репозиторий на GitHub, клонировать уже Вашу копию локально, создать новую ветку, 
внеси изменения (например,исправить баг или добавить фунционал), запушить ветку и 
создать [pull request](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/creating-a-pull-request).
Далее последует некоторое обсуждение с разработчиками проекта, и Вы получите
фидбек на Ваши изменения. В итоге, если все сложится благополучно, Ваши изменения
будут смерджены в оргинальный (upstream) репозиторий. Как правило, у больших проектов
существует гайд для участия, проблемы (issues) с пометкой beginner-friendly (для новичков),
а некоторые даже имеют менторские программы, помогающие новичкам ознакомиться с проектом.
