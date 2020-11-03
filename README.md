# -
C语言
//学生成绩管理系统3.0
/************************
** 文件名:实验七
** 创建人:周哲萱
** 日 期:2020/11/3
** 修改人:
** 日 期:
** 描 述:学生成绩管理系统
** 版 本:3.0
*************************/
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<malloc.h>
#define Max 30
#define Maxofsubject 6
void Input(int,int, int*, char [30][20],int**,int*);
void List(int, int, int*, char [30][20],int** );
void Sort1                 (int,int, int(*ptr)(int a,int b),  int *,char [30][20],int **,int*);
void Sort2                 (int,int, int(*ptr)(int a,int b),  int *,char [30][20],int **,int*);
void Sort3                 (int,int, int(*ptr)(int a,int b),  int *,char [30][20],int **,int*);
int Search                 (int,int, int(*ptr)(int a,int b),  int *,char [30][20],int **,int*);
void Sortindic             (int,int,int*,char cName[30][20],int**,int*);
void StaticAnalysis        (int, int,int *,int ** );
void Searchbyname          (int, int,int *,int(*ptr)(int a,int b),char cName[30][20],int **,int* );
int arrange(int a,int b);
int inputcontrol(int*, int );
void SumandAver1(int,int **);
void SumandAver2(int,int, int **);
/*************************************************/
int main()
{
    printf("Number:200320728\nsubject No.6 - program No.1\n\n");
    int n,m;//人数、科目数
    int iChoice=1;//选项编号
    int(*ptr)(int a,int b)=&arrange;//函数指针
    /*********************************************/
    printf("请输入学生人数以及科目数量:\n");
    scanf("%d %d",&n,&m);//读入学生人数
    while(n>Max||m>Maxofsubject)
    {
        printf("输入数据有误,请重新输入:");
        fflush(stdin);//清空缓冲区
        scanf("%d %d",&n,&m);
    }
    /*********************************************/
    int *  iStuID = (int*)malloc(n*(sizeof(int)));//储存学号
    int ** iScore = (int**)malloc(n*(sizeof(int*)));//储存成绩
    int * Sumscore = (int*)malloc(n*(sizeof(int)));//储存总分
    for(int i=0; i<m; i++)
    {
        *(iScore+i) = malloc(sizeof(int)*n);
    }
    char cName[n][20];
    printf("1. Input record\n2. List record\n3. Calculate total and average score of every course\n4. Calculate total and average score of every student\n5. Sort in descending order by total score of every student\n6. Sort in ascending order by total score of every student\n7. Sort in ascending order by StudentID\n8. Sort in dictionary order by name\n9. Search by StudentID\n10.Search by name\n11.Statistic analysis for every course\n0. Exit");
    /**********************************************/
    int con = 0;
    int *control = &iChoice;
    while(iChoice!=0)
    {
        printf("\nPlease enter your choice:");
        fflush(stdin);
        scanf("%d",&iChoice);
        switch(inputcontrol(control,con))
        {
        case 1:
            Input(n,m,iStuID,cName,iScore,Sumscore);
            con=1;
            break;
        case 2:
            List (n,m,iStuID,cName,iScore);
            break;
        case 3:
            SumandAver2(n,m,iScore);
            break;
        case 4:
            SumandAver1(n,iScore);
            break;
        case 5:
            Sort1(n,m, ptr,iStuID,cName,iScore,Sumscore );
            break;
        case 6:
            Sort2(n,m, ptr,iStuID,cName,iScore,Sumscore );
            break;
        case 7:
            Sort3(n, m, ptr,iStuID,cName,iScore,Sumscore );
            break;
        case 8:
            Sortindic(n,m,iStuID,cName,iScore,Sumscore);
            break;
        case 9:
            Search(n,m,ptr,iStuID,cName,iScore,Sumscore);
            break;
        case 10:
            Searchbyname(n,m,iStuID,ptr,cName,iScore,Sumscore);
            break;
        case 11:
            StaticAnalysis(n,m,iStuID,iScore);
            break;
        case 0:
            exit(0);
        default:
            printf("Wrong command.Please input again!");
            fflush(stdin);
            break;
        }
    }
    return 0;
}
/*
** 函数名:Input
** 输 入: int n, int* istuID, int** iScore,char cName[30][20]
** n---学生人数
** istuID---储存学号
** iScore---储存成绩
** cName----储存姓名
** 输 出:
** 功能描述: 将学号、姓名以及成绩读入数组
** 全局变量: 无
** 调用模块: main
** 作 者:周哲萱
** 日 期:2020/11/3
** 修 改:
** 日 期:
** 版本
*/
void Input(int n,int m, int* istuID, char cName[n][20], int** iScore,int * Sumscore)
{
    for(int i=0; i<n; i++) //i为循环变量遍历数组
    {
        //int ret,reta;//检查输入
        /*********************************/
        //ret=0;
        fflush(stdin);
        printf("输入ID:");
        scanf("%d",&istuID[i]);
        fflush(stdin);
        printf("输入姓名:");
        scanf("%s",cName[i]);
        fflush(stdin);
        printf("请输入各科成绩:");
        int sum=0;
        for(int j=0; j<m; j++)
        {

            scanf("%d",&iScore[i][j]);
            sum+=iScore[i][j];
            while(iScore[i][j]>100||iScore[i][j]<0)
            {
                printf("输入错误，请重新输入:");
                fflush(stdin);
                scanf("%d",&iScore[i][j]);
            }
        }
        Sumscore[i]=sum;
        /**********************************/
        /* for(int i1=0; i1<i; i1++)
         {
             if(istuID[i1]==istuID[i])
             {
                 printf("Repeated ID,please choose cover[1] or cancel[2]\n\t[1]\t[2]\t");//检查重复学号
             do
             {     scanf("%d",&reta);
                 switch(reta)
                 {
                 case 1:
                     iScore[i1]=iScore[i];
                     i=i-1;
                     break;
                 case 2:
                     i=i-1;
                     break;
                 default:
                     printf("Invalid input!Please input again:");
                     reta=0;
                     break;
                 }
            }while(reta==0);
             }
         }*/

    }
}
/*
** 函数名:List
** 输 入: int n, int* istuID, int** iScore,char cName[30][20]
** n---学生人数
** istuID---储存学号
** iScore---储存各科成绩
** 输 出: istuID
**        iScore
** 功能描述: 将学号以及成绩按顺序输出
** 全局变量: 无
** 调用模块: main
** 作 者:周哲萱
** 日 期:2020/10/27
** 修 改:
** 日 期:
** 版本
*/
void List(int n,int m,int* istuID, char cName[n][20],int** iScore)
{
    printf("\t学号\t姓名\t\t各科成绩\t\t总分\t\n");
    int sum;
    for(int i=0; i<n; i++)
    {
        printf("\t%d\t%s",istuID[i],cName[i]);
        sum=0;
        for(int j=0; j<m; j++)
        {
            printf("  %d  ",iScore[i][j]);
            sum+=iScore[i][j];
        }
        printf("\t\t%d",sum);
        printf("\n");
    }
}
/*
** 函数名:SumandAver
** 输 入: int n,int** iScore
** n---学生人数
** iScore---储存成绩
** 输 出: iAver 成绩平均值
          iSum  成绩总和
** 功能描述: 将学生成绩之和及平均值输出
** 全局变量: 无
** 调用模块: main
** 作 者:周哲萱
** 日 期:2020/10/27
** 修 改:
** 日 期:
** 版本
*/
void SumandAver1(int n, int **iScore)
{
    int iSum=0;//总分
    int iAver=0;//平均分
    for(int i=0; i<n; i++)
    {
        for(int j=0; j<n; j++)
        {
            iSum+=iScore[i][j];
        }
    }
    iAver=iSum/n;
    printf("所有总分为：%d\n",iSum);
    printf("每个学生平均分为：%d",iAver);
}


void SumandAver2(int n,int m, int **iScore)
{
    int iSum=0;//总分
    int course=0;
    for(int j=0; j<n; j++)
    {
        iSum=0;
        for(int i=0; i<n; i++)
        {
            iSum+=iScore[i][j];
        }
        course++;
        printf("课程%d的总分为：%d,平均分为：%4.2f",course,iSum,(float)(iSum/n));
    }
}
/*
** 函数名:Sort1
** 输 入: int n, int* istuID, int* iScore,char cName[30][20]
** n---学生人数
** istuID---储存学号
** iScore---储存成绩
** 输 出: istuID
**        iScore
** 功能描述: 将学号以及成绩按成绩由高到低排序后输出
** 全局变量: 无
** 调用模块: main
** 作 者:周哲萱
** 日 期:2020/10/27
** 修 改:
** 日 期:
** 版本
*/

void Sort1(int n,int m,int(*ptr)(int a,int b), int * iStuID,char cName[30][20],int ** iScore,int*Sumscore)
{
    int x,y,z,z1,max;//排序
    char l[20]= {0};
    for(x=0; x<n; x++)
    {
        max=x;
        for(y=x+1; y<n; y++)
        {
            if(!(*ptr)(Sumscore[max],Sumscore[y]))
            {
                max=y;
            }
        }
        if(max!=x)
        {
            for(int j=0; j<m; j++)
            {
                z=iScore[max][j];
                iScore[max][j]=iScore[x][j];
                iScore[x][j]=z;
            }
            z1=Sumscore[x];
            Sumscore[x]=Sumscore[max];
            Sumscore[max]=z1;
            z=iStuID[x];
            iStuID[x]=iStuID[max];
            iStuID[max]=z;
            strcpy(l,cName[x]);
            strcpy(cName[x],cName[max]);
            strcpy(cName[max],l);
        }

    }
    printf("成绩由高到低排序为:\n");
    printf("\t学号\t姓名\t总分\t\n");
    for(int T=0; T<n; T++)
    {
        printf("\t%d\t%s\t%d\n",iStuID[T],cName[T],Sumscore[T]);
    }
}
/*
** 函数名:Sort2
** 输 入: int n, int* istuID, int* iScore,char cName[30][20]
** n---学生人数
** istuID---储存学号
** iScore---储存成绩
** 输 出: istuID
**        iScore
** 功能描述: 将学号以及成绩按成绩由低到高排序后输出
** 全局变量: 无
** 调用模块: main
** 作 者:周哲萱
** 日 期:2020/11/3
** 修 改:
** 日 期:
** 版本
*/
void Sort2(int n,int m, int(*ptr)(int a,int b), int * iStuID,char cName[30][20],int ** iScore,int*Sumscore )
{
    int x,y,z,z1,t,min;//排序
    char l[20]= {0};
    for(x=0; x<n; x++)
    {
        min=x;
        for(y=x+1; y<n; y++)
        {
            if((*ptr)(Sumscore[min],Sumscore[y]))
            {
                min=y;
            }
        }
        if(min!=x)
        {
            for(int j=0; j<m; j++)
            {
                z=iScore[min][j];
                iScore[min][j]=iScore[x][j];
                iScore[x][j]=z;
            }
            z1=Sumscore[min];
            z=iStuID[min];
            Sumscore[min]=Sumscore[x];
            iStuID[min]=iStuID[x];
            Sumscore[x]=z1;
            iStuID[x]=z;
            strcpy(l,cName[min]);
            strcpy(cName[min],cName[x]);
            strcpy(cName[x],l);
        }
    }
    printf("成绩由低到高排序为:\n");
    printf("\t学号\t姓名\t总分\t\n");
    for(t=0; t<n; t++)
    {

        printf("\t%d\t%s\t%d\n",iStuID[t],cName[t],Sumscore[t]);
    }
}

/*
** 函数名:Sort3
** 输 入: int n, int* istuID, int* iScore,char cName[30][20]
** n---学生人数
** istuID---储存学号
** iScore---储存成绩
** cName----储存姓名
** 输 出: istuID
**        iScore
** 功能描述: 将学号以及成绩按学号由低到高排序后输出
** 全局变量: 无
** 调用模块: main
** 作 者:周哲萱
** 日 期:2020/11/3
** 修 改:
** 日 期:
** 版本
*/

void Sort3(int n,int m,int(*ptr)(int a,int b),  int * iStuID,char cName[30][20],int ** iScore,int * Sumscore )
{
    int x1,y,z,z1,min;//排序算法
    char l[20]= {0};
    for(x1=0; x1<n; x1++)
    {
        min=x1;
        for(y=x1+1; y<n; y++)
        {
            if((*ptr)(iStuID[min],iStuID[y]))
            {
                min=y;
            }
        }
        if(min!=x1)
        {
            for(int j=0; j<m; j++)
            {
                z=iScore[min][j];
                iScore[min][j]=iScore[x1][j];
                iScore[x1][j]=z;
            }
            z1=Sumscore[min];
            Sumscore[min]=Sumscore[x1];
            Sumscore[x1]=z1;
            z1=iStuID[min];
            iStuID[min]=iStuID[x1];//交换x与min对应的姓名学号
            iStuID[x1]=z1;
            strcpy(l,cName[min]);
            strcpy(cName[min],cName[x1]);
            strcpy(cName[x1],l);
        }
    }
    printf("学号由低到高排序为:\n");
    printf("\t学号\t姓名\t成绩\t\n");
    for(int i=0; i<n; i++)
    {
        printf("\t%d\t%s",iStuID[i],cName[i]);
        for(int j=0; j<m; j++)
        {
            printf("\t%d\t",iScore[i][j]);
        }
        printf("\t\t%d",Sumscore[i]);
        printf("\n");
    }
}

/*
** 函数名:Search
** 输 入: n,stu[]
** n---学生人数
** 输 入: int n, int* istuID,char cName[30][20], int* iScore
** n---学生人数
** istuID---储存学号
** iScore---储存成绩
** cName----储存姓名
** 输 出: iScore
** 功能描述: 按学号搜索对应学生成绩
** 全局变量: 无
** 调用模块: main
** 作 者:周哲萱
** 日 期:2020/11/3
** 修 改:
** 日 期:
** 版本
*/
int Search(int n,int m,int(*ptr)(int a,int b), int * iStuID,char cName[30][20],int ** iScore,int* Sumscore )
{
    int ID;
    printf("Please input ID:");
    scanf("%d",&ID);
    //二分查找
    int first=0,last=n-1;
    /***************************
    *排序
    ***************************/
    int x1,y,z,z1,min;//排序算法
    char l[20]= {0};
    for(x1=0; x1<n; x1++)
    {
        min=x1;
        for(y=x1+1; y<n; y++)
        {
            if((*ptr)(iStuID[min],iStuID[y]))
            {
                min=y;
            }
        }
        if(min!=x1)
        {
            for(int j=0; j<m; j++)
            {
                z=iScore[min][j];
                iScore[min][j]=iScore[x1][j];
                iScore[x1][j]=z;
            }
            z1=Sumscore[min];
            Sumscore[min]=Sumscore[x1];
            Sumscore[x1]=z1;
            z1=iStuID[min];
            iStuID[min]=iStuID[x1];//交换x与min对应的姓名学号
            iStuID[x1]=z1;
            strcpy(l,cName[min]);
            strcpy(cName[min],cName[x1]);
            strcpy(cName[x1],l);
        }
    }
    /********************************
    ****查找
    *****************************/
    int mid=(first+last)/2;
    while(first<=last&&iStuID[mid]!=ID)
    {
        if(iStuID[mid]>ID)
        {
            last=mid-1;
        }
        else if(iStuID[mid]<ID)
        {
            first=mid+1;
        }
        mid=(first+last)/2;
    }
    if(iStuID[mid]==ID)
    {
        printf("姓名：%s",cName[mid]);
        for(int j=0; j<m; j++)
        {
            printf("\t\t%d\t",iScore[mid][j]);
        }
        /***************************
        *排序
        ***************************/
        int x,y,z,z1,max;//排序
        char l[20]= {0};
        for(x=0; x<n; x++)
        {
            max=x;
            for(y=x+1; y<n; y++)
            {
                if(!(*ptr)(Sumscore[max],Sumscore[y]))
                {
                    max=y;
                }
            }
            if(max!=x)
            {
                for(int j=0; j<m; j++)
                {
                    z=iScore[min][j];
                    iScore[min][j]=iScore[x1][j];
                    iScore[x1][j]=z;
                }
                z1=Sumscore[min];
                Sumscore[min]=Sumscore[x1];
                Sumscore[x1]=z1;
                z=iStuID[x];
                iStuID[x]=iStuID[max];
                iStuID[max]=z;
                strcpy(l,cName[x]);
                strcpy(cName[x],cName[max]);
                strcpy(cName[max],l);
            }

        }
        for(int ret=0; ret<n; ret++)
        {
            if(ID==iStuID[ret])
            {
                printf("排名为：%d",ret+1);
                break;
            }
        }
    }
    return 0;
}

/*
** 函数名:StaticAnalysis
** 输 入: n， iStuID,cName, iScore
** n---学生人数
** istuID---储存学号
** iScore---储存成绩
** cName----储存姓名
** 输 出: 不同成绩人数r1--r5及其对应比例
** 功能描述: 按优秀（90—100分）、良好（80—89分）、中等（70—79分）、及格（60一69分）、不及格
(0—59分)5个类别，统计每个类别的人数以及所占的百分比
** 全局变量: 无
** 调用模块: main
** 作 者:周哲萱
** 日 期:2020/11/3
** 修 改:
** 日 期:
** 版本
*/

void StaticAnalysis(int n,int m, int * iStuID,int ** iScore)
{

    for(int i=0; i<m; i++)
    {
        int r1=0,r2=0,r3=0,r4=0,r5=0;//统计人数
        for(int r=0; r<n; r++)
        {
            if(iScore[r][i]<=100&&iScore[r][i]>=90)
            {
                r1++;
            }
            else if(iScore[r][i]<90&&iScore[r][i]>=80)
            {
                r2++;
            }
            else if(iScore[r][i]<80&&iScore[r][i]>=70)
            {
                r3++;
            }
            else if(iScore[r][i]<70&&iScore[r][i]>=60)
            {
                r4++;
            }
            else
            {
                r5++;
            }
        }
        printf("科目%d",i+1);
        printf("优秀人数为：%d  良好人数为：%d  中等人数为：%d  及格人数为：%d  不及格人数为：%d\n",r1,r2,r3,r4,r5);
        printf("优秀比例：%3.1f  良好比例：%3.1f  中等比例：%3.1f  及格比例：：%3.1f  不及格比例：%3.1f\n",(float)r1/n,(float)r2/n,(float)r3/n,(float)r4/n,(float)r5/n);
    }


}

/*void record(int n,struct student stu[])
{
    printf("Input record:\n");
    for(int i=0; i<n; i++)
    {
        scanf("%d %d",&stu[i].studentID,&stu[i].score);
    }
    for(int j=0; j<n; j++)
    {
        printf("studentID:%d score:%d\n",(stu[j].studentID),(stu[j].score));
    }
}
*/


/*
** 函数名:Sortindic
** 输 入: n， iStuID,cName, iScore
** n---学生人数
** istuID---储存学号
** iScore---储存成绩
** cName----储存姓名
** 输 出:
** 功能描述:
** 全局变量: 按姓名字典顺序对学生信息进行排序
** 调用模块: main
** 作 者:周哲萱
** 日 期:2020/11/3
** 修 改:
** 日 期:
** 版本
*/

void Sortindic(int n,int m,int* iStuID,char cName[30][20],int** iScore,int* Sumscore)
{
    int x1,y,z,z1,min;//排序算法
    char l[20]= {0};
    for(x1=0; x1<n; x1++)
    {
        min=x1;
        for(y=x1+1; y<n; y++)
        {
            if(strcmp(cName[x1],cName[y])>0)
            {
                min=y;
            }
        }
        if(min!=x1)
        {
            for(int j=0; j<m; j++)
            {
                z=iScore[min][j];
                iScore[min][j]=iScore[x1][j];
                iScore[x1][j]=z;
            }
            z1=Sumscore[min];
            Sumscore[min]=Sumscore[x1];
            Sumscore[x1]=z1;
            z1=iStuID[min];
            iStuID[min]=iStuID[x1];//交换x与min对应的姓名学号
            iStuID[x1]=z1;
            strcpy(l,cName[min]);
            strcpy(cName[min],cName[x1]);
            strcpy(cName[x1],l);
        }
    }
    printf("按姓名字典顺序排序为:\n");
    printf("\t学号\t姓名\t成绩\t总分\t\n");
    for(int t=0; t<n; t++)
    {

        printf("\t%d\t%s",iStuID[t],cName[t]);
        for(int j=0; j<m; j++)
        {
            printf("\t%d\t",iScore[t][j]);
        }
        printf(" %d ",Sumscore[t]);
        printf("\n");
    }
}


/*
** 函数名:Searchbyname
** 输 入: n,stu[]
** n---学生人数
** 输 入: int n, int* istuID, int* iScore,char cName[30][20]
** n---学生人数
** istuID---储存学号
** iScore---储存成绩
** 输 出: iScore
** 功能描述: 按学号搜索对应学生成绩
** 全局变量: 无
** 调用模块: main
** 作 者:周哲萱
** 日 期:2020/11/3
** 修 改:
** 日 期:
** 版本
*/
void Searchbyname(int n,int m, int * iStuID,int(*ptr)(int a,int b),char cName[30][20],int ** iScore,int*Sumscore )
{
    int q=0;//循环变量
    char name[20];
    printf("Please input Name:");
in5:
    scanf("%s",name);//读入ID
    while (q<n)
    {
        if(strcmp(name,cName[q])==0)
        {
            printf("各科成绩：\n");
            for(int t=0; t<m; t++)
            {

                printf("  %d  ",iScore[q][t]);

            }//输出对应成绩
            printf("  总分为：%d",Sumscore[q]);
            int x,y,z,z1,max;//排序
            char l[20]= {0};
            for(x=0; x<n; x++)
            {
                max=x;
                for(y=x+1; y<n; y++)
                {
                    if(!(*ptr)(Sumscore[max],Sumscore[y]))
                    {
                        max=y;
                    }
                }
                if(max!=x)
                {
                    for(int j=0; j<m; j++)
                    {
                        z=iScore[max][j];
                        iScore[max][j]=iScore[x][j];
                        iScore[x][j]=z;
                    }
                    z1=Sumscore[max];
                    Sumscore[max]=Sumscore[x];
                    Sumscore[x]=z1;
                    z=iStuID[x];
                    iStuID[x]=iStuID[max];
                    iStuID[max]=z;
                    strcpy(l,cName[x]);
                    strcpy(cName[x],cName[max]);
                    strcpy(cName[max],l);
                }

            }
            for(int ret=0; ret<n; ret++)
            {
                if(strcmp(cName[ret],name)==0)
                {
                    printf("排名为：%d",ret+1);
                    break;
                }
            }

            q=0;
            break;
        }
        q++;
    }
    if (q==n)
    {
        printf(" Wrong input, Please input another Name:");//防止输入无效号码
        fflush(stdin);
        q=0;
        goto in5;
    }
}

/*
** 函数名:arrange
** 输 入: a,b两整数
** 输 出: true（1） 或 false（0）
** 功能描述: 比较两整数大小并返回1或0
** 全局变量: 无
** 调用模块: Sort2,3
** 作 者:周哲萱
** 日 期:2020/10/31
** 修 改:
** 日 期:
** 版本
*/
int arrange(int a,int b)
{
    if(a>b)
    {
        return 1;
    }
    else
    {
        return 0;
    }
}
/*
** 函数名:arrange
** 输 入: a,b两整数
** 输 出: true（1） 或 false（0）
** 功能描述: 比较两整数大小并返回1或0
** 全局变量: 无
** 调用模块: Sort2,3
** 作 者:周哲萱
** 日 期:2020/10/31
** 修 改:
** 日 期:
** 版本
*/
int inputcontrol(int* n,int con)
{
    while(con!=1&&(*n)!=1)
    {
        printf("No data , please input!\n");
        printf("Please input your choice:");
        scanf("%d",n);
        if((*n)==1)
        {
            return  *n;
        }
    }
    if(con==1)
    {
        return *n;
    }
}

