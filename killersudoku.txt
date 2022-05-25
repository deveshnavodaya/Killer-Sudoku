#include <iostream>
using namespace std;
struct cage{
int a[10][2];
int c;
int l;
};
bool Find(int m[9][9], int &row, int &col)
{
    for (row = 0; row < 9; row++)
        for (col = 0; col < 9; col++)
            if (m[row][col] == 0)
                return true;
    return false;
}
bool Row(int m[9][9], int row, int num)
{
    for (int col = 0; col < 9; col++)
        if (m[row][col] == num)
            return true;
    return false;
}
bool Col(int m[9][9], int col, int num)
{
    for (int row = 0; row < 9; row++)
        if (m[row][col] == num)
            return true;
    return false;
}
bool Box(int m[9][9], int StartRow, int StartCol, int num)
{
    for (int row = 0; row < 3; row++)
        for (int col = 0; col < 3; col++)
            if (m[row+StartRow][col+StartCol] == num)
                return true;
    return false;
}
cage where(int x,int y,cage t[],int n){
for(int i=0;i<n;i++){
for(int j=0;j<t[i].l;j++){
//cout<<t[i].a[j][0]<<" "<<t[i].a[j][1]<<endl;
if(t[i].a[j][0]==x&&t[i].a[j][1]==y)
return t[i];
}
}
}
bool killer(int m[9][9],int w,cage s){
    int count=0,b=0;
for(int i=0;i<s.l;i++){
if(m[s.a[i][0]][s.a[i][1]]==0)
count++;
}
for(int i=0;i<s.l;i++){
b+=m[s.a[i][0]][s.a[i][1]];
}
b+=w;
if(count==1)
return (b==s.c);
else
return (b<=s.c);
}
bool isSafe(int m[9][9], int row, int col, int num,int n,cage t[])
{   cage s=where(row,col,t,n);
    return !Row(m, row, num) && !Col(m, col, num) &&
           !Box(m, row - row % 3 , col - col % 3, num)&&killer(m,num,s);
}
bool Solve(int m[9][9],int n,cage t[])
{
    int row, col;
    if (!Find(m, row, col))
       return true;
    for (int num = 1; num <= 9; num++)
    {
        if (isSafe(m, row, col, num,n,t))
        {
            m[row][col] = num;

            if (Solve(m,n,t))
                return true;
            m[row][col] = 0;
        }
    }
    return false;
}
void print(int m[9][9])
{
    for (int row = 0; row < 9; row++)
    {
        for (int col = 0; col < 9; col++)
            cout<<m[row][col]<<"  ";
        cout<<endl;
    }
}
int main()
{
    int m[9][9]={0},n;
    cin>>n;
    cage t[n];
    for(int i=0;i<n;i++){
    cin>>t[i].l>>t[i].c;
    for(int j=0;j<t[i].l;j++){
    cin>>t[i].a[j][0]>>t[i].a[j][1];
    }    }
    if (Solve(m,n,t) == true)
         print(m);
    else
        cout<<"No solution exists"<<endl;
    return 0;
}