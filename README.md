# Assignmet-3
This is a repository for Assignment 3


#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<time.h>
#include<sys/wait.h>
#include<sys/types.h>
int main(){

int Slength=0,start;
int Msum=0,sum;
int sum1[10]; 
int s=0,Fsum=0;

int Flength=100,end;
int arr[1000];
for(int i=0;i<1000;i++){
		arr[i]= i;
	}

int fd1[2];//for start limit
int fd2[2];//for end
int fd3[2];//for sum value
int childID;

	
 
	if(pipe(fd1)==-1){printf("Pipe 1 failed");return 1;}
	if(pipe(fd2)==-1){printf("Pipe 2 failed");return 1;}
	if(pipe(fd3)==-1){printf("Pipe 3 failed");return 1;}
	
	write(fd1[1],&Slength,sizeof(int));
	write(fd2[1],&Flength,sizeof(int));


      for(int i=0;i<10;i++) 
    { 
		childID=fork();

		if(childID<0){
		printf("Fork Failed");
		return 1;}
		
	        if(childID == 0) 
	        { start=0;end=0;sum=0;
			
	 		read(fd1[0],&start,sizeof(int));
			read(fd2[0],&end,sizeof(int));


		
		for(int j=start;j<end;j++){
			sum=sum+arr[j];
			}
		printf("\n");
		printf("child=");
		printf("%d",sum);
		write(fd1[1], &start, sizeof(int));
		write(fd2[1], &end, sizeof(int));
		write(fd3[1], &sum, sizeof(int));
                exit(0); 
        } 
	else {
		wait(NULL);
		
		read(fd1[0], &Slength, sizeof(int));
		read(fd2[0], &Flength, sizeof(int));
		read(fd3[0], &Msum, sizeof(int));
		sum1[s++]=Msum;
		Slength=Slength+100;
		Flength=Flength+100;
		write(fd1[1], &Slength, sizeof(int));
		write(fd2[1], &Flength, sizeof(int));
		
	}
 }
		for(int i=0;i<10;i++)
		{
			Fsum= Fsum + sum1[i];
		} 
		printf("\n");
		printf("final sum=");
		printf("%d",Fsum);
      
      
      return 0;
} 
