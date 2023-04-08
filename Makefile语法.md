<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [:point_right:文件名](#point_right%E6%96%87%E4%BB%B6%E5%90%8D)
  - [包含其它Makefile](#%E5%8C%85%E5%90%AB%E5%85%B6%E5%AE%83makefile)
  - [工作方式](#%E5%B7%A5%E4%BD%9C%E6%96%B9%E5%BC%8F)
- [make 规则](#make-%E8%A7%84%E5%88%99)
  - [:point_right:基本语法](#point_right%E5%9F%BA%E6%9C%AC%E8%AF%AD%E6%B3%95)
  - [:point_right:.PHONY 伪目标](#point_rightphony-%E4%BC%AA%E7%9B%AE%E6%A0%87)
  - [:point_right:多目标](#point_right%E5%A4%9A%E7%9B%AE%E6%A0%87)
  - [:point_right:显示命令](#point_right%E6%98%BE%E7%A4%BA%E5%91%BD%E4%BB%A4)
  - [:point_right:执行命令](#point_right%E6%89%A7%E8%A1%8C%E5%91%BD%E4%BB%A4)
  - [:point_right:忽略错误](#point_right%E5%BF%BD%E7%95%A5%E9%94%99%E8%AF%AF)
  - [:point_right:基本变量](#point_right%E5%9F%BA%E6%9C%AC%E5%8F%98%E9%87%8F)
  - [override 指示符](#override-%E6%8C%87%E7%A4%BA%E7%AC%A6)
  - [:point_right:环境变量](#point_right%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F)
  - [变量内容替换](#%E5%8F%98%E9%87%8F%E5%86%85%E5%AE%B9%E6%9B%BF%E6%8D%A2)
  - [:point_right:追加变量值](#point_right%E8%BF%BD%E5%8A%A0%E5%8F%98%E9%87%8F%E5%80%BC)
- [:point_right:分支](#point_right%E5%88%86%E6%94%AF)
  - [if 函数](#if-%E5%87%BD%E6%95%B0)
- [:point_right:循环](#point_right%E5%BE%AA%E7%8E%AF)
- [函数](#%E5%87%BD%E6%95%B0)
  - [:point_right:函数调用](#point_right%E5%87%BD%E6%95%B0%E8%B0%83%E7%94%A8)
  - [:point_right:自定义函数](#point_right%E8%87%AA%E5%AE%9A%E4%B9%89%E5%87%BD%E6%95%B0)
  - [字符串处理函数](#%E5%AD%97%E7%AC%A6%E4%B8%B2%E5%A4%84%E7%90%86%E5%87%BD%E6%95%B0)
    - [:point_right:subst - 标准替换函数](#point_rightsubst---%E6%A0%87%E5%87%86%E6%9B%BF%E6%8D%A2%E5%87%BD%E6%95%B0)
    - [patsubst - 正则替换函数](#patsubst---%E6%AD%A3%E5%88%99%E6%9B%BF%E6%8D%A2%E5%87%BD%E6%95%B0)
    - [strip - 去掉空格](#strip---%E5%8E%BB%E6%8E%89%E7%A9%BA%E6%A0%BC)
    - [findstring- 查找一个字符串](#findstring--%E6%9F%A5%E6%89%BE%E4%B8%80%E4%B8%AA%E5%AD%97%E7%AC%A6%E4%B8%B2)
    - [filter - 过滤掉一个指定的字符串](#filter---%E8%BF%87%E6%BB%A4%E6%8E%89%E4%B8%80%E4%B8%AA%E6%8C%87%E5%AE%9A%E7%9A%84%E5%AD%97%E7%AC%A6%E4%B8%B2)
    - [filter-out - 是一个反过滤函数](#filter-out---%E6%98%AF%E4%B8%80%E4%B8%AA%E5%8F%8D%E8%BF%87%E6%BB%A4%E5%87%BD%E6%95%B0)
    - [sort - 单词排序](#sort---%E5%8D%95%E8%AF%8D%E6%8E%92%E5%BA%8F)
    - [:point_right:word - 取单词](#point_rightword---%E5%8F%96%E5%8D%95%E8%AF%8D)
    - [wordlist - 取子串](#wordlist---%E5%8F%96%E5%AD%90%E4%B8%B2)
    - [words - 统计单词数目](#words---%E7%BB%9F%E8%AE%A1%E5%8D%95%E8%AF%8D%E6%95%B0%E7%9B%AE)
    - [firstword - 取首个单词](#firstword---%E5%8F%96%E9%A6%96%E4%B8%AA%E5%8D%95%E8%AF%8D)
  - [路径文件名处理函数](#%E8%B7%AF%E5%BE%84%E6%96%87%E4%BB%B6%E5%90%8D%E5%A4%84%E7%90%86%E5%87%BD%E6%95%B0)
    - [:point_right:dir - 取路径名的目录](#point_rightdir---%E5%8F%96%E8%B7%AF%E5%BE%84%E5%90%8D%E7%9A%84%E7%9B%AE%E5%BD%95)
    - [:point_right:notdir - 取文件名](#point_rightnotdir---%E5%8F%96%E6%96%87%E4%BB%B6%E5%90%8D)
    - [suffix - 取文件名后缀](#suffix---%E5%8F%96%E6%96%87%E4%BB%B6%E5%90%8D%E5%90%8E%E7%BC%80)
    - [basename - 取文件名前缀](#basename---%E5%8F%96%E6%96%87%E4%BB%B6%E5%90%8D%E5%89%8D%E7%BC%80)
    - [:point_right:join - 单词连接](#point_rightjoin---%E5%8D%95%E8%AF%8D%E8%BF%9E%E6%8E%A5)
    - [:point_right:wildcard - 列出所有符号匹配模式的文件](#point_rightwildcard---%E5%88%97%E5%87%BA%E6%89%80%E6%9C%89%E7%AC%A6%E5%8F%B7%E5%8C%B9%E9%85%8D%E6%A8%A1%E5%BC%8F%E7%9A%84%E6%96%87%E4%BB%B6)
  - [:point_right:call  - 调用用户自定义函数](#point_rightcall----%E8%B0%83%E7%94%A8%E7%94%A8%E6%88%B7%E8%87%AA%E5%AE%9A%E4%B9%89%E5%87%BD%E6%95%B0)
  - [origin -  帮助命令函数](#origin----%E5%B8%AE%E5%8A%A9%E5%91%BD%E4%BB%A4%E5%87%BD%E6%95%B0)
  - [:point_right:shell 函数](#point_rightshell-%E5%87%BD%E6%95%B0)
  - [:point_right:error 和 warning 函数](#point_righterror-%E5%92%8C-warning-%E5%87%BD%E6%95%B0)
- [:point_right:Makefile 通配符](#point_rightmakefile-%E9%80%9A%E9%85%8D%E7%AC%A6)
- [rocketmq-operator例子](#rocketmq-operator%E4%BE%8B%E5%AD%90)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# :point_right:文件名

make命令默认会在当前目录下按顺序寻找文件名为 `GNUmakefile` 、 `makefile` 和 `Makefile` 的文件。

推荐使用`Makefile`

```bash
# 可以通过 -f 手动指定makefile文件
make -f testfile
```

## 包含其它Makefile

```bash
# include <filenames>...
include foo.make *.mk $(bar)
```

## 工作方式

GNU的make工作时的执行步骤如下：（想来其它的make也是类似）

- 读入所有的Makefile。
- 读入被include的其它Makefile。
- 初始化文件中的变量。
- 推导隐式规则，并分析所有规则。
- 为所有的目标文件创建依赖关系链。
- 根据依赖关系，决定哪些目标要重新生成。
- 执行生成命令。

# make 规则

## :point_right:基本语法

make支持三个通配符： `*` ， `?` 和 `~` 。

```makefile
# targets: prerequisites; command
#    command
#    ...

h:
	echo "h"

hh: h
	echo "hh"

clean:
	rm -f *.go
    rm -f *.java

# 运行
make hh
make clean
```

## :point_right:.PHONY 伪目标 

将 `命令参数` 和本地文件区分

```makefile
# 不管是否有“clean”文件，要运行“clean”这个目标
.PHONY : clean
clean :
    rm *.o temp

# 其他写法
all : prog1 prog2 prog3
.PHONY : all
.PHONY : cleanall cleanobj cleandiff
```

> 推荐每个 step 命令单独写 .PHONY

## :point_right:多目标

```makefile
bigoutput littleoutput : text.g
    generate text.g -$(subst output,,$@) > $@

# 等价于
bigoutput : text.g
    generate text.g -big > bigoutput
littleoutput : text.g
    generate text.g -little > littleoutput
```

## :point_right:显示命令

通常，make会把其要执行的命令行在命令执行前输出到屏幕上。当我们用 `@` 字符在命令行前，那么，这个命令将不被make显示出来

```makefile
h:
	echo "h"
# echo "h"
# h
##########################
h:
	@echo "h"
# h
```

带入make参数 `-n` 或 `--just-print` ，那么其只是显示命令，但不会执行命令。

make参数 `-s` 或 `--silent` 或 `--quiet` 则是全面禁止命令的显示。

## :point_right:执行命令

```makefile
exec:
    cd /home/hchen
    pwd

exec:
    cd /home/hchen; pwd
```

## :point_right:忽略错误

给make加上 `-i` 或是 `--ignore-errors` 参数，那么，Makefile中所有命令都会忽略错误。

```bash
make -i 
```

## :point_right:基本变量

量在声明时需要给予初值，而在使用时，需要给在变量名前加上 `$` 符号

但最好用小括号 `()` 或是大括号 `{}` 把变量给包括起来。

> 如果你要使用真实的 `$` 字符，那么你需要用 `$$` 来表示。

```makefile
objects = program.o foo.o utils.o
program : $(objects)
    cc -o program $(objects)

$(objects) : defs.h

# 变量的赋值
foo = $(bar)
bar = $(ugh)
ugh = Huh?
all:
    echo $(foo)
```

## override 指示符

不希望在命令行指定的变量值替代在Makefile中的原来定义，那么我们可以在Makefile中使用指示符 override 对这个变量进行声明：

```makefile
.PHONY: all
override web = www.zhaixue.cc
all:
    @echo "web = $(web)"
```

## :point_right:环境变量

```makefile
.PHONY:all
CFLAGS = -g
all:
    @echo "CFLAGS = $(CFLAGS)"
    @echo "SHELL = $(SHELL)"
    @echo "MAKE = $(MAKE)"
    @echo "HOSTNAME = $(HOSTNAME)"

# make HOSTNAME=zhaixue.cc
# CFLAGS = -g
# SHELL = /bin/sh
# MAKE = make
# HOSTNAME = zhaixue.cc
```

## 变量内容替换

我们可以替换变量中的共有的部分，其格式是 `$(var:a=b)` 或是 `${var:a=b}` ，其意思是，把变量“var”中所有以“a”字串“结尾”的“a”替换成“b”字串。这里的“结尾”意思是“空格”或是“结束符”。

```makefile
foo := a.o b.o c.o
bar := $(foo:.o=.c)

.PHONY: outputbar
outputbar:
	@echo "$(bar)"

## a.c b.c c.c
```

## :point_right:追加变量值

```bash
objects = main.o foo.o bar.o utils.o
objects += another.o

# 或者
objects = main.o foo.o bar.o utils.o
objects := $(objects) another.o
```

# :point_right:分支

```makefile
bar =
foo = $(bar)
ifdef foo
    frobozz = yes
else
    frobozz = no
endif
```

## if 函数

类似于ifeq关键字，if函数的使用格式如下

```makefile
$(if CONDITION,THEN-PART)
$(if CONDITION,THEN-PART[,ELSE-PART])

.PHONY: all
install_path = $(if $(install__path), $(install__path), /usr/local)
all:
    @echo "install_path = $(install_path)"
```

# :point_right:循环

如果想做一些循环或遍历操作时，可以使用foreach函数：

```makefile
$(foreach VAR,LIST,TEXT)

.PHONY: all
dirs = lcd usb media keyboard
srcs = $(foreach dir, $(dirs), $(wildcard $(dir)/*.c))
objs = $(foreach src, $(srcs), $(subst .c,.o,$(src)))
all:
    @echo "srcs = $(srcs)"
    @echo "objs = $(objs)"
```



# 函数

## :point_right:函数调用

函数调用，很像变量的使用，也是以 `$` 来标识的

```makefile
$(<function> <arguments>)
# ${<function> <arguments>}

bar:= $(subst $(space),$(comma),$(foo))
.PHONT: output
output:
	@echo "$(bar)"
```

## :point_right:自定义函数

```bash
PHONY: all
define func
    @echo "pram1 = $(0)"
    @echo "pram2 = $(1)"
endef
all:
    # 如果想调用用户自定义的函数，则只能使用call函数来间接调用
    $(call func, hello zhaixue.cc)
```

## 字符串处理函数

### :point_right:subst - 标准替换函数

```makefile
# 把字串 <text> 中的 <from> 字符串替换成 <to>
# 函数返回被替换过后的字符串。
$(subst <from>,<to>,<text>)
# 例子
$(subst ee,EE,feet on the street)
```

### patsubst - 正则替换函数

```makefile
$(patsubst PATTERN, REPLACEMENT, TEXT)
.PHONY: all
SRC  = $(wildcard *.c)
OBJ  = $(patsubst %.c, %.o, $(SRC))
all:
    @echo "OBJ = $(OBJ)"
```

### strip - 去掉空格

```makefile
.PHONY: all
STR =     hello a    b   c   
STRIP_STR = $(strip $(STR))
all:
    @echo "STR = $(STR)"
    @echo "STRIP_STR = $(STRIP_STR)"
# hello a b c

# strip函数经常用在条件判断语句的表达式中，去掉多余的空格等因素，确保表达式比较的可靠和健壮。
ifeq ($(strip $(foo)),)
    echo "foo is empty"
endif
```

### findstring- 查找一个字符串

```makefile
$(findstring FIND, IN)

.PHONY: all
STR =     hello a    b   c   
FIND = $(findstring hello, $(STR))
all:
    @echo "STR = $(STR)"
    @echo "FIND = $(FIND)"
```

### filter - 过滤掉一个指定的字符串

```makefile
$(filter PATTERN…,TEXT)

.PHONY: all
FILE = a.c b.h c.s d.cpp   
SRC = $(filter %.c, $(FILE))
all:
    @echo "FILE = $(FILE)"
    @echo "SRC = $(SRC)"
```

### filter-out - 是一个反过滤函数

```makefile
.PHONY: all
FILE = a.c b.h c.s d.cpp   
SRC = $(filter-out %.c, $(FILE))
all:
    @echo "FILE = $(FILE)"
    @echo "SRC = $(SRC)"
```

### sort - 单词排序

```makefile
$(sort LIST)
```

### :point_right:word - 取单词

```bash
$(word N,TEXT)

.PHONY: all
LIST = banana pear apple peach orange 
word1 = $(word 1, $(LIST))
word2 = $(word 2, $(LIST))
word3 = $(word 3, $(LIST))
word4 = $(word 4, $(LIST))
word5 = $(word 5, $(LIST))
word6 = $(word 6, $(LIST))
all:
    @echo "word1 = $(word1)"
    @echo "word2 = $(word2)"
    @echo "word3 = $(word3)"
    @echo "word4 = $(word4)"
    @echo "word5 = $(word5)"
    @echo "word6 = $(word6)"
```

### wordlist - 取子串

```makefile
$(wordlist N, M, TEXT)

.PHONY: all
LIST = banana pear apple peach orange 
sub_list = $(wordlist 1, 3, $(LIST))
all:
    @echo "LIST = $(LIST)"
    @echo "sub_list = $(sub_list)"
```

### words - 统计单词数目

```makefile
$(words TEXT)
.PHONY: all
LIST = banana pear apple peach orange 
all:
    @echo "LIST = $(LIST)"
    @echo "LIST len = $(words $(LIST))

# LIST = banana pear apple peach orange 
# LIST len = 5
```

### firstword - 取首个单词

```makefile
$(firstword NAMES…)
$(word 1,TEXT)

.PHONY: all
LIST = banana pear apple peach orange 
all:
    @echo "LIST = $(LIST)"
    @echo "first word = $(firstword $(LIST))"
```

## 路径文件名处理函数

### :point_right:dir - 取路径名的目录

```makefile
$(dir NAMES…)

.PHONY: all
LIST = /home/wit/banana.c /usr/include/stdio.h
all:
    @echo "LIST = $(LIST)"
    @echo "dir = $(dir $(LIST))"

# LIST = /home/wit/banana.c /usr/include/stdio.h
# dir = /home/wit/ /usr/include/
```

### :point_right:notdir - 取文件名

```makefile
$(notdir  NAMES…)

.PHONY: all
LIST = /home/wit/banana.c /usr/include/stdio.h
all:
    @echo "LIST = $(LIST)"
    @echo "file = $(notdir $(LIST))"
    
# LIST = /home/wit/banana.c /usr/include/stdio.h
# file = banana.c stdio.h
```

### suffix - 取文件名后缀

```makefile
$(suffix NAMES…)

.PHONY: all
LIST = /home/wit/banana.c /usr/include/stdio.h
all:
    @echo "LIST = $(LIST)"
    @echo "suffix = $(suffix $(LIST))"
    
# c h
```

### basename - 取文件名前缀

```makefile
$(basename NAMES…)
```

### :point_right:join - 单词连接

```makefile
$(join LIST1,LIST2)

.PHONY: all
LIST1 = apple banana peach
LIST2 = .c .h .s
LIST  = $(join $(LIST1), $(LIST2))
all:
    @echo "LIST1 = $(LIST1)"
    @echo "LIST2 = $(LIST2)"
    @echo "LIST = $(LIST)"
```

### :point_right:wildcard - 列出所有符号匹配模式的文件

```makefile
$(wildcard PATTERN)
.PHONY: all
LIST  = $(wildcard *.c)
all:
    @echo "LIST = $(LIST)"
```

## :point_right:call  - 调用用户自定义函数

```makefile
$(function arguments)
${function arguments}

PHONY: all
define func
    @echo "pram1 = $(0)"
    @echo "pram2 = $(1)"
endef
all:
    $(call func, hello zhaixue.cc)
```

## origin -  帮助命令函数

函数的作用就是告诉你，你所关注的一个变量是从哪里来的。函数的使用格式为

```makefile
$(origin <variable>)

.PHONY: all
WEB = www.zhaixue.cc
web_type = $(origin WEB)
all:
    @echo "web_type = $(web_type)"
    @echo "cc_type  = $(origin CC)"
    @echo "cmd_type = $(origin CMD)"
```

常见的返回值如下:

- default：变量是一个默认的定义，比如 CC 这个变量
- file：这个变量被定义在[Makefile](https://www.zhaixue.cc/makefile/makefile-intro.html)中
- command line：这个变量是被命令行定义的
- override：这个变量是被override指示符重新定义过的
- automatic：一个命令运行中的自动化变量

## :point_right:shell 函数

makefile 中 运行shell命令

```bash
.PHONY: all
current_path = $(shell pwd)
all:
    @echo "current_path = $(current_path)"
```

## :point_right:error 和 warning 函数

当遇到error函数时，就会给用户一个错误提示信息，并终止make的继续执行：

```makefile
$(error TEXT…)

.PHONY: all
all:
    @echo "make command start..."
    $(error find a error)
    @echo "make command end..."
```

warning函数跟error函数类似，也会给用户提示信息，唯一的区别是：warning函数不会终止make的运行，make会继续运行下去

```bash
$(warning TEXT…)

.PHONY: all
all:
    @echo "make command start..."
    $(warning find a error)
    @echo "make command end..."
```

# :point_right:Makefile 通配符

在Makefile中可以使用的通配符有：* 、? 、 […]。

通配符的使用方法和含义和在shell中一样。比如：*.c 表示当前目录下所有以“.c”结尾的文件。除此之外，Makefile还有经常使用的几个自动变量也可以看做特殊通配符：

- $@：所有目标文件
- $^：目标依赖的所有文件
- $<：第一个依赖文件
- $?：所有更新过的依赖文件

```makefile
.PHONY: h
h:
	@echo "h"

.PHONY: hh
hh: h 
	@echo "$@"
```

# rocketmq-operator例子

```bash
# VERSION defines the project version for the bundle.
# Update this value when you upgrade the version of your project.
# To re-generate a bundle for another specific version without changing the standard setup, you can:
# - use the VERSION as arg of the bundle target (e.g make bundle VERSION=0.0.2)
# - use environment variables to overwrite this value (e.g export VERSION=0.0.2)
VERSION ?= 0.0.1

# CHANNELS define the bundle channels used in the bundle.
# Add a new line here if you would like to change its default config. (E.g CHANNELS = "candidate,fast,stable")
# To re-generate a bundle for other specific channels without changing the standard setup, you can:
# - use the CHANNELS as arg of the bundle target (e.g make bundle CHANNELS=candidate,fast,stable)
# - use environment variables to overwrite this value (e.g export CHANNELS="candidate,fast,stable")
ifneq ($(origin CHANNELS), undefined)
BUNDLE_CHANNELS := --channels=$(CHANNELS)
endif

# DEFAULT_CHANNEL defines the default channel used in the bundle.
# Add a new line here if you would like to change its default config. (E.g DEFAULT_CHANNEL = "stable")
# To re-generate a bundle for any other default channel without changing the default setup, you can:
# - use the DEFAULT_CHANNEL as arg of the bundle target (e.g make bundle DEFAULT_CHANNEL=stable)
# - use environment variables to overwrite this value (e.g export DEFAULT_CHANNEL="stable")
ifneq ($(origin DEFAULT_CHANNEL), undefined)
BUNDLE_DEFAULT_CHANNEL := --default-channel=$(DEFAULT_CHANNEL)
endif
BUNDLE_METADATA_OPTS ?= $(BUNDLE_CHANNELS) $(BUNDLE_DEFAULT_CHANNEL)

# IMAGE_TAG_BASE defines the docker.io namespace and part of the image name for remote images.
# This variable is used to construct full image tags for bundle and catalog images.
#
# For example, running 'make bundle-build bundle-push catalog-build catalog-push' will build and push both
# apache.org/operatorsdk-bundle:$VERSION and apache.org/operatorsdk-catalog:$VERSION.
IMAGE_TAG_BASE ?= apache.org/operatorsdk

# BUNDLE_IMG defines the image:tag used for the bundle.
# You can use it as an arg. (E.g make bundle-build BUNDLE_IMG=<some-registry>/<project-name-bundle>:<tag>)
BUNDLE_IMG ?= $(IMAGE_TAG_BASE)-bundle:v$(VERSION)

# Image URL to use all building/pushing image targets
IMG ?= controller:latest
# ENVTEST_K8S_VERSION refers to the version of kubebuilder assets to be downloaded by envtest binary.
ENVTEST_K8S_VERSION = 1.22

# Get the currently used golang install path (in GOPATH/bin, unless GOBIN is set)
ifeq (,$(shell go env GOBIN))
GOBIN=$(shell go env GOPATH)/bin
else
GOBIN=$(shell go env GOBIN)
endif

# Setting SHELL to bash allows bash commands to be executed by recipes.
# This is a requirement for 'setup-envtest.sh' in the test target.
# Options are set to exit when a recipe line exits non-zero or a piped command fails.
SHELL = /usr/bin/env bash -o pipefail
.SHELLFLAGS = -ec

.PHONY: all
all: build

##@ General

# The help target prints out all targets with their descriptions organized
# beneath their categories. The categories are represented by '##@' and the
# target descriptions by '##'. The awk commands is responsible for reading the
# entire set of makefiles included in this invocation, looking for lines of the
# file as xyz: ## something, and then pretty-format the target and help. Then,
# if there's a line with ##@ something, that gets pretty-printed as a category.
# More info on the usage of ANSI control characters for terminal formatting:
# https://en.wikipedia.org/wiki/ANSI_escape_code#SGR_parameters
# More info on the awk command:
# http://linuxcommand.org/lc3_adv_awk.php

.PHONY: help
help: ## Display this help.
	@awk 'BEGIN {FS = ":.*##"; printf "\nUsage:\n  make \033[36m<target>\033[0m\n"} /^[a-zA-Z_0-9-]+:.*?##/ { printf "  \033[36m%-15s\033[0m %s\n", $$1, $$2 } /^##@/ { printf "\n\033[1m%s\033[0m\n", substr($$0, 5) } ' $(MAKEFILE_LIST)

##@ Development

.PHONY: manifests
manifests: controller-gen ## Generate WebhookConfiguration, ClusterRole and CustomResourceDefinition objects.
	$(CONTROLLER_GEN) rbac:roleName=rocketmq-operator crd:generateEmbeddedObjectMeta=true webhook paths="./..." output:dir=deploy output:crd:artifacts:config=deploy/crds
	head -n 14 deploy/role_binding.yaml > deploy/role.yaml.bak
	cat deploy/role.yaml >> deploy/role.yaml.bak
	rm deploy/role.yaml && mv deploy/role.yaml.bak deploy/role.yaml

.PHONY: generate
generate: controller-gen ## Generate code containing DeepCopy, DeepCopyInto, and DeepCopyObject method implementations.
	$(CONTROLLER_GEN) object:headerFile="hack/boilerplate.go.txt" paths="./..."

.PHONY: fmt
fmt: ## Run go fmt against code.
	go fmt ./...

.PHONY: vet
vet: ## Run go vet against code.
	go vet ./...

.PHONY: test
test: manifests generate fmt vet envtest ## Run tests.
	KUBEBUILDER_ASSETS="$(shell $(ENVTEST) use $(ENVTEST_K8S_VERSION) -p path)" go test ./... -coverprofile cover.out

##@ Build

.PHONY: build
build: generate fmt vet ## Build manager binary.
	go build -o bin/manager main.go

.PHONY: run
run: manifests generate fmt vet ## Run a controller from your host.
	go run ./main.go

.PHONY: docker-build
docker-build: test ## Build docker image with the manager.
	docker build -t ${IMG} .

.PHONY: docker-push
docker-push: ## Push docker image with the manager.
	docker push ${IMG}

##@ Deployment

ifndef ignore-not-found
  ignore-not-found = false
endif

.PHONY: install
install: manifests kustomize ## Install CRDs into the K8s cluster specified in ~/.kube/config.
	kubectl apply -f deploy/crds/rocketmq.apache.org_controllers.yaml
	kubectl create -f deploy/crds/rocketmq.apache.org_brokers.yaml
	kubectl create -f deploy/crds/rocketmq.apache.org_nameservices.yaml
	kubectl create -f deploy/crds/rocketmq.apache.org_consoles.yaml
	kubectl create -f deploy/crds/rocketmq.apache.org_topictransfers.yaml

.PHONY: uninstall
uninstall: manifests kustomize ## Uninstall CRDs from the K8s cluster specified in ~/.kube/config. Call with ignore-not-found=true to ignore resource not found errors during deletion.
	kubectl delete --ignore-not-found=$(ignore-not-found) -f deploy/crds/rocketmq.apache.org_brokers.yaml
	kubectl delete --ignore-not-found=$(ignore-not-found) -f deploy/crds/rocketmq.apache.org_controllers.yaml
	kubectl delete --ignore-not-found=$(ignore-not-found) -f deploy/crds/rocketmq.apache.org_nameservices.yaml
	kubectl delete --ignore-not-found=$(ignore-not-found) -f deploy/crds/rocketmq.apache.org_consoles.yaml
	kubectl delete --ignore-not-found=$(ignore-not-found) -f deploy/crds/rocketmq.apache.org_topictransfers.yaml

.PHONY: deploy
deploy: manifests install ## Deploy controller to the K8s cluster specified in ~/.kube/config.
	kubectl create -f deploy/service_account.yaml
	kubectl create -f deploy/role.yaml
	kubectl create -f deploy/role_binding.yaml
	kubectl create -f deploy/operator.yaml

.PHONY: undeploy
undeploy: uninstall ## Undeploy controller from the K8s cluster specified in ~/.kube/config. Call with ignore-not-found=true to ignore resource not found errors during deletion.
	kubectl delete --ignore-not-found=$(ignore-not-found) -f deploy/service_account.yaml
	kubectl delete --ignore-not-found=$(ignore-not-found) -f deploy/role.yaml
	kubectl delete --ignore-not-found=$(ignore-not-found) -f deploy/role_binding.yaml
	kubectl delete --ignore-not-found=$(ignore-not-found) -f deploy/operator.yaml

CONTROLLER_GEN = $(shell pwd)/bin/controller-gen
.PHONY: controller-gen
controller-gen: ## Download controller-gen locally if necessary.
	$(call go-get-tool,$(CONTROLLER_GEN),sigs.k8s.io/controller-tools/cmd/controller-gen@v0.7.0)

KUSTOMIZE = $(shell pwd)/bin/kustomize
.PHONY: kustomize
kustomize: ## Download kustomize locally if necessary.
	$(call go-get-tool,$(KUSTOMIZE),sigs.k8s.io/kustomize/kustomize/v3@v3.8.7)

ENVTEST = $(shell pwd)/bin/setup-envtest
.PHONY: envtest
envtest: ## Download envtest-setup locally if necessary.
	$(call go-get-tool,$(ENVTEST),sigs.k8s.io/controller-runtime/tools/setup-envtest@latest)

# go-get-tool will 'go get' any package $2 and install it to $1.
PROJECT_DIR := $(shell dirname $(abspath $(lastword $(MAKEFILE_LIST))))
define go-get-tool
@[ -f $(1) ] || { \
set -e ;\
TMP_DIR=$$(mktemp -d) ;\
cd $$TMP_DIR ;\
go mod init tmp ;\
echo "Downloading $(2)" ;\
GOBIN=$(PROJECT_DIR)/bin go get $(2) ;\
rm -rf $$TMP_DIR ;\
}
endef

#.PHONY: bundle
#bundle: manifests kustomize ## Generate bundle manifests and metadata, then validate generated files.
#	operator-sdk generate kustomize manifests -q
#	cd config/manager && $(KUSTOMIZE) edit set image controller=$(IMG)
#	$(KUSTOMIZE) build config/manifests | operator-sdk generate bundle -q --overwrite --version $(VERSION) $(BUNDLE_METADATA_OPTS)
#	operator-sdk bundle validate ./bundle
#
#.PHONY: bundle-build
#bundle-build: ## Build the bundle image.
#	docker build -f bundle.Dockerfile -t $(BUNDLE_IMG) .
#
#.PHONY: bundle-push
#bundle-push: ## Push the bundle image.
#	$(MAKE) docker-push IMG=$(BUNDLE_IMG)

.PHONY: opm
OPM = ./bin/opm
opm: ## Download opm locally if necessary.
ifeq (,$(wildcard $(OPM)))
ifeq (,$(shell which opm 2>/dev/null))
	@{ \
	set -e ;\
	mkdir -p $(dir $(OPM)) ;\
	OS=$(shell go env GOOS) && ARCH=$(shell go env GOARCH) && \
	curl -sSLo $(OPM) https://github.com/operator-framework/operator-registry/releases/download/v1.19.1/$${OS}-$${ARCH}-opm ;\
	chmod +x $(OPM) ;\
	}
else
OPM = $(shell which opm)
endif
endif

# A comma-separated list of bundle images (e.g. make catalog-build BUNDLE_IMGS=example.com/operator-bundle:v0.1.0,example.com/operator-bundle:v0.2.0).
# These images MUST exist in a registry and be pull-able.
BUNDLE_IMGS ?= $(BUNDLE_IMG)

# The image tag given to the resulting catalog image (e.g. make catalog-build CATALOG_IMG=example.com/operator-catalog:v0.2.0).
CATALOG_IMG ?= $(IMAGE_TAG_BASE)-catalog:v$(VERSION)

# Set CATALOG_BASE_IMG to an existing catalog image tag to add $BUNDLE_IMGS to that image.
ifneq ($(origin CATALOG_BASE_IMG), undefined)
FROM_INDEX_OPT := --from-index $(CATALOG_BASE_IMG)
endif

# Build a catalog image by adding bundle images to an empty catalog using the operator package manager tool, 'opm'.
# This recipe invokes 'opm' in 'semver' bundle add mode. For more information on add modes, see:
# https://github.com/operator-framework/community-operators/blob/7f1438c/docs/packaging-operator.md#updating-your-existing-operator
.PHONY: catalog-build
catalog-build: opm ## Build a catalog image.
	$(OPM) index add --container-tool docker --mode semver --tag $(CATALOG_IMG) --bundles $(BUNDLE_IMGS) $(FROM_INDEX_OPT)

# Push the catalog image.
.PHONY: catalog-push
catalog-push: ## Push a catalog image.
	$(MAKE) docker-push IMG=$(CATALOG_IMG)
```

