Домашнее Задание

Часть 1

Виртуал бокс и Вагрант.

Пункты с 1-6.

Установлен Virtual Box, Vagrant
Поправлен конфиг и PATH
Vagrant развернул ВМ

c:\Oleg\Vagrant>vagrant up
Bringing machine 'default' up with 'virtualbox' provider...
==> default: Checking if box 'bento/ubuntu-20.04' version '202012.23.0' is up to date...
==> default: Setting the name of the VM: oleg_test
==> default: Clearing any previously set forwarded ports...
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat
==> default: Forwarding ports...
    default: 22 (guest) => 2222 (host) (adapter 1)
==> default: Running 'pre-boot' VM customizations...
==> default: Booting VM...
==> default: Waiting for machine to boot. This may take a few minutes...
    default: SSH address: 127.0.0.1:2222
    default: SSH username: vagrant
    default: SSH auth method: private key
==> default: Machine booted and ready!
==> default: Checking for guest additions in VM...
==> default: Mounting shared folders...
    default: /vagrant => C:/Oleg/Vagrant
==> default: Machine already provisioned. Run `vagrant provision` or use the `--provision`
==> default: flag to force provisioning. Provisioners marked to run always will still run.

c:\Oleg\Vagrant>vagrant ssh

c:\Oleg\Vagrant>vagrant ssh
Welcome to Ubuntu 20.04.1 LTS (GNU/Linux 5.4.0-58-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Wed 19 May 2021 09:18:43 PM UTC

  System load:  0.05              Processes:             115
  Usage of /:   2.2% of 61.31GB   Users logged in:       1
  Memory usage: 7%                IPv4 address for eth0: 10.0.2.15
  Swap usage:   0%


This system is built by the Bento project by Chef Software
More information can be found at https://github.com/chef/bento
Last login: Wed May 19 21:17:12 2021

Для изменения конфигурации ВМ исполььзовал следующий файл.

 Vagrant.configure("2") do |config|
 	config.vm.box = "bento/ubuntu-20.04"
	config.vm.provider "virtualbox" do |vb|
    # имя виртуальной машины
    vb.name = "oleg_test"
    # объем оперативной памяти
    vb.memory = 2048
    # количество ядер процессора
    vb.cpus = 2
  end
 end

Конфигурация ВМ стала в соответствии с указаными параметрами


Часть 2 LINUX

Пункт 8.

какой переменной можно задать длину журнала history, и на какой строчке manual это описывается?

line 862


     HISTSIZE
              The number of commands to remember in the command history (see HISTORY below).  If the value is 0, com‐
              mands  are  not saved in the history list.  Numeric values less than zero result in every command being
              saved on the history list (there is no limit).  The shell sets the default value to 500  after  reading
              any startup files.


что делает директива ignoreboth в bash?

line 833

       HISTCONTROL
              A  colon-separated  list of values controlling how commands are saved on the history list.  If the list
              of values includes ignorespace, lines which begin with a space character are not saved in  the  history
              list.  A value of ignoredups causes lines matching the previous history entry to not be saved.  A value
              of ignoreboth is shorthand for ignorespace and ignoredups.  A value of erasedups  causes  all  previous
              lines  matching  the  current  line to be removed from the history list before that line is saved.  Any
              value not in the above list is ignored.  If HISTCONTROL is unset, or does not include  a  valid  value,
              all  lines  read by the shell parser are saved on the history list, subject to the value of HISTIGNORE.
              The second and subsequent lines of a multi-line compound command are not tested, and are added  to  the
              history regardless of the value of HISTCONTROL.


ignoreboth указывает что в истории не сохранять команды с начинающиеся с пробела или повторяющие предыдущие.

Пункт 9.

9
В каких сценариях использования применимы скобки {} и на какой строчке man bash это описано?

line 257

       { list; }
              list is simply executed in the current shell environment.  list must be terminated with  a  newline  or
              semicolon.  This is known as a group command.  The return status is the exit status of list.  Note that
              unlike the metacharacters ( and ), { and } are reserved words and must occur where a reserved  word  is
              permitted  to be recognized.  Since they do not cause a word break, they must be separated from list by
              whitespace or another shell metacharacter.

Фигурные скопки позволяют уменьшить число записей в команде, сократить

Пункт 10

Основываясь на предыдущем вопросе, как создать однократным вызовом touch 100000 файлов? А получилось ли создать 300000?

touch test{1..100000}.txt

vagrant@vagrant:/var/netology$ dir
vagrant@vagrant:/var/netology$ sudo touch test{1..100000}.txt
-bash: /usr/bin/sudo: Argument list too long
vagrant@vagrant:/var/netology$ sudo touch test{1..10000}.txt
vagrant@vagrant:/var/netology$ dir

Получается создать например 10000 файлов, на 100000 и 300000 сообщает что аргумент слишком большой. 
Надо искать где ограничения


Пункт 11


В man bash поищите по /\[\[. Что делает конструкция [[ -d /tmp ]]


 When  used  with  [[, the < and > operators sort lexicographically using the current locale.  The test command
       sorts using ASCII ordering.

       -a file
              True if file exists.
       -b file
              True if file exists and is a block special file.
       -c file
              True if file exists and is a character special file.
       -d file
              True if file exists and is a directory.

line 269


[[ expression ]]
              Return a status of 0 or 1 depending on the evaluation of the conditional  expression  expression.   Ex‐
              pressions  are composed of the primaries described below under CONDITIONAL EXPRESSIONS.  Word splitting
              and pathname expansion are not performed on the words between the [[ and ]]; tilde expansion, parameter
              and variable expansion, arithmetic expansion, command substitution, process substitution, and quote re‐
              moval are performed.  Conditional operators such as -f must be unquoted to be recognized as primaries.

              When used with [[, the < and > operators sort lexicographically using the current locale.

конструкция [[ -d /tmp ]] возвращает true если /tmp является директорией


Пункт 12

Основываясь на знаниях о просмотре текущих (например, PATH) и установке новых переменных; командах, которые мы рассматривали, добейтесь в выводе type -a bash в виртуальной машине наличия первым пунктом в списке:

bash is /tmp/new_path_directory/bash
bash is /usr/local/bin/bash
bash is /bin/bash
(прочие строки могут отличаться содержимым и порядком)

vagrant@vagrant:~$ type -a bash
bash is /tmp//new_path_directory/bash
bash is /usr/local/bin/bash
bash is /usr/bin/bash
bash is /bin/bash
vagrant@vagrant:~$

Пункт 13 

Чем отличается планирование команд с помощью batch и at?

 at      executes commands at a specified time.

batch   executes commands when system load levels permit; in other words, when the load average drops below 1.5,
               or the value specified in the invocation of atd.

отличаются типам запуска





