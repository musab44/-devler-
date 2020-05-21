# -devler-#include <cstdio>
#include <cstdlib>
#include <ctime>
#include <cmath>
#include <cstring>
FILE *outputf  = fopen ("mine.out","w");
int width=0,length=0,mines=0,map[200][200],hiddenmap[200][200],checked[200][200];
int inputy,inputx,ax,ay,x,y;
void printintro()
{
       while (width>30 || width <9)
       {
             printf("Width (9-30)  : ");
             scanf("%d",&width);
       }
       while (length>24 || length <9)
       {
             printf("Length (9-24) : ");
             scanf("%d",&length);
       }
       while (mines>668 || mines <10)
       {
             printf("Mines (10-668): ");
             scanf("%d",&mines);
       }
       printf("Creating Map ...\n\nMine Map : \n\n");
}
void createmap()
{
       srand(time(0));
       for (int a=1; a<length+1; a++)
             for (int b=1; b<width+1; b++)
             {
                    map[a][b]=0;
                    hiddenmap[a][b] = 'X';
             }
       int temp_width,temp_length;
       for (int i = 1; i <= mines; i++)
       {
             temp_length= rand()%length+1;
             temp_width= rand()%width+1;
             if (map[temp_length][temp_width]!= '*')
                    map[temp_length][temp_width]= '*';
             else i--;
       }
}
void putnumber()
{
       for (int a=1; a<length+1; a++)
             for (int b=1; b<width+1; b++)
             {
                    if (map[a][b]!= '*')
                    {
                           if (map[a-1][b-1] == '*')
                                  map[a][b]++;
                           if (map[a-1][b] == '*')
                                  map[a][b]++;
                           if (map[a-1][b+1] == '*')
                                  map[a][b]++;
                           if (map[a][b-1] == '*')
                                  map[a][b]++;
                           if (map[a][b+1] == '*')
                                  map[a][b]++;
                           if (map[a+1][b-1] == '*')
                                  map[a][b]++;
                           if (map[a+1][b] == '*')
                                  map[a][b]++;
                           if (map[a+1][b+1] == '*')
                                  map[a][b]++;
                    }
             }
}
void printmap()
{
       for (int a=1; a<length+1; a++)
       {
             for (int b=1; b<width+1; b++)
             {
                    if (map[a][b]!= '*')
                    {
                           fprintf(outputf,"%d ",map[a][b]);
                           printf("%d ",map[a][b]);
                    }
                    else
                    {
                          fprintf(outputf,"%c ",map[a][b]);
                           printf("%c ",map[a][b]);
                    }
             }
             fprintf(outputf,"\n");
             printf("\n");
       }
}
void printcmdmap()
{
       printf("   ");
       for (int c=1; c< width+1 ; c++)
             printf("%2d ",c);
       printf("\n");
       for (int a=1; a<length+1; a++)
       {
             printf("%2d  ",a);
             for (int b=1; b<width+1; b++)
             {
                    if (hiddenmap[a][b]== 'X')
                           printf("%c  ",hiddenmap[a][b]);
                    else
                           printf("%d  ",hiddenmap[a][b]);
             }
                    
             printf("\n");
       }
       printf("\n");
}
int checkxy (int k, int l)
{
       if (k > 0 && k < length+1 && l > 0 && l < width+1)
             return 1;
       else return 0;
}
void foo(int p)
{
       switch(p){
             case 1 : ay=y-1 ; ax=x-1; break;
             case 2 : ay=y-1 ; ax=x; break;
             case 3 : ay=y-1 ; ax=x+1; break;
             case 4 : ay=y   ; ax=x-1; break;
             case 5 : ay=y   ; ax=x+1; break;
             case 6 : ay=y+1 ; ax=x-1; break;
             case 7 : ay=y+1 ; ax=x; break;
             case 8 : ay=y+1 ; ax=x+1; break;
       }
}
void checkzeros(int b, int a)
{
       y=b;
       x=a;
       for (int i = 1 ; i < 9 ; i++ )
       {
             foo(i);
             if (checkxy(ay,ax)==1 && checked[ay][ax]== 0)
             {
                    hiddenmap[ay][ax]=map[ay][ax];
                    if (hiddenmap[ay][ax]== 0)
                    {
                           checked[ay][ax]=1;
                           checkzeros(ay,ax);
                    }
             }
       }
       /*if (checkxy((y-1),(x-1))== 1)
       {
             hiddenmap[y-1][x-1]=map[y-1][x-1];
             if (hiddenmap[y-1][x-1]== 0 )
                    checkzeros((y-1),(x-1));
       }
       if (checkxy((y-1),(x))== 1)
       {
             hiddenmap[y-1][x]=map[y-1][x];
             if (hiddenmap[y-1][x]== 0 )
                    checkzeros((y-1),(x));
       }
       if (checkxy((y-1),(x+1))== 1)
       {
             hiddenmap[y-1][x+1]=map[y-1][x+1];
             if (hiddenmap[y-1][x+1]== 0)
                    checkzeros((y-1),(x+1));
       }
       if (checkxy((y),(x-1))== 1)
       {
             hiddenmap[y][x-1]=map[y][x-1];
             if (hiddenmap[y][x-1]== 0)
                    checkzeros((y),(x-1));
      }
       if (checkxy((y),(x+1))== 1)
       {
             hiddenmap[y][x+1]=map[y][x+1];
             if (hiddenmap[y][x+1]== 0)
                    checkzeros((y),(x+1));
       }
       if (checkxy((y+1),(x-1))== 1)
       {
             hiddenmap[y+1][x-1]=map[y+1][x-1];
             if (hiddenmap[y+1][x-1]== 0)
                    checkzeros((y+1),(x-1));
       }
       if (checkxy((y+1),(x))== 1)
       {
             hiddenmap[y+1][x]=map[y+1][x];
             if (hiddenmap[y+1][x]== 0)
                    checkzeros((y+1),(x));
       }
       if (checkxy((y+1),(x+1))== 1)
       {
             hiddenmap[y+1][x+1]=map[y+1][x+1];
             if (hiddenmap[y+1][x+1]== 0)
                    checkzeros((y+1),(x+1));
       }*/
}
void scancheck()
{
       printf("x : ");
       scanf("%d",&inputx);
       printf("y : ");
       scanf("%d",&inputy);
       if (map[inputy][inputx]!= '*' )
       {
             hiddenmap[inputy][inputx] = map[inputy][inputx];
             checked[inputy][inputx]= 1;
             if (hiddenmap[inputy][inputx] == 0)
                    checkzeros(inputy,inputx);
             
             printcmdmap();
             scancheck();
       }
       else
       {
             system("color cf");
             printf("\n OYUN BİTTİ!\n\n");
             printmap();
             printf("\n\nYour mine map saved to mine.out ... \nOpen that file to see your last gameplay.\n\n");
       }
}
void fcall()
{
       printintro();
       createmap();
       putnumber();
       printmap();
       printcmdmap();
       scancheck();
}
int main()
{     
       fcall();
       fclose(outputf);
       return 0;
}

