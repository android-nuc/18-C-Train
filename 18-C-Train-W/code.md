# codes of 18-C-Train



## 文件

```c
#include "stdio.h"
#include "stdlib.h"
int main()
{
    char ch; //定义一个字符串
    int i = 0;
    ch = getchar();
    while(ch != '\n');
    {
        i = fputc(ch,fp);  // 以字符为单位，写入到text.txt文件
        if(i == -1) 
        {
            puts("字符写入失败！");	
            exit(0);
        }
        ch = getchar();
    }

    int value = fclose(fp);
    if(value != 0)
    {
        puts("文件关闭失败！");
        exit(0);
    }
    puts("文件关闭成功！");
    return 0;
}
```

```c
#include "stdio.h"
#include "stdlib.h"

struct info
{
    short no;
    char name[20];
    char sex[10];
};
int main()
{
	//定义一个指向文件类型的指针 
	FILE *fp;
	//打开一个已有的文件 
	fp = fopen("D:\\Desktop\\text.txt","w");
	if(fp == NULL )
	{
		printf("打开文件失败！\n");
		exit(0);
	}
	printf("打开文件成功！\n");

	
	struct info info_st[3] ={
	    {1,"baoqianyue","men"},
	    {2,"lihao","men"},
	    {3,"wanghao","men"}
	};
	for(int i = 0;i < 3;i ++)
	{
	    fprintf(fp,"No = %d\tname = %-8s\tsex = %-6s\n",info_st[i].no,
	        info_st[i].name,info_st[i].sex);
	}
	int value = fclose(fp);
	if(value != 0)
	{
		puts("文件关闭失败！");
		exit(0);
	}
	puts("文件关闭成功！");
	return 0;
}
```