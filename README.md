#Tadso
#include <windows.h>
#include <stdio.h>
#include<stdlib.h>
#include <string.h>


struct book
{
	char code[20];
	char title[20];
	char author[20];
	int status;
	struct *next;
};

typedef struct book Book;

void Addbook()
{  
	char code[20]={'a'};
    char title[20]={'a'};
	char author[20]={'a'};
	int status;
	FILE *fp;
	printf("请输入:\n");
	printf("code title author status \n");	
	fp=fopen("d://book.txt","a");
	if(fp==NULL)printf("book.txt cannot be opened");
	scanf("%s %s %s %d",&code,&title,&author,&status);
	fprintf(fp,"%s %s %s %d ",code,title,author,status);
	fclose(fp);
	printf("添加成功");
	Sleep(1000);
}



void toxy(int x,int y)   //移动光标
{
	HANDLE hout;
    COORD coord;
    coord.X=x;
    coord.Y=y;
    hout=GetStdHandle(STD_OUTPUT_HANDLE);
    SetConsoleCursorPosition(hout,coord); 
}



void fbook()
{
	char n[1000]={'a'};
	FILE *fp;
	system("cls");
	fp=fopen("d://book.txt","r");
	if(fp==NULL)
	{
		printf("book.txt cannot be opened");
	}
	fscanf(fp,"%s",&n);
	fprintf(fp,"%s",n);
	fclose(fp);
}



int *fun(void)//从文件读取书本信息并创建链表并返回头指针
{
	int i;
	char ch;
	char code[20]={'a'},title[20]={'a'},author[20]={'a'};
	int status;
	Book *head=NULL;
	Book *q=NULL;
	Book *temp;	
	FILE *fp;
	head = (Book *)malloc(sizeof(Book));
	temp=head;
	fp=fopen("d://book.txt","r");
	if(fp==NULL)printf("book.txt cannot be opened");
	ch=fgetc(fp);
	while(ch!=EOF)
	{
		q = (Book *)malloc(sizeof(Book));
		i=0;
		while(ch!=EOF&&ch!=' ')
		{
			code[i]=ch;
			ch=fgetc(fp);	
			i++;
		}
		strcpy(q->code,code);
		memset(code,0,sizeof(code));
		ch=fgetc(fp);
		i=0;
		while(ch!=EOF&&ch!=' ')
		{
			title[i]=ch;
			ch=fgetc(fp);	
			i++;
		}
		strcpy(q->title,title);	
		memset(title,0,sizeof(code));
		ch=fgetc(fp);
		i=0;
		while(ch!=EOF&&ch!=' ')
		{
			author[i]=ch;
			ch=fgetc(fp);
			i++;
		}
		strcpy(q->author,author);
		memset(author,0,sizeof(code));
		ch=fgetc(fp);
		q->status=ch;
		q->next=NULL;
		temp->next=q;
		temp=q;
		ch=fgetc(fp);
		ch=fgetc(fp);
	}
	fclose(fp);
	printf("链表构建完成\n");
	return head;
}



void rbook()
{
	int n;
	char ** ch = (char **)malloc(20 * sizeof (char *));
	int i=1;
	char author[20];
	char book[20];
	int count=0;
	Book *head;
	Book *p;
	Book *temp;
	head=fun();
	temp=head;
	p=head->next;
	system("cls");
	toxy(45,3);
	printf("所有图书信息\n");
	head=fun();
	toxy(30,5);
	printf("序号     书名      作者      数量 \n");
	p=head->next;
	while(head->next!=NULL)
	{
	count=0;
	temp=head;
	p=head;
	p=p->next;
	strcpy(author,p->author);
	strcpy(book,p->title);
	while(p!=NULL)
	{
		if(strcmp(p->title,book)==0)
		{
			count++;
			temp->next=p->next;	
			free(p);
			p=temp->next;
		}
		else
		{
			temp=temp->next;
			p=p->next;
		}
	}
	toxy(30,5+i);
	printf(" %d    %7s     %7s   %4d\n",i,book,author,count);
	ch[i] = (char *)malloc(20 * sizeof (char));
	strcpy(ch[i],book);
	i++;
	}
	system("pause");
	printf("请输入要查看图书的序号\n");
	scanf("%d",&n);
	head=fun();
	p=head;
	p=p->next;
	while(p!=NULL)
	{
		if(strcmp(p->title,ch[n])==0)
		{	
			printf("%s ",p->code);
			printf("%s ",p->title);
			printf("%s %c\n",p->author,p->status);
		}
		p=p->next;
	}
	system("pause");
}


void delbook()
{
	char ch;
	int n;
	Book *head;
	Book *p;
	Book *temp;
	char book[20];
	char code[20];
	FILE *fp;
	head=fun();
	p=head;
	temp=head;
	p=p->next;
	printf("请输入删除类型:\n");
	printf("1:按照书名删除:");
	printf("2:按照编号删除\n");
	scanf("%d",&n);
	if(n==1)
	{
		printf("请输入书名:");
		scanf("%s",&book);
		while(p!=NULL)
		{
			if(strcmp(p->title,book)==0)
			{
				printf("%s\n",p->title);
				temp->next=p->next;
				free(p);
				p=temp->next;
			}
			else
			{
			temp=temp->next;
			p=p->next;
			}
		}
	}
 	if(n==2)
	{
		printf("请输入编号:");
		scanf("%s",code);
		while(p!=NULL)
		{
			if(strcmp(p->code,code)==0)
			{
				temp->next=p->next;
				free(p);
				p=temp->next;
			}
			else
			{
			temp=temp->next;
			p=p->next;
			}
		}
	}
	p=head;
	p=p->next;
	while(p!=NULL)
	{
	printf("%s %s %s %c ",p->code,p->title,p->author,p->status);
	p=p->next;
    }
	system("pause");
	fp=fopen("d://book.txt","w");
	if(fp==NULL)
		printf("book.txt cannot be opened");
	p=head;
	p=p->next;
	while(p!=NULL)
	{
		fprintf(fp,"%s %s %s %c ",p->code,p->title,p->author,p->status);
		p=p->next;
	}
	fclose(fp);
}

void findbook()
{
	int n;
	char book[20];
	char code[20];
	Book *head;
	Book *p;
	Book *temp;
	head=fun();
	printf("1:按书名查找\n");
	printf("2:按编号查找\n");
	scanf("%d",&n);
	if(n==1)
	{			
		printf("请输入要查询的书名:");
		scanf("%s",&book);
		p=head;
		temp=head;
		p=p->next;
		while(p!=NULL)
		{
			if(strcmp(p->title,book)==0)
			{
				printf("%s ",p->code);
				printf("%s",p->title);	
				printf(" %s %c\n",p->author,p->status);
				temp->next=p->next;
				free(p);
				p=temp->next;
			}
			else
			{
				p=p->next;
				temp=temp->next;
			}
		}
		system("pause");		
	}
	if(n==2)
	{			
		printf("请输入要查询的编号:");
		scanf("%s",&code);
		p=head;
		temp=head;
		p=p->next;
		while(p!=NULL)
		{
			if(strcmp(p->code,code)==0)
			{
				printf("%s %s %s %c\n",p->code,p->title,p->author,p->status);
				system("pause");
				break;
			}
			else
			{
				p=p->next;
				temp=temp->next;
			}
		}			
	}

}


void chagenapa()
{
	char name[20];
	char na[20];
	char pass[20];
	char pa[20];
	char ch;
	FILE *fp;
	printf("请输入账号:");
	scanf("%s",&name);
	printf("请输入密码:");
	scanf("%s",&pass);

	fp=fopen("d://NameAdmin.txt","r");
	if(fp==NULL)
		printf("NameAdmin.txt cannot be opened");
	fscanf(fp,"%s",&na);
	fclose(fp);
	
	fp=fopen("d://PassAdmin.txt","r");
	if(fp==NULL)
		printf("NameAdmin.txt cannot be opened");
	fscanf(fp,"%s",&pa);
	fclose(fp);

	if(strcmp(na,name)==0)
	{
		if(strcmp(pass,pa)==0)
		{
			memset(name,0,sizeof(name));
			printf("请输入账号:");
        	scanf("%s",&name);
			memset(pass,0,sizeof(pass));
			printf("请输入密码:");
			scanf("%s",&pass);
			
			fp=fopen("d://NameAdmin.txt","w");
			if(fp==NULL)
				printf("NameAdmin.txt cannot be opened");
			fprintf(fp,"%s",&name);
			fclose(fp);

			fp=fopen("d://PassAdmin.txt","w");
			if(fp==NULL)
				printf("NameAdmin.txt cannot be opened");
			fprintf(fp,"%s",&pass);
			fclose(fp);

			printf("修改密码成功");
			Sleep(1000);
		}
		else
		{
			printf("密码错误");
			Sleep(1000);
                
		}
	}
	else 
	{
		printf("账号错误");
		Sleep(1000);
	}
}

void NameAdmin()
{
	char a[6]={'a'};
	char b[6]={'a'};
	char p[100]={'a'};
	char pass[100]={'a'};
	int i=0;
	int m;
	int login;
	FILE *fp;
	printf("请输入账号:");
	scanf("%s",a);
	fp=fopen("d://NameAdmin.txt","r");
	if(fp==NULL)
	{
		printf("NameAdmin.txt cannot be opened\n");
	}
	fscanf(fp,"%s",&b);
	fclose(fp);
	Sleep(1000);
    if(strcmp(a, b))
	{
		printf("无此账户!\n");
		return NameAdmin();
	}
}



void PassAdmin() 
{
    char pw[50],ch;
    char syspw[20]; // 原始密码
    int i,m = 0;
	FILE *fp;

	fp=fopen("d://PassAdmin.txt","r");
	if(fp==NULL)printf("PassAdmin.txt cannot be opened\n");
	fscanf(fp,"%s",syspw);
	fclose(fp);

	printf("您有三次输入机会\n");
    printf("请输入密码:\n"); 
    while(m < 3)
	{
        i = 0;
        while((ch = _getch()) != '\r') 
		{
            if(ch == '\b' && i > 0)
			{
                printf("\b \b");
                --i;
            }
            else if(ch != '\b') 
			{
                pw[i++] = ch;
                printf("*");
            }
        }
        pw[i] = '\0';
        printf("\n");
        if(strcmp(pw,syspw) != 0)
		{
            printf("密码错误，请重新输入!\n");
            m++;
        }
        else 
		{       
            printf("正在登陆\n");
			break;
        }
    }
	if(m==3)
	{
		printf("连续3次输入错误，退出!\n");
		exit(0);
	}
	Sleep(1000);
}

void NameBorro()
{
	char a[6]={'a'};
	char b[6]={'a'};
	char p[100]={'a'};
	char pass[100]={'a'};
	int i=0;
	int m;
	int login;
	FILE *fp;
	printf("请输入账号:");
	scanf("%s",a);
	fp=fopen("d://NameBorro.txt","r");
	if(fp==NULL)
	{
		printf("NameBorro.txt cannot be opened");
	}
	fscanf(fp,"%s",&b);
	fclose(fp);
	Sleep(1000);
    if(strcmp(a, b))
	{
		printf("无此账户!\n");
		return NameBorro();
	}
}



void PassBorro() 
{
    char pw[50],ch;
    char *syspw = "abc"; // 原始密码
    int i,m = 0;
	printf("您有三次输入机会\n");
    printf("请输入密码:\n"); 
    while(m < 3)
	{
        i = 0;
        while((ch = _getch()) != '\r') 
		{
            if(ch == '\b' && i > 0)
			{
                printf("\b \b");
                --i;
            }
            else if(ch != '\b') 
			{
                pw[i++] = ch;
                printf("*");
            }
        }
        pw[i] = '\0';
        printf("\n");
        if(strcmp(pw,syspw) != 0)
		{
            printf("密码错误，请重新输入!\n");
            m++;
        }
        else 
		{
            printf("正在登陆\n");
			Sleep(1000);
			break;
        }
    }
	if(m==3)
	{
		printf("连续3次输入错误，退出!\n");
		exit(0);
	}
}







void Administrator()
{
   int m; 
   system("cls");
   toxy(45,5);
   printf("欢迎登陆管理员模式");
   toxy(45,10);
   printf("1:查询所有书籍状态");
   toxy(45,12);
   printf("2:添加书籍");
   toxy(45,14);
   printf("3:删除书籍");
   toxy(45,16);  
   printf("4:查找书籍");
   toxy(45,18); 
   printf("5:更改账号及密码");
   toxy(45,20); 
   printf("6:返回上级菜单");
   toxy(0,22);
   printf("请选择:");
   scanf("%d",&m);
   switch(m)
   {  
   case 1: 
	   rbook();
	   return Administrator();
	   break;      
   case 2:
	   Addbook();
	   return Administrator();
	   break;
   case 3:
	   delbook();
	   return Administrator();
	   break; 
   case 4:
	   findbook();
	   return Administrator();
	   break; 
   case 5:
	   chagenapa();
	   return Administrator();
	   break;
   case 6:
	   system("cls");break;
   default:
	   printf("error\n");
	   system("pause");
	   return	Administrator();
	   break;
   }
}


void Borrower()
{
   int m; 
   system("cls");
   toxy(45,5);
   printf("欢迎登陆借阅者模式");
   toxy(45,10);
   printf("1:查看所有书籍状态");
   toxy(45,12);
   printf("2:查找书籍");
   toxy(45,14);
   printf("3:已借图书");
   toxy(45,16);
   printf("4:更改账号及密码");
   toxy(45,18); 
   printf("5:返回上级菜单");
   toxy(0,20);
   printf("请选择:");
   scanf("%d",&m); 
   switch(m)
   {
   case 1:
       rbook();
	   return Borrower();
	   break;
   case 2:
	   findbook();
	   return Borrower();
	   break;
   case 3:
   case 4:
   case 5:
	   system("cls");break;
   default:
	   printf("error\n");
	   system("pause");
	   return	Borrower();
	   break;	
   }
}



void main()
{
	int n;
	toxy(40,5);
	printf("欢迎使用TS图书管理系统!");
	toxy(45,10);
	printf("1:管理员登陆");
	toxy(45,12);
	printf("2:借阅者登陆\n");
	toxy(45,14);
	printf("3:退出系统\n");
	toxy(0,20);
	printf("请选择:");
	scanf("%d",&n);
    switch(n)
	{
	case 1:
		NameAdmin();
		PassAdmin();
		system("cls");
		Administrator();
		return main();
		break;
	case 2:
		NameBorro();
		PassBorro();
		system("cls");
		Borrower();
		return main();
		break;
	case 3:
		printf("成功退出\n");
		exit(0);
	default:
		printf("error\n");
		system("pause");
		system("cls");
		return main();
		break;
	}
}
