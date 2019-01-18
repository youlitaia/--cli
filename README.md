# 基础知识
## 1.selpg
### 定义
selpg 是一个自定义命令行程序，全称select page，即从源（标准输入流或文件）读取指定页数的内容到目的地（标准输出流或给给打印机打印）  

### 命令格式  
```java
selpg [-s startPage] [-e endPage] [-l linePerPage | -f] [-d dest] input_file >output_file 2>error_file
   ```

### 参数
   `-s`：startPage，后面接开始读取的页号   
    `-e`：endPage，后面接结束读取的页号  
    `-l`：后面跟行数，代表多少行分为一页  
    `-f`：该标志无参数，代表按照分页符’\f’ 分页，一般默认72行为一页  
    `-d`：“-dDestination”选项将选定的页直接发送至打印机，“Destination”应该是 lp 命令“-d”选项可接受的打印目的地名称  
   ` input_file`，`output_file 2`，`error_file`：输入文件、输出文件、错误信息文件的名字  
   
(-s和-e是必须的参数，其它为可选参数，-l和-f参数不可能同时出现）  
## 2. flag
flag 是Go 标准库提供的解析命令行参数的包。 使用方式： flag.Type(name, defValue, usage) 其中Type为String, Int, Bool等；并返回一个相应类型的指针。 flag.TypeVar(&flagvar, name, defValue, usage) 将flag绑定到一个变量上。 f lag 自定义flag 只要实现flag.Value接口即可  
## 3. bufio
bufio包实现了有缓冲的I/O。它包装一个io.Reader或io.Writer接口对象，创建另一个也实现了该接口，且同时还提供了缓冲和一些文本I/O的帮助函数的对象。 
## 4. os.exec
exec包执行外部命令。它包装了os.StartProcess函数以便更容易的修正输入和输出，使用管道连接I/O，以及作其它的一些调整。 
# 设计
## main()函数  
```java
func main(){
	var args selpg_args
	get(&args)
	process_args(&args)
	process_input(&args)
}
```
1.func get(args *selpg_args)

初始化，定义结构体各个变量的值。

2.func process_args(args *selpg_args)

检测处理命令行参数是否有错误，如结束页码小于开始页码等。

3.func process_input(args *selpg_args)

根据参数，执行相应逻辑(处理输入，输出)  
## 导入所需包
 ```java
 package main

import (
	"fmt"
	"flag"
	"os"
	"io"
	"bufio"
	"os/exec"
)
```

## 定义结构体
 ```java
 type selpg_args struct
{
	start_page int
	end_page int
	in_filename string
	page_len int
	page_type bool
	print_dest string
}
 ```
# 运行
使用以下命令
```java
go install github.com/github-user/selpg
selpg [-s startPage] [-e endPage] [-l linePerPage | -f] [-d dest] input_file >output_file 2>error_file
```

