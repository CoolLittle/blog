---
title: Linux常用命令
tags:   
  - linux  
categories:
  - linux    
description: linux常用命令详解.....    
date: 2020-02-16 13:19:00
---

### 常用命令

#### 文件管理

##### awk

##### cat

连接多个文件并打印到标准输出。

    cat [选项] [文件]
        选项
            -n 或 --number：由 1 开始对所有输出的行数编号。
            -b 或 --number-nonblank：和 -n 相似，只不过对于空白行不编号。
            -s 或 --squeeze-blank：当遇到有连续两行以上的空白行，就代换为一行的空白行。
            -v 或 --show-nonprinting：使用 ^ 和 M- 符号，除了 LFD 和 TAB 之外。
            -E 或 --show-ends : 在每行结束处显示 $。
            -T 或 --show-tabs: 将 TAB 字符显示为 ^I。
            -A, --show-all：等价于 -vET。
            -e：等价于"-vE"选项；
            -t：等价于"-vT"选项
    
        实例
            # 合并显示多个文件
            cat ./1.log ./2.log ./3.log
            # 显示文件中的非打印字符、tab、换行符
            cat -A test.log
            # 压缩文件的空行
            cat -s test.log
            # 显示文件并在所有行开头附加行号
            cat -n test.log
            # 显示文件并在所有非空行开头附加行号
            cat -b test.log
            # 将标准输入的内容和文件内容一并显示
            echo '######' |cat - test.log

##### chgrp

用来变更文件或目录的所属群组。该命令用来改变指定文件所属的用户组。其中，组名可以是用户组的id，也可以是用户组的组名。文件名可以 是由空格分开的要改变属组的文件列表，也可以是由通配符描述的文件集合。如果用户不是该文件的文件主或超级用户(root)，则不能改变该文件的组。

在UNIX系统家族里，文件或目录权限的掌控以拥有者及所属群组来管理。您可以使用chgrp指令去变更文件与目录的所属群组，设置方式采用群组名称或群组识别码皆可。

    chgrp [选项][组群][文件|目录]
        选项
            -R 递归式地改变指定目录及其下的所有子目录和文件的所属的组
            -c或——changes：效果类似“-v”参数，但仅回报更改的部分；
            -f或--quiet或——silent：不显示错误信息；
            -h或--no-dereference：只对符号连接的文件作修改，而不是该其他任何相关文件；
            -H如果命令行参数是一个通到目录的符号链接，则遍历符号链接
            -R或——recursive：递归处理，将指令目录下的所有文件及子目录一并处理；
            -L遍历每一个遇到的通到目录的符号链接
            -P不遍历任何符号链接（默认）
            -v或——verbose：显示指令执行过程；
            --reference=<参考文件或目录>：把指定文件或目录的所属群组全部设成和参考文件或目录的所属群组相同；
        群组
            所属新群组
        文件|目录
            待修改的文件或者目录
            
        实例
            将/usr/meng及其子目录下的所有文件的用户组改为mengxin
            chgrp -R mengxin /usr/meng
            更改文件ah的组群所有者为 newuser
            [root@rhel ~]# chgrp newuser ah
            
##### chmod

用来变更文件或目录的权限

1. 通过符号组合的方式更改目标文件或目录的权限。
2. 通过八进制数的方式更改目标文件或目录的权限。
3. 通过参考文件的权限来更改目标文件或目录的权限。


    chmod [选项]... 模式[,模式]... 文件...
    chmod [选项]... 八进制模式 文件...
    chmod [选项]... --reference=参考文件 文件...
    将每个文件的权限模式变更至指定模式。
    使用 --reference 选项时，把指定文件的模式设置为与参考文件相同。
    
        选项
            -c, --changes          like verbose but report only when a change is made
            -f, --silent, --quiet  suppress most error messages
            -v, --verbose          output a diagnostic for every file processed
              --no-preserve-root  do not treat '/' specially (the default)
              --preserve-root    fail to operate recursively on '/'
              --reference=参考文件  使用参考文件的模式而非给定模式的值
            -R, --recursive        递归修改文件和目录
              --help		显示此帮助信息并退出
              --version		显示版本信息并退出
              
        实例：
            # 添加组用户的写权限。
            chmod g+w ./test.log
            # 删除其他用户的所有权限。
            chmod o= ./test.log
            # 使得所有用户都没有写权限。
            chmod a-w ./test.log
            # 当前用户具有所有权限，组用户有读写权限，其他用户只有读权限。
            chmod u=rwx, g=rw, o=r ./test.log
            # 等价的八进制数表示：
            chmod 754 ./test.log
            # 将目录以及目录下的文件都设置为所有用户拥有读写权限。
            # 注意，使用'-R'选项一定要保留当前用户的执行和读取权限，否则会报错！
            chmod -R a=rw ./testdir/
            # 根据其他文件的权限设置文件权限。
            chmod --reference=./1.log  ./test.log

说明：
    
>   u 符号代表当前用户。  
    g 符号代表和当前用户在同一个组的用户，以下简称组用户。   
    o 符号代表其他用户。    
    a 符号代表所有用户。    
    r 符号代表读权限以及八进制数4。    
    w 符号代表写权限以及八进制数2。    
    x 符号代表执行权限以及八进制数1。    
    X 符号代表如果目标文件是可执行文件或目录，可给其设置可执行权限。    
    s 符号代表设置权限suid和sgid，使用权限组合u+s设定文件的用户的ID位，g+s设置组用户ID位。    
    t 符号代表只有目录或文件的所有者才可以删除目录下的文件。    
    + 符号代表添加目标用户相应的权限。    
    - 符号代表删除目标用户相应的权限。   
    = 符号代表添加目标用户相应的权限，删除未提到的权限。

linux文件的用户权限说明：
    
    # 查看当前目录（包含隐藏文件）的长格式。
    ls -la
      -rw-r--r--   1 user  staff   651 Oct 12 12:53 .gitmodules
    
    # 第1位如果是d则代表目录，是-则代表普通文件。
    # 更多详情请参阅info coreutils 'ls invocation'（ls命令的info文档）的'-l'选项部分。
    # 第2到4位代表当前用户的权限。
    # 第5到7位代表组用户的权限。
    # 第8到10位代表其他用户的权限。

##### chown

用来变更文件或目录的拥有者或所属群组，该命令可以向某个用户授权，使该用户变成指定文件的所有者或者改变文件所属的组。用户可以是用户或者是用户D，用户组可以是组名或组id。文件名可以使由空格分开的文件列表，在文件名中可以包含通配符。

只有文件主和超级用户才可以便用该命令。

    chown [选项] [参数]
        选项
            -c或——changes：效果类似“-v”参数，但仅回报更改的部分；
            -f或--quite或——silent：不显示错误信息；
            -h或--no-dereference：只对符号连接的文件作修改，而不更改其他任何相关文件；
            -R或——recursive：递归处理，将指定目录下的所有文件及子目录一并处理；
            -v或——version：显示指令执行过程；
            --dereference：效果和“-h”参数相同；
            --help：在线帮助；
            --reference=<参考文件或目录>：把指定文件或目录的拥有者与所属群组全部设成和参考文件或目录的拥有者与所属群组相同；
            --version：显示版本信息。
        参数
            用户：组：指定所有者和所属工作组。当省略“：组”，仅改变文件所有者；
            文件：指定要改变所有者和工作组的文件列表。支持多个文件和目标，支持shell通配符。
            
        实例
            将目录/usr/meng及其下面的所有文件、子目录的文件主改成 liu：
            chown -R liu /usr/meng
         
##### cmp

用来比较两个文件是否有差异。当相互比较的两个文件完全一样时，则该指令不会显示任何信息。若发现有差异，预设会标示出第一个不通之处的字符和列数编号。若不指定任何文件名称或是所给予的文件名为“-”，则cmp指令会从标准输入设备读取数据。

    cmp [选项] [参数]
        选项
            -c或--print-chars：除了标明差异处的十进制字码之外，一并显示该字符所对应字符；
            -i<字符数目>或--ignore-initial=<字符数目>：指定一个数目；
            -l或——verbose：标示出所有不一样的地方；
            -s或--quiet或——silent：不显示错误信息；
            -v或——version：显示版本信息；
            --help：在线帮助。
        参数
            目录：比较两个文件的差异。
        
        实例
            使用cmp命令比较文件"testfile"和文件"testfile1"两个文件，则输入下面的命令：
            cmp testfile testfile1            #比较两个指定的文件
            
            在上述指令执行之前，使用cat命令查看两个指定的文件内容，如下所示：
            cat testfile                    #查看文件内容  
            Absncn 50                       #显示文件“testfile”  
            Asldssja 60  
            Jslkadjls 85 
            
            cat testfile1                   #查看文件内容  
            Absncn 50                       #显示文件“testfile1”  
            AsldssjE 62  
            Jslkadjls 85  
            然后，再执行cmp命令，并返回比较结果，具体如下所示：
            
            cmp testfile testfile1       #比较两个文件  
            testfile testfile1           #有差异：第8字节，第2行  
            
            注意：在比较结果中，只能够显示第一比较结果。   
            
##### cut

用来显示行中的指定部分，删除文件中指定字段。cut 经常用来显示文件的内容，类似于 type 命令。

说明：该命令有两项功能，其一是用来显示文件的内容，它依次读取由参数 file 所指 明的文件，将它们的内容输出到标准输出上；其二是连接两个或多个文件，如cut fl f2 > f3将把文件 fl 和 f2 的内容合并起来，然后通过输出重定向符“>”的作用，将它们放入文件 f3 中。

当文件较大时，文本在屏幕上迅速闪过（滚屏），用户往往看不清所显示的内容。因此，一般用 more 等命令分屏显示。为了控制滚屏，可以按 Ctrl+S 键，停止滚屏；按 Ctrl+Q 键可以恢复滚屏。按 Ctrl+C（中断）键可以终止该命令的执行，并且返回 Shell 提示符状态。

    cut（选项）（参数）
    
        选项
            -b：仅显示行中指定直接范围的内容；
            -c：仅显示行中指定范围的字符；
            -d：指定字段的分隔符，默认的字段分隔符为“TAB”；
            -f：显示指定字段的内容；
            -n：与“-b”选项连用，不分割多字节字符；
            --complement：补足被选择的字节、字符或字段；
            --out-delimiter= 字段分隔符：指定输出内容是的字段分割符；
            --help：显示指令的帮助信息；
            --version：显示指令的版本信息。
        参数
            文件：指定要进行内容过滤的文件。

实例

例如有一个学生报表信息，包含 No、Name、Mark、Percent：

    [root@localhost text]# cat test.txt
    No Name Mark Percent
    01 tom 69 91
    02 jack 71 87
    03 alex 68 98

使用 -f 选项提取指定字段（这里的 f 参数可以简单记忆为 --fields的缩写）：

    [root@localhost text]# cut -f 1 test.txt
    No
    01
    02
    03

    [root@localhost text]# cut -f2,3 test.txt
    Name Mark
    tom 69
    jack 71
    alex 68

--complement 选项提取指定字段之外的列（打印除了第二列之外的列）：

    [root@localhost text]# cut -f2 --complement test.txt
    No Mark Percent
    01 69 91
    02 71 87
    03 68 98

使用 -d 选项指定字段分隔符：

    [root@localhost text]# cat test2.txt
    No;Name;Mark;Percent
    01;tom;69;91
    02;jack;71;87
    03;alex;68;98

    [root@localhost text]# cut -f2 -d";" test2.txt
    Name
    tom
    jack
    alex

指定字段的字符或者字节范围

cut 命令可以将一串字符作为列来显示，字符字段的记法：

    N- ：从第 N 个字节、字符、字段到结尾；
    N-M ：从第 N 个字节、字符、字段到第 M 个（包括 M 在内）字节、字符、字段；
    -M ：从第 1 个字节、字符、字段到第 M 个（包括 M 在内）字节、字符、字段。

上面是记法，结合下面选项将摸个范围的字节、字符指定为字段：

    -b 表示字节；
    -c 表示字符；
    -f 表示定义字段。

示例

    [root@localhost text]# cat test.txt
    abcdefghijklmnopqrstuvwxyz
    abcdefghijklmnopqrstuvwxyz
    abcdefghijklmnopqrstuvwxyz
    abcdefghijklmnopqrstuvwxyz
    abcdefghijklmnopqrstuvwxyz
    
打印第 1 个到第 3 个字符：

    [root@localhost text]# cut -c1-3 test.txt
    abc
    abc
    abc
    abc
    abc

打印前 2 个字符：

    [root@localhost text]# cut -c-2 test.txt
    ab
    ab
    ab
    ab
    ab

打印从第 5 个字符开始到结尾：

    [root@localhost text]# cut -c5- test.txt
    efghijklmnopqrstuvwxyz
    efghijklmnopqrstuvwxyz
    efghijklmnopqrstuvwxyz
    efghijklmnopqrstuvwxyz
    efghijklmnopqrstuvwxyz

##### cp

将源文件或目录复制到目标文件或目录中 - copy files and directories
用来将一个或多个源文件或者目录复制到指定的目的文件或目录。它可以将单个源文件复制成一个指定文件名的具体的文件或一个已经存在的目录下。cp命令还支持同时复制多个文件，当一次复制多个文件时，目标文件参数必须是一个已经存在的目录，否则将出现错误

    cp [选项] [参数]
        选项：
            -a：此参数的效果和同时指定"-dpR"参数相同；
            -d：当复制符号连接时，把目标文件或目录也建立为符号连接，并指向与源文件或目录连接的原始文件或目录；
            -f：强行复制文件或目录，不论目标文件或目录是否已存在；
            -i：覆盖既有文件之前先询问用户；
            -l：对源文件建立硬连接，而非复制文件；
            -p：保留源文件或目录的属性；
            -R/r：递归处理，将指定目录下的所有文件与子目录一并处理；
            -s：对源文件建立符号连接，而非复制文件；
            -u：使用这项参数后只会在源文件的更改时间较目标文件更新时或是名称相互对应的目标文件并不存在时，才复制文件；
            -S：在备份文件时，用指定的后缀“SUFFIX”代替文件的默认后缀；
            -b：覆盖已存在的文件目标前将目标文件备份；
            -v：详细显示命令执行的操作。
        参数：
            源文件：制定源文件列表。默认情况下，cp命令不能复制目录，如果要复制目录，则必须使用-R选项；
            目标文件：指定目标文件。当“源文件”为多个文件时，要求“目标文件”为指定的目录。
    实例：
        （-r 是“递归”， -u 是“更新”，-v 是“详细”）
        $ cp -r -u -v /usr/men/tmp ~/men/tmp 

        版本备份 --backup=numbered 参数意思为“我要做个备份，而且是带编号的连续备份”。所以一个备份就是 1 号，第二个就是 2 号，等等。
        $ cp --force --backup=numbered test1.py test1.py
        $ ls
        test1.py test1.py.~1~ test1.py.~2~

        # 将当前目录下所有文件，复制到当前目录的兄弟目录 backup 文件夹中
        cp -rfb ./* ../backup

##### diff

比较给定的两个文件的不同

    diff [选项] [参数]
        选项：
            -<行数>：指定要显示多少行的文本。此参数必须与-c或-u参数一并使用；
            -a或——text：diff预设只会逐行比较文本文件；
            -b或--ignore-space-change：不检查空格字符的不同；
            -B或--ignore-blank-lines：不检查空白行；
            -c：显示全部内容，并标出不同之处；
            -C<行数>或--context<行数>：与执行“-c-<行数>”指令相同；
            -d或——minimal：使用不同的演算法，以小的单位来做比较；
            -D<巨集名称>或ifdef<巨集名称>：此参数的输出格式可用于前置处理器巨集；
            -e或——ed：此参数的输出格式可用于ed的script文件；
            -f或-forward-ed：输出的格式类似ed的script文件，但按照原来文件的顺序来显示不同处；
            -H或--speed-large-files：比较大文件时，可加快速度；
            -l<字符或字符串>或--ignore-matching-lines<字符或字符串>：若两个文件在某几行有所不同，而之际航同时都包含了选项中指定的字符或字符串，则不显示这两个文件的差异；
            -i或--ignore-case：不检查大小写的不同；
            -l或——paginate：将结果交由pr程序来分页；
            -n或——rcs：将比较结果以RCS的格式来显示；
            -N或--new-file：在比较目录时，若文件A仅出现在某个目录中，预设会显示：Only in目录，文件A 若使用-N参数，则diff会将文件A 与一个空白的文件比较；
            -p：若比较的文件为C语言的程序码文件时，显示差异所在的函数名称；
            -P或--unidirectional-new-file：与-N类似，但只有当第二个目录包含了第一个目录所没有的文件时，才会将这个文件与空白的文件做比较；
            -q或--brief：仅显示有无差异，不显示详细的信息；
            -r或——recursive：比较子目录中的文件；
            -s或--report-identical-files：若没有发现任何差异，仍然显示信息；
            -S<文件>或--starting-file<文件>：在比较目录时，从指定的文件开始比较；
            -t或--expand-tabs：在输出时，将tab字符展开；
            -T或--initial-tab：在每行前面加上tab字符以便对齐；
            -u，-U<列数>或--unified=<列数>：以合并的方式来显示文件内容的不同；
            -v或——version：显示版本信息；
            -w或--ignore-all-space：忽略全部的空格字符；
            -W<宽度>或--width<宽度>：在使用-y参数时，指定栏宽；
            -x<文件名或目录>或--exclude<文件名或目录>：不比较选项中所指定的文件或目录；
            -X<文件>或--exclude-from<文件>；您可以将文件或目录类型存成文本文件，然后在=<文件>中指定此文本文件；
            -y或--side-by-side：以并列的方式显示文件的异同之处；
            --left-column：在使用-y参数时，若两个文件某一行内容相同，则仅在左侧的栏位显示该行内容；
            --suppress-common-lines：在使用-y参数时，仅显示不同之处。
        参数：
            文件1：指定要比较的第一个文件；
            文件2：指定要比较的第二个文件。
    实例：
       将目录/usr/li下的文件"test.txt"与当前目录下的文件"test.txt"进行比较，输入如下命令：

       diff /usr/li test.txt     #使用diff指令对文件进行比较

       上面的命令执行后，会将比较后的不同之处以指定的形式列出，如下所示：
       n1 a n3,n4  
       n1,n2 d n3  
       n1,n2 c n3,n4 

其中，字母"a"、"d"、"c"分别表示添加、删除及修改操作。而"n1"、"n2"表示在文件1中的行号，"n3"、"n4"表示在文件2中的行号。

注意：以上说明指定了两个文件中不同处的行号及其相应的操作。在输出形式中，每一行后面将跟随受到影响的若干行。其中，以<开始的行属于文件1，以>开始的行属于文件2。

##### file

用来探测给定文件的类型

    file [选项] [参数]
        -b：列出辨识结果时，不显示文件名称；
        -c：详细显示指令执行过程，便于排错或分析程序执行的情形；
        -f<名称文件>：指定名称文件，其内容有一个或多个文件名称时，让file依序辨识这些文件，格式为每列一个文件名称；
        -L：直接显示符号连接所指向的文件类别；
        -m<魔法数字文件>：指定魔法数字文件；
        -v：显示版本信息；
        -z：尝试去解读压缩文件的内容。
    实例：
        显示文件类型

            [root@localhost ~]# file install.log
            install.log: UTF-8 Unicode text

            [root@localhost ~]# file -b install.log      <== 不显示文件名称
            UTF-8 Unicode text

            [root@localhost ~]# file -i install.log      <== 显示MIME类别。
            install.log: text/plain; charset=utf-8

            [root@localhost ~]# file -b -i install.log
            text/plain; charset=utf-8
            
        显示符号链接的文件类型

            [root@localhost ~]# ls -l /var/mail
            lrwxrwxrwx 1 root root 10 08-13 00:11 /var/mail -> spool/mail

            [root@localhost ~]# file /var/mail
            /var/mail: symbolic link to `spool/mail'

            [root@localhost ~]# file -L /var/mail
            /var/mail: directory

            [root@localhost ~]# file /var/spool/mail
            /var/spool/mail: directory

            [root@localhost ~]# file -L /var/spool/mail
            /var/spool/mail: directory


##### find

##### ln

##### less

##### locate

##### lsattr

##### more

##### mv

##### paste 

##### read 

##### rcp
 
##### rm

##### split

##### touch

##### which

##### whereis



#### 文件目录管理

##### cd 

cd --change directory 切换工作目录

    cd [选项] (目录参数)
        常用参数:
            -p 如果要切换到的目标目录是一个符号连接，直接切换到符号连接指向的目标目录
            -L 如果要切换的目标目录是一个符号的连接，直接切换到字符连接名代表的目录，而非符号连接所指向的目标目录。
            - 当仅实用"-"一个选项时，当前工作目录将被切换到环境变量"OLDPWD"所表示的目录。
        实例：
            cd    进入用户主目录；
            cd ~  进入用户主目录；
            cd -  返回进入此目录之前所在的目录；
            cd ..  返回上级目录（若当前目录为“/“，则执行完后还在“/"；".."为上级目录的意思）；
            cd ../..  返回上两级目录；
            cd !$  把上个命令的参数作为cd参数使用。

##### pwd

显示当前工作目录的绝对路径

##### ls

`ls - list directory contents ` 列出当前目录内容

说明：ll = ls -lh，开启方式：vim ~/.bashrc 

	常用参数：
        -C     # 多列输出，纵向排序。
        -F     # 每个目录名加 "/" 后缀，每个 FIFO 名加 "|" 后缀， 每个可运行名加“ * ”后缀。
        -R     # 递归列出遇到的子目录。
        -a     # 列出所有文件，包括以 "." 开头的隐含文件。
        -c     # 使用“状态改变时间”代替“文件修改时间”为依据来排序（使用“-t”选项时）或列出（使用“-l”选项时）。
        -d     # 将目录名象其它文件一样列出，而不是列出它们的内容。
        -i     # 输出文件前先输出文件系列号（即 i 节点号: i-node number）。 -l  列出（以单列格式）文件模式（file mode），文件的链     接数，所有者名，组名，文件大小（以字节为单位），时间信息，及文件名。缺省时，时间信息显示最近修改时间；可以以选项“-c”和“-u”选择显示其它两种时间信息。对于设    备文件，原先显示文件大小的区域通常显示的是主要和次要的号（majorand minor device numbers）。
        -q     # 将文件名中的非打印字符输出为问号。（对于到终端的输出这是缺省的。）
        -r     # 逆序排列。
        -t     # 按时间信息排序。
        -u     # 使用最近访问时间代替最近修改时间为依据来排序（使用“-t”选项时）或列出（使用“-l”选项时）。
        -1     # 单列输出。
        -1, --format=single-column  # 一行输出一个文件（单列输出）。如标准输出不是到终端，此选项就是缺省选项。
        -a, --all # 列出目录中所有文件，包括以“.”开头的文件。
        -b, --escape # 把文件名中不可输出的字符用反斜杠加字符编号(就象在 C 语言里一样)的形式列出。
        -c, --time=ctime, --time=status
            # 按文件状态改变时间（i节点中的ctime）排序并输出目录内
            # 容。如采用长格式输出（选项“-l”），使用文件的状态改
            # 变时间取代文件修改时间。【译注：所谓文件状态改变（i节
            # 点中以ctime标志），既包括文件被修改，又包括文件属性（ 如所有者、组、链接数等等）的变化】
        -d, --directory
            # 将目录名象其它文件一样列出，而不是列出它们的内容。
        -f    #  不排序目录内容；按它们在磁盘上存储的顺序列出。同时启 动“ -a ”选项，如果在“ -f ”之前存在“ -l
            # ”、“ - -color ”或“ -s ”，则禁止它们。
        -g    # 忽略，为兼容UNIX用。
        -i, --inode
            # 在每个文件左边打印  i  节点号（也叫文件序列号和索引号:  file  serial  number and index num‐
            # ber）。i节点号在每个特定的文件系统中是唯一的。
        -k, --kilobytes
            # 如列出文件大小，则以千字节KB为单位。
        -l, --format=long, --format=verbose
            # 除每个文件名外，增加显示文件类型、权限、硬链接数、所       有者名、组名、大小（        byte
            # ）、及时间信息（如未指明是      其它时间即指修改时间）。对于6个月以上的文件或超出未来     1
            # 小时的文件，时间信息中的时分将被年代取代。
            # 每个目录列出前，有一行“总块数”显示目录下全部文件所       占的磁盘空间。块默认是        1024
            # 字节；如果设置了   POSIXLY_CORRECT  的环境变量，除非用“  -k  ”选项，则默认块大小是  512  字
            # 节。每一个硬链接都计入总块数（因此可能重复计数），这无 疑是个缺点。

        # 列出的权限类似于以符号表示（文件）模式的规范。但是 ls
            # 在每套权限的第三个字符中结合了多位（ multiple bits ） 的信息，如下： s 如果设置了  setuid
            # 位或 setgid   位，而且也设置了相应的可执行位。 S 如果设置了 setuid 位或 setgid
            # 位，但是没有设置相应的可执行位。 t 如果设置了  sticky  位，而且也设置了相应的可执行位。  T
            # 如果设置了 sticky 位，但是没有设置相应的可执行位。              x
            # 如果仅仅设置了可执行位而非以上四种情况。 - 其它情况（即可执行位未设置）。
        -m, --format=commas
            # 水平列出文件，每行尽可能多，相互用逗号和一个空格分隔。
        -n, --numeric-uid-gid
            # 列出数字化的 UID 和 GID 而不是用户名和组名。
        -o    #  以长格式列出目录内容，但是不显示组信息。等于使用“         --format=long          --no-group
            # ”选项。提供此选项是为了与其它版本的 ls 兼容。
        -p    #  在每个文件名后附上一个字符以说明该文件的类型。类似“ -F ”选项但是不 标示可执行文件。
        -q, --hide-control-chars
            # 用问号代替文件名中非打印的字符。这是缺省选项。
        -r, --reverse
            # 逆序排列目录内容。
        -s, --size
            # 在每个文件名左侧输出该文件的大小，以    1024   字节的块为单位。如果设置了   POSIXLY_CORRECT
            # 的环境变量，除非用“ -k ”选项，块大小是 512 字节。
        -t, --sort=time
            # 按文件最近修改时间（ i 节点中的 mtime ）而不是按文件名字典序排序，新文件 靠前。
        -u, --time=atime, --time=access, --time=use
            # 类似选项“    -t    ”，但是用文件最近访问时间（    i     节点中的     atime     ）取代文件修
            # 改时间。如果使用长格式列出，打印的时间是最近访问时间。
        -w, --width cols
            # 假定屏幕宽度是      cols      （      cols     以实际数字取代）列。如未用此选项，缺省值是这
            # 样获得的：如可能先尝试取自终端驱动，否则尝试取自环境变量          COLUMNS          （如果设
            # 置了的话），都不行则取 80 。

        -x, --format=across, --format=horizontal
            # 多列输出，横向排序。

        -A, --almost-all
            # 显示除 "." 和 ".." 外的所有文件。

        -B, --ignore-backups
            # 不输出以“ ~ ”结尾的备份文件，除非已经在命令行中给出。

        -C, --format=vertical
            # 多列输出，纵向排序。当标准输出是终端时这是缺省项。使用命令名 dir 和 d 时， 则总是缺省的。

        -D, --dired
            # 当采用长格式（“-l”选项）输出时，在主要输出后，额外打印一行：  //DIRED//  BEG1 END1 BEG2
            # END2 ...

        # BEGn 和 ENDn 是无符号整数，记录每个文件名的起始、结束位置在输出中的位置（
        #        字节偏移量）。这使得          Emacs          易于找到文件名，即使文件名包含空格或换行等非正
        #        常字符也无需特异的搜索。
        # 
        # 如果目录是递归列出的（“ -R ”选项），每个子目录后列出类似一行：
            # //SUBDIRED//  BEG1 END1 ...  【译注：我测试了 TurboLinux4.0 和 RedHat6.1 ，发现它们都是在 “
            # //DIRED//     BEG1...     ”之后列出“     //SUBDIRED//     BEG1     ...      ”，也即只有一个
            # 而不是在每个子目录后都有。而且“ //SUBDIRED// BEG1 ... ”列出的是各个子目 录名的偏移。】

        -F, --classify, --file-type
            # 在每个文件名后附上一个字符以说明该文件的类型。“  * ”表示普通的可执行文件； “ / ”表示目录；“
            # @ ”表示符号链接；“ | ”表示FIFOs；“ = ”表示套接字 (sockets) ；什么也没有则表示普通文件。

        -G, --no-group
            # 以长格式列目录时不显示组信息。

        -I, --ignorepattern
            # 除非在命令行中给定，不要列出匹配shell文件名匹配式（pattern ，不是指一般
            # 表达式）的文件。在shell中，文件名以"."起始的不与在文件名匹配式(pattern)
            # 开头的通配符匹配。

        -L, --dereference
            # 列出符号链接指向的文件的信息，而不是符号链接本身。

        -N, --literal
            # 不要用引号引起文件名。

        -Q, --quote-name
            # 用双引号引起文件名，非打印字符以 C 语言的方法表示。

        -R, --recursive
            # 递归列出全部目录的内容。

        -S, --sort=size
            # 按文件大小而不是字典序排序目录内容，大文件靠前。

        -T, --tabsize cols
            # 假定每个制表符宽度是 cols 。缺省为 8。为求效率， ls 可能在输出中使用制表符。  若 cols 为
            0，则不使用制表符。

        -U, --sort=none
            # 不排序目录内容；按它们在磁盘上存储的顺序列出。（选项“-U”和“-f”的不
            # 同是前者不启动或禁止相关的选项。）这在列很大的目录时特别有用，因为不加排序
            # 能显著的加快速度。

        -X, --sort=extension
            # 按文件扩展名（由最后的 "." 之后的字符组成）的字典序排序。没有扩展名的先列 出。

        --color[=when]
            # 指定是否使用颜色区别文件类别。环境变量  LS_COLORS  指定使用的颜色。如何设置 这个变量见 dir‐
            # colors(1) 。 when 可以被省略，或是以下几项之一：
        none # 不使用颜色，这是缺省项。
            # auto 仅当标准输出是终端时使用。 always 总是使用颜色。指定 --color 而且省略 when  时就等同于
            # --color=always 。

        --full-time
            # 列出完整的时间，而不是使用标准的缩写。格式如同          date(1)          的缺省格式；此格式
            # 是不能改变的，但是你可以用 cut(1) 取出其中的日期字串并将结果送至命令 “ date -d ”。

        # 输出的时间包括秒是非常有用的。（ Unix 文件系统储存文件的时间信息精确到秒，
            # 因此这个选项已经给出了系统所知的全部信息。）例如，当你有一个         Makefile          文件
            # 不能恰当的生成文件时，这个选项会提供帮助。

#### 磁盘管理

##### df 

显示磁盘空间 — 用于显示磁盘分区上的可使用的磁盘空间。默认显示单位为KB。可以利用该命令来获取硬盘被占用了多少空间，目前还剩下多少空间等信息。

    df [选项] [参数]
        选项
            -a或--all：包含全部的文件系统；
            --block-size=<区块大小>：以指定的区块大小来显示区块数目；
            -h或--human-readable：以可读性较高的方式来显示信息；
            -H或--si：与-h参数相同，但在计算时是以1000 Bytes为换算单位而非1024 Bytes；
            -i或--inodes：显示inode的信息；
            -k或--kilobytes：指定区块大小为1024字节；
            -l或--local：仅显示本地端的文件系统；
            -m或--megabytes：指定区块大小为1048576字节；
            --no-sync：在取得磁盘使用信息前，不要执行sync指令，此为预设值；
            -P或--portability：使用POSIX的输出格式；
            --sync：在取得磁盘使用信息前，先执行sync指令；
            -t<文件系统类型>或--type=<文件系统类型>：仅显示指定文件系统类型的磁盘信息；
            -T或--print-type：显示文件系统的类型；
            -x<文件系统类型>或--exclude-type=<文件系统类型>：不要显示指定文件系统类型的磁盘信息；
            --help：显示帮助；
            --version：显示版本信息。
    参数：文件 — 指定文件系统上的文件。

##### fdisk

用于观察硬盘实体使用情况，也可对硬盘分区。它采用传统的问答式界面，而非类似DOS fdisk的cfdisk互动式操作界面，因此在使用上较为不便，但功能却丝毫不打折扣。

    fdisk [选项] [参数]
        选项
             -b <大小>             扇区大小(512、1024、2048或4096)
             -c[=<模式>]           兼容模式：“dos”或“nondos”(默认)
             -h                    打印此帮助文本
             -u[=<单位>]           显示单位：“cylinders”(柱面)或“sectors”(扇区，默认)
             -v                    打印程序版本
             -C <数字>             指定柱面数
             -H <数字>             指定磁头数
             -S <数字>             指定每个磁道的扇区数
        参数
            设备文件：指定要进行分区或者显示分区的硬盘设备文件。
 
 实例
 
 首先选择要进行操作的磁盘：
 
    [root@localhost ~]# fdisk /dev/sdb
    
 输入m列出可以执行的命令：
 
     command (m for help): m
     Command action
        a   toggle a bootable flag
        b   edit bsd disklabel
        c   toggle the dos compatibility flag
        d   delete a partition
        l   list known partition types
        m   print this menu
        n   add a new partition
        o   create a new empty DOS partition table
        p   print the partition table
        q   quit without saving changes
        s   create a new empty Sun disklabel
        t   change a partition's system id
        u   change display/entry units
        v   verify the partition table
        w   write table to disk and exit
        x   extra functionality (experts only)
 
 输入p列出磁盘目前的分区情况：
 
     Command (m for help): p
     
     Disk /dev/sdb: 3221 MB, 3221225472 bytes
     255 heads, 63 sectors/track, 391 cylinders
     Units = cylinders of 16065 * 512 = 8225280 bytes
     
        Device Boot      Start         End      Blocks   Id  System
     /dev/sdb1               1           1        8001   8e  Linux LVM
     /dev/sdb2               2          26      200812+  83  Linux
 
 输入d然后选择分区，删除现有分区：
 
     Command (m for help): d
     Partition number (1-4): 1
     
     Command (m for help): d
     Selected partition 2
 
 查看分区情况，确认分区已经删除：
 
     Command (m for help): print
     
     Disk /dev/sdb: 3221 MB, 3221225472 bytes
     255 heads, 63 sectors/track, 391 cylinders
     Units = cylinders of 16065 * 512 = 8225280 bytes
     
        Device Boot      Start         End      Blocks   Id  System
     
     Command (m for help):
 
 输入n建立新的磁盘分区，首先建立两个主磁盘分区：
 
     Command (m for help): n
     Command action
        e   extended
        p   primary partition (1-4)
     p    //建立主分区
     Partition number (1-4): 1  //分区号
     First cylinder (1-391, default 1):  //分区起始位置
     Using default value 1
     last cylinder or +size or +sizeM or +sizeK (1-391, default 391): 100  //分区结束位置，单位为扇区
     
     Command (m for help): n  //再建立一个分区
     Command action
        e   extended
        p   primary partition (1-4)
     p 
     Partition number (1-4): 2  //分区号为2
     First cylinder (101-391, default 101):
     Using default value 101
     Last cylinder or +size or +sizeM or +sizeK (101-391, default 391): +200M  //分区结束位置，单位为M
 
 确认分区建立成功：
 
     Command (m for help): p
     
     Disk /dev/sdb: 3221 MB, 3221225472 bytes
     255 heads, 63 sectors/track, 391 cylinders
     Units = cylinders of 16065 * 512 = 8225280 bytes
     
        Device Boot      Start         End      Blocks   Id  System
     /dev/sdb1               1         100      803218+  83  Linux
     /dev/sdb2             101         125      200812+  83  Linux
 
 再建立一个逻辑分区：
 
     Command (m for help): n
     Command action
        e   extended
        p   primary partition (1-4)
     e  //选择扩展分区
     Partition number (1-4): 3
     First cylinder (126-391, default 126):
     Using default value 126
     Last cylinder or +size or +sizeM or +sizeK (126-391, default 391):
     Using default value 391
 
 确认扩展分区建立成功：
 
     Command (m for help): p
     
     Disk /dev/sdb: 3221 MB, 3221225472 bytes
     255 heads, 63 sectors/track, 391 cylinders
     Units = cylinders of 16065 * 512 = 8225280 bytes
     
        Device Boot      Start         End      Blocks   Id  System
     /dev/sdb1               1         100      803218+  83  Linux
     /dev/sdb2             101         125      200812+  83  Linux
     /dev/sdb3             126         391     2136645    5  Extended
 
 在扩展分区上建立两个逻辑分区：
 
     Command (m for help): n
     Command action
        l   logical (5 or over)
        p   primary partition (1-4)
     l //选择逻辑分区
     First cylinder (126-391, default 126):
     Using default value 126
     Last cylinder or +size or +sizeM or +sizeK (126-391, default 391): +400M    
     
     Command (m for help): n
     Command action
        l   logical (5 or over)
        p   primary partition (1-4)
     l
     First cylinder (176-391, default 176):
     Using default value 176
     Last cylinder or +size or +sizeM or +sizeK (176-391, default 391):
     Using default value 391
 
 确认逻辑分区建立成功：
 
     Command (m for help): p
     
     Disk /dev/sdb: 3221 MB, 3221225472 bytes
     255 heads, 63 sectors/track, 391 cylinders
     Units = cylinders of 16065 * 512 = 8225280 bytes
     
        Device Boot      Start         End      Blocks   Id  System
     /dev/sdb1               1         100      803218+  83  Linux
     /dev/sdb2             101         125      200812+  83  Linux
     /dev/sdb3             126         391     2136645    5  Extended
     /dev/sdb5             126         175      401593+  83  Linux
     /dev/sdb6             176         391     1734988+  83  Linux
     
     Command (m for help):
 
 从上面的结果我们可以看到，在硬盘sdb我们建立了2个主分区（sdb1，sdb2），1个扩展分区（sdb3），2个逻辑分区（sdb5，sdb6）
 
 注意：主分区和扩展分区的磁盘号位1-4，也就是说最多有4个主分区或者扩展分区，逻辑分区开始的磁盘号为5，因此在这个实验中试没有sdb4的。
 
 最后对分区操作进行保存：
 
     Command (m for help): w
     The partition table has been altered!
     
     Calling ioctl() to re-read partition table.
     Syncing disks.
 
 建立好分区之后我们还需要对分区进行格式化才能在系统中使用磁盘。
 
 在sdb1上建立ext2分区：
 
     [root@localhost ~]# mkfs.ext2 /dev/sdb1
     mke2fs 1.39 (29-May-2006)
     Filesystem label=
     OS type: Linux
     Block size=4096 (log=2)
     Fragment size=4096 (log=2)
     100576 inodes, 200804 blocks
     10040 blocks (5.00%) reserved for the super user
     First data block=0
     Maximum filesystem blocks=209715200
     7 block groups
     32768 blocks per group, 32768 fragments per group
     14368 inodes per group
     Superblock backups stored on blocks:
             32768, 98304, 163840
     
     Writing inode tables: done                           
     Writing superblocks and filesystem accounting information: done
     
     This filesystem will be automatically checked every 32 mounts or
     180 days, whichever comes first.  Use tune2fs -c or -i to override.
 
 在sdb6上建立ext3分区：
 
     [root@localhost ~]# mkfs.ext3 /dev/sdb6
     mke2fs 1.39 (29-May-2006)
     Filesystem label=
     OS type: Linux
     Block size=4096 (log=2)
     Fragment size=4096 (log=2)
     217280 inodes, 433747 blocks
     21687 blocks (5.00%) reserved for the super user
     First data block=0
     Maximum filesystem blocks=444596224
     14 block groups
     32768 blocks per group, 32768 fragments per group
     15520 inodes per group
     Superblock backups stored on blocks:
             32768, 98304, 163840, 229376, 294912
     
     Writing inode tables: done                           
     Creating journal (8192 blocks): done
     Writing superblocks and filesystem accounting information: done
     
     This filesystem will be automatically checked every 32 mounts or
     180 days, whichever comes first.  Use tune2fs -c or -i to override.
     [root@localhost ~]#
 
 建立两个目录/oracle和/web，将新建好的两个分区挂载到系统：
 
     [root@localhost ~]# mkdir /oracle
     [root@localhost ~]# mkdir /web
     [root@localhost ~]# mount /dev/sdb1 /oracle
     [root@localhost ~]# mount /dev/sdb6 /web
 
 查看分区挂载情况：
 
    [root@localhost ~]# df -h
    
    文件系统              容量  已用 可用 已用% 挂载点
     /dev/mapper/VolGroup00-LogVol00
                           6.7G  2.8G  3.6G  44% /
     /dev/sda1              99M   12M   82M  13% /boot
     tmpfs                 125M     0  125M   0% /dev/shm
     /dev/sdb1             773M  808K  733M   1% /oracle
     /dev/sdb6             1.7G   35M  1.6G   3% /web
 
 如果需要每次开机自动挂载则需要修改/etc/fstab文件，加入两行配置：
 
     [root@localhost ~]# vim /etc/fstab
     
     /dev/VolGroup00/LogVol00 /                       ext3    defaults        1 1
     LABEL=/boot             /boot                   ext3    defaults        1 2
     tmpfs                   /dev/shm                tmpfs   defaults        0 0
     devpts                  /dev/pts                devpts  gid=5,mode=620  0 0
     sysfs                   /sys                    sysfs   defaults        0 0
     proc                    /proc                   proc    defaults        0 0
     /dev/VolGroup00/LogVol01 swap                    swap    defaults        0 0
     /dev/sdb1               /oracle                 ext2    defaults        0 0
     /dev/sdb6               /web                    ext3    defaults        0 0
 
 
##### mount 

用于挂载Linux系统外的文件

    mount [-hV]
    mount -a [-fFnrsvw] [-t vfstype]
    mount [-fnrsvw] [-o options [,...]] device | dir
    mount [-fnrsvw] [-t vfstype] [-o options] device dir
    
        选项
            -V：显示程序版本
            -h：显示辅助讯息
            -v：显示较讯息，通常和 -f 用来除错。
            -a：将 /etc/fstab 中定义的所有档案系统挂上。
            -F：这个命令通常和 -a 一起使用，它会为每一个 mount 的动作产生一个行程负责执行。在系统需要挂上大量 NFS 档案系统时可以加快挂上的动作。
            -f：通常用在除错的用途。它会使 mount 并不执行实际挂上的动作，而是模拟整个挂上的过程。通常会和 -v 一起使用。
            -n：一般而言，mount 在挂上后会在 /etc/mtab 中写入一笔资料。但在系统中没有可写入档案系统存在的情况下可以用这个选项取消这个动作。
            -s-r：等于 -o ro
            -w：等于 -o rw
            -L：将含有特定标签的硬盘分割挂上。
            -U：将档案分割序号为 的档案系统挂下。-L 和 -U 必须在/proc/partition 这种档案存在时才有意义。
            -t：指定档案系统的型态，通常不必指定。mount 会自动选择正确的型态。
            -o async：打开非同步模式，所有的档案读写动作都会用非同步模式执行。
            -o sync：在同步模式下执行。
            -o atime、-o noatime：当 atime 打开时，系统会在每次读取档案时更新档案的『上一次调用时间』。当我们使用 flash 档案系统时可能会选项把这个选项关闭以减少写入的次数。
            -o auto、-o noauto：打开/关闭自动挂上模式。
            -o defaults:使用预设的选项 rw, suid, dev, exec, auto, nouser, and async.
            -o dev、-o nodev-o exec、-o noexec允许执行档被执行。
            -o suid、-o nosuid：
            允许执行档在 root 权限下执行。
            -o user、-o nouser：使用者可以执行 mount/umount 的动作。
            -o remount：将一个已经挂下的档案系统重新用不同的方式挂上。例如原先是唯读的系统，现在用可读写的模式重新挂上。
            -o ro：用唯读模式挂上。
            -o rw：用可读写模式挂上。
            -o loop=：使用 loop 模式用来将一个档案当成硬盘分割挂上系统。
   
实例：
    
将 /dev/hda1 挂在 /mnt 之下        

    #mount /dev/hda1 /mnt
    
将 /dev/hda1 用唯读模式挂在 /mnt 之下。

    #mount -o ro /dev/hda1 /mnt
    
将 /tmp/image.iso 这个光碟的 image 档使用 loop 模式挂在 /mnt/cdrom 之下。用这种方法可以将一般网络上可以找到的 Linux 光 碟 ISO 档在不烧录成光碟的情况下检视其内容。
    
    #mount -o loop /tmp/image.iso /mnt/cdrom
    
##### umount   

用于卸载已经加载的文件系统。利用设备名或挂载点都能umount文件系统，不过最好还是通过挂载点卸载，以免使用绑定挂载（一个设备，多个挂载点）时产生混乱。 
    
    umount [选项] (参数)
        选项:
            -a：卸除/etc/mtab中记录的所有文件系统；
            -h：显示帮助；
            -n：卸除时不要将信息存入/etc/mtab文件中；
            -r：若无法成功卸除，则尝试以只读的方式重新挂入文件系统；
            -t<文件系统类型>：仅卸除选项中所指定的文件系统；
            -v：执行时显示详细的信息；
            -V：显示版本信息。
    
实例：

下面两条命令分别通过设备名和挂载点卸载文件系统，同时输出详细信息：

通过设备名卸载

    umount -v /dev/sda1
    /dev/sda1 umounted

通过挂载点卸载

    umount -v /mnt/mymount/
    /tmp/diskboot.img umounted

如果设备正忙，卸载即告失败。卸载失败的常见原因是，某个打开的shell当前目录为挂载点里的某个目录：

    umount -v /mnt/mymount/
    umount: /mnt/mymount: device is busy
    umount: /mnt/mymount: device is busy

有时，导致设备忙的原因并不好找。碰到这种情况时，可以用lsof列出已打开文件，然后搜索列表查找待卸载的挂载点：

    lsof | grep mymount         查找mymount分区里打开的文件
    bash   9341  francois  cwd   DIR   8,1   1024    2 /mnt/mymount

从上面的输出可知，mymount分区无法卸载的原因在于，francois运行的PID为9341的bash进程。

对付系统文件正忙的另一种方法是执行延迟卸载：

    umount -vl /mnt/mymount/     执行延迟卸载

延迟卸载（lazy unmount）会立即卸载目录树里的文件系统，等到设备不再繁忙时才清理所有相关资源。卸载可移动存储介质还可以用eject命令。下面这条命令会卸载cd并弹出CD：

    eject /dev/cdrom      卸载并弹出CD 
    
    
### 参考

[Linux命令查询](https://jaywcjlove.gitee.io/linux-command)    
[菜鸟教程](https://www.runoob.com/linux)    
[视频教程](https://www.bilibili.com/video/av21303002)    
