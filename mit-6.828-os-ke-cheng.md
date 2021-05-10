# MIT 6.828 OS课程

### 环境配置

参考：[https://pdos.csail.mit.edu/6.828/2020/tools.html](https://pdos.csail.mit.edu/6.828/2020/tools.html)

这里我使用的是ubuntu 20.04

### Util Lab作业

作业描述：[https://pdos.csail.mit.edu/6.828/2020/labs/util.html](https://pdos.csail.mit.edu/6.828/2020/labs/util.html)

#### 启动xv6

下载课程代码：

```bash
git clone git://g.csail.mit.edu/xv6-labs-2020 
cd xv6-labs-2020
git checkout util # 切换到util分支
```

运行方法：

```bash
make qemu # 推出系统的方法：ctrl + a, 然后再按下x
```

#### 实现sleep功能

在`user` 目录下创建`sleep.c` 文件，实现代码如下：

```cpp
#include "kernel/types.h"
#include "kernel/stat.h"
#include "user/user.h"

int
main(int argc, char *argv[])
{
    if (argc != 2) {
        fprintf(2, "Usage: sleep seconds\n");
        exit(1);
    }
    fprintf(2, "sleeping %d seconds\n", atoi(argv[1]));
    sleep(atoi(argv[1]));
    exit(0);
}
```

然后在`Makefile` 文件的`UPROG` （第17行）部分加入`$U/_sleep\`然后执行make qemu，进入终端后输入sleep 5即可验证该功能。

同时也可以使用项目中的自动化测试脚本进行验证：

```text
./grade-lab-util sleep
```



