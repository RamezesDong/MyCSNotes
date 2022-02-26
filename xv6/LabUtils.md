# LabUtils

----

## 读代码

1. ls.c

ls 的参数可以是一个文件，也可以是一个路径。若无参数则运行`ls(".")`。我在代码中增加了注释，通过学习ls，来写`find.c`。

```c
void ls(char *path)
{
  char buf[512], *p;
  int fd;
  struct dirent de;
  struct stat st;

  if((fd = open(path, 0)) < 0){
    fprintf(2, "ls: cannot open %s\n", path);
    return;
  }

  if(fstat(fd, &st) < 0){
    fprintf(2, "ls: cannot stat %s\n", path);
    close(fd);
    return;
  }

  switch(st.type){
  case T_FILE: //参数是一个文件
    printf("%s %d %d %l\n", fmtname(path), st.type, st.ino, st.size);
    break;

  case T_DIR: //参数是一个路径
    if(strlen(path) + 1 + DIRSIZ + 1 > sizeof buf){
      printf("ls: path too long\n");
      break;
    }
    strcpy(buf, path); //先向buf复制路径
    p = buf+strlen(buf);
    *p++ = '/';
    while(read(fd, &de, sizeof(de)) == sizeof(de)){
        // 读取路径下所有文件和路径
      if(de.inum == 0)
        continue;
      memmove(p, de.name, DIRSIZ);
      p[DIRSIZ] = 0;
      if(stat(buf, &st) < 0){
        printf("ls: cannot stat %s\n", buf);
        continue;
      }
      printf("%s %d %d %d\n", fmtname(buf), st.type, st.ino, st.size);
    }
    break;
  }
  close(fd);
}
```

## 理解题意

1. xrags: 从标准输入读入行，并为每一行运行一个命令，将该行作为命令的参数。
   - 最初，我认为是直接读入命令，然后fork，exec执行命令
   - 但是，本命令不是这样执行的。`cat url-list.txt | xargs wget -c`，如此语句就是读取txt文件中的链接，进行下载。所以，命令执行必须有`readline`的过程。

