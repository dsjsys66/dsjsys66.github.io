# dsjsys66.github.io
#include<stdio.h>
#include<windows.h>
#include<stdlib.h>
#include<string.h>
typedef struct Node
{
	int ID;// 学号
	char Name[50];// 姓名
	char Sex[10];// 性别
	char Class[50];// 班级
	int shengao;// 宿舍号
	int tizhong;// 成绩
	struct Node* next;
}node;
node list;
int Read_FILE(node* L);
int  Save_FILE(node* L);
void welcome();
void Add(node *L,node e);
void Add_Printf();
void Dele(node*L);
void Delete(node* s);
void Fix(node *L);
void Search(node* L);
node* Search_id(int id, node* L);
void Print(node* L);
void Print_Printf();
void out();
int t=1;
int main()
{
	printf("*************************************\n");
	printf("------请输入1跳转管理员登录界面------\n");
	printf("------请输入2跳转用  户登录界面------\n");
	printf("------请输入3跳转新用户注册界面------\n");
	printf("*************************************\n");
	printf("\n请输入：");
	system("pause");
	int a; scanf("%d",&a);
	char password[7]={"111111"};
	char zhanghao[10],password1[7],password2[7];
	if(a==1)
	{
	printf("请输入管理员账号和密码：\n系统默认登录密码为:111111.\n");
	printf("管理员账号："); scanf("%s",&zhanghao);
	printf("密码（六位）："); scanf("%s",&password1);
	if(strcmp(password1,password)!=0)
	{
		printf("输入错误！"); 
		t=0;
	}
	else
	printf("欢迎使用！！\n");}
	if(a==2)
	{
		printf("请输入账号："); scanf("%s",&zhanghao);
		printf("请输入密码(六位)："); scanf("%s",&password1);
		printf("欢迎使用！！\n");
	 } 
	if(a==3)
	{
	printf("请输入新账号："); scanf("%s",&zhanghao);
	printf("请输入新密码（六位）："); scanf("%s",&password1);
	printf("请再次输入新密码："); scanf("%s",&password2);
	if(strcmp(password1,password2)==0)
		printf("注册成功！欢迎使用！\n");
	else
		{
		printf("注册失败！\n");
		t=0; 
		}	
	}
	int choice = 0;
	Read_FILE(&list);
	while (t)
	{
		welcome();
		scanf("%d", &choice);
		switch (choice)
		{
		case 1:// 增加学生信息
			Add_Printf();
			break;
		case 2:// 删除学生信息
			Dele(&list);
			break;
		case 3:// 修改学生信息
			Fix(&list);
			break;
		case 4:// 查询学生信息
			Search(&list);
			break;
		case 5:// 输出学生信息
			Print(&list);
			break;
		case 6: out();
			break;
		default: printf("你丫审不审题？重新输!!\n");
		}
	}
	return 0;
}
void welcome()
{
	printf("*******************************\n");
	printf("★★★★-----菜 单-----★★★★\n"); 
	printf("1.增加学生信息---2.删除学生信息\n");
	printf("-------------------------------\n");
	printf("3.修改学生信息---4.查询学生信息\n");
	printf("-------------------------------\n");
	printf("5.显示学生信息---6.退 出 系 统 \n");
	printf("*******************************\n");
	printf("请输入菜单编号：");
}
int Read_FILE(node* L)
{
	FILE* pfRead = fopen("student_information.txt", "r");
	node st;
	node* s;
	node* t = L;
	if (pfRead == NULL)
	{
		return 0;
	}
	while (fscanf(pfRead, "%d %s %s %s %d %d", &st.ID, st.Name, st.Sex, st.Class,&st.shengao, &st.tizhong) != EOF)
	{
		s = (node*)malloc(sizeof(node));
		*s = st;
		t->next = s;
		t = s;
		t->next = NULL;
	}
	return 1;
}
int  Save_FILE(node* L)
{
	FILE* pfWrite = fopen("student_information.txt", "w");
	if (pfWrite == NULL)
	{
		return 0;
	}
	node* p = L->next;
	while (p != NULL)
	{
		fprintf(pfWrite, " %d %s %s %s %d %d\n", p->ID, p->Name, p->Sex, p->Class, p->shengao, p->tizhong);
		p = p->next;
	}
	return 1;
}
void Add_Printf()
{
	system("pause");
	int n;
	printf("\n请输入要增加的学生人数：\n");
	scanf("%d",&n);
	for(int i=0;i<n;++i){
	node st;
	printf("学号：");
	scanf("%d", &st.ID);
	printf("姓名：");
	scanf("%s", st.Name);
	printf("性别：");
	scanf("%s", st.Sex);
	printf("班级：");
	scanf("%s", st.Class);
	printf("体重：");
	scanf("%d",&st.shengao);
	printf("身高：");
	scanf("%d", &st.tizhong);
	Add(&list, st);
	}
	printf("录入成功！！\n"); 
}
void Add(node* L, node e)
{
	node* p = L;
	node* s = (node*)malloc(sizeof(node));
	*s = e;
	s->next = p->next;
	p->next = s;
	Save_FILE(L);
}
void Dele(node* L)
{
	system("pause");
	int id;
	node* p;
	printf("请输入要删除的学生的学号：");
	scanf("%d", &id);
	node* st = Search_id(id, L);
	p = st;
	if (st == NULL)
	{
		printf("查无此人！\n");
		return;
	}
	st = st->next;
	printf("所删除信息为：\n");
	printf("________________________________________________________________\n");
	printf("|学号\t|姓名\t|性别\t|班级\t|身高\t|体重\t|\n");
	printf("________________________________________________________________\n");
	printf("%d|%s\t|%s\t|%s\t|%d\t|%d\t|\n", st->ID, st->Name, st->Sex, st->Class, st->shengao, st->tizhong);
	printf("________________________________________________________________\n");
	printf("删除成功！！\n"); 
	Delete(p);
	Save_FILE(L);
}
void Delete(node* s)
{
	node* t = s->next;
	s->next = t->next;
	t->next = NULL;
	free(t);
}
void Fix(node* L)
{
	system("pause");
	int id;
	printf("请输入要修改的学生的学号：");
	scanf("%d", &id);
	node* st = Search_id(id, L);
	if (st == NULL)
	{
		printf("羊村你喜哥都查不出来！！\n");
		return;
	}
	st = st->next;
	int choice = 0;
		printf("________________________________________________________________\n");
		printf("|学号\t|姓名\t|性别\t|班级\t|身高\t|体重\t|\n");
		printf("________________________________________________________________\n");
		printf("%d|%s\t|%s\t|%s\t|%d\t|%d\t|\n", st->ID, st->Name, st->Sex, st->Class, st->shengao, st->tizhong);
		printf("________________________________________________________________\n");

		printf("修改姓名请扣1\n");
		printf("修改性别请扣2\n");
		printf("修改班级请扣3\n");
		printf("修改身高请扣4\n");  
		printf("修改体重请扣5\n");
		printf("嗯哼：");
		scanf("%d", &choice);
		switch (choice)
		{
		case 1:
			printf("请输入姓名：");
			scanf("%s", st->Name);
			break;
		case 2:
			printf("请输入性别：");
			scanf("%s", st->Sex);
			break;
		case 3:
			printf("请输入班级：");
			scanf("%s", st->Class);
			break; 
		case 4:
			printf("请输入身高：");
			scanf("%s",&st->shengao);
			break;
		case 5:
			printf("请输入体重：");
			scanf("%d",&st->tizhong);
			break;
		}
		printf("修改成功！！\n");
	Save_FILE(L);
}
void Search(node* L)
{
	system("pause");
	int id;
	node* st;
		printf("请输入要查询的学号：");
		scanf("%d", &id);
		st = Search_id(id, L);
		if (st == NULL)
		{
			printf("查无此人！\n");
		}
		else
		{
			st = st->next;
			printf("________________________________________________________________\n");
			printf("|学号\t|姓名\t|性别\t|班级\t|身高\t|体重\t|\n");
			printf("________________________________________________________________\n");
			printf("%d|%s\t|%s\t|%s\t|%d\t|%d\t|\n", st->ID, st->Name, st->Sex, st->Class, st->shengao, st->tizhong);
			printf("________________________________________________________________\n");
		}
}
node* Search_id(int id, node* L)
{
	node* p = L;
	
	while (p->next != NULL)
	{
		if (p->next->ID == id)
		{
			return p;
		}
		p = p->next;
	}
	return NULL;
}
void Print(node* L)
{
	system("pause");
	node* p = L->next;
	Print_Printf();
	if (p != NULL)
	{
		while (p != NULL)
		{
			printf("________________________________________________________________\n");
			printf("%d|%s\t|%s\t|%s\t|%d\t|%d\t|\n", p->ID, p->Name, p->Sex, p->Class, p->shengao, p->tizhong);
			printf("________________________________________________________________\n");
			p = p->next;
		}
	}
}
void Print_Printf()
{
	printf("________________________________________________________________\n");
	printf("|学号\t|姓名\t|性别\t|班级\t|身高\t|体重\t|\n");
	printf("________________________________________________________________\n");
}
void out()
{
	system("pause");
	int p;
	printf("你确定？？？？？？？？？？？\n");
	printf("确定退出请按1，返回菜单请按2\n");
	scanf("%d",&p);
	if(p==1)
	{
		t=0;
		printf("再见再见！！\n"); 
	}
	if(p==2) 
	printf("已取消退出\n"); 
 }
