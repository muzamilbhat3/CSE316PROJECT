# CSE316PROJECT
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<stdlib.h>
int n,c1=-1,c2=-1,c3=-1;
int i,j;
struct node2
{
int priority,arrival_time,burst_time;
int process_id,ready,process_counter;
}queue1,queue2,queue3,processArray[100];
struct node2 *process;
 struct node2 *q1;
  struct node2 *q2;
   struct node2 *q3;
void processsort(struct node2 sptr[],int n)
{
 	for(i=0;i<n;i++)
 	{
 		for(j=0;j<n-1;j++)
 		{
 			if(sptr[j].arrival_time>sptr[j+1].arrival_time)
 			{
 				struct node2 temp=sptr[j];
 				sptr[j]=sptr[j+1];
 				sptr[j+1]=temp;
			 }
		 }
	 }
}
void processassign(struct node2 process[],int n)
{
	for(i=0;i<n;i++)
	{
		if(((process+i)->priority)<=4)
		{
			c1++;
			*(q1+c1)=*(process+i);
			q1->process_counter=q1->process_counter+1;
		}
	else if(((process+i)->priority)>4 && ((process+i)->priority)<=8)
		{
			c2++;
			*(q2+c2)=*(process+i);
		    q2->process_counter=q2->process_counter+1;
		}
		else if(((process+i)->priority)>8 && ((process+i)->priority)<=12)
		{
			c3++;
			*(q3+c3)=*(process+i);
			q3->process_counter=q3->process_counter+1;
		}
		else
		{
			printf("Highest priority range\n");
		}
	}
}
void Roundrobin(struct node2 spp[],int x)
 {
 	printf("\tProcess_id\t\tRemaining time\t\tTime quantam given\t\tRemaining time after time quantum\n");
 	while(x>0)
 	{
 		for(i=0;i<x;i++)
 	{
 			if((spp+i)->burst_time>4)
 			{
		   		printf("\t%d\t\t\t%d\t\t\t%d\t\t\t\t%d\n",(spp+i)->process_id,(spp+i)->burst_time,4,(spp+i)->burst_time-4);
		   		(spp+i)->burst_time=(spp+i)->burst_time-4;
	    	}
	   		else
	   		{
	   			printf("\t%d\t\t\t%d\t\t\t%d\t\t\t\t%d\n",(spp+i)->process_id,(spp+i)->burst_time,(spp+i)->burst_time,0);
	   			printf("Process %d completed\n",(spp+i)->process_id);
	   			for(j=i-1;j<x;j++)
	   			{
	   				*(spp+j)=*(spp+(j+1));
				   }
				   i--;
	   			x=x-1;
	   		}
 }
}
}
void priority_schedule(struct node2 sptr[],int x)
{
	int total_time=0;
	while(x>0)
	{
		
		for(i=0;i<x;i++)
		{
			//for(j=0;j<x;j++)
		//	{
				if((sptr+i)->arrival_time=(sptr+(i+1))->arrival_time && (sptr+i)->priority<=(sptr+(i+1))->priority)
				{
					printf("\n\t\tProcess %d running\n",(sptr+i)->process_id);
					for(int c=0;c<=(sptr+i)->burst_time;c++)
					{
						printf("%d\t",total_time);
						total_time+=1;
					}
					printf("\t\tProcess %d completed\n",(sptr+i)->process_id);
					for(j=i-1;j<x;j++)
	   				{
	   					*(sptr+j)=*(sptr+(j+1));
				   	}
					i--;
					x=x-1;
				}
				else if((sptr+i)->arrival_time<(sptr+(i+1))->arrival_time && (sptr+i)->priority>(sptr+(i+1))->priority)
				{
					printf("\t\tProcess %d is running\n",(sptr+i)->process_id);
					for(int c=1;c<=(sptr+(i+1))->arrival_time-(sptr+i)->arrival_time;c++)
					{
						printf("%d",total_time);
						(sptr+i)->burst_time-=1;
						total_time+=1;
					}
					printf("Process %d interrupted by process %d\n",(sptr+i)->process_id,(sptr+(i+1))->process_id);
					printf("\t\tProcss %d is running ",(sptr+(i+1))->process_id);
					for(int c=1;c<=(sptr+(j+1))->burst_time;c++)
					{
						printf("%d",total_time);
						(sptr+i)->burst_time-=1;
						total_time+=1;
					}
					printf("\t\tProcss %d is completed ",(sptr+(i+1))->process_id);
					i--;
					x=x-1;
				}
				else
				{
					printf("revisiting queue\n ");
				}
			}
		}
	}
main()
{
printf("Enter number of processes\n");
scanf("%d",&n);
process=(struct node2*)calloc(n,sizeof(processArray));
q1=(struct node2*)calloc(10,sizeof(queue1));
q2=(struct node2*)calloc(10,sizeof(queue2));
q3=(struct node2 *)calloc(10,sizeof(queue3));
for(i=0;i<n;i++)
{
printf("Enter unique process Id\n");
scanf("%d",&(process+i)->process_id);
printf("Enter Arrival time\n");
scanf("%d",&(process+i)->arrival_time);
printf("Enter Burst time \n");
scanf("%d",&(process+i)->burst_time);
printf("Enter priority of process\n");
scanf("%d",&(process+i)->priority);
}
processsort(process,n);
q1->process_counter=0;
q2->process_counter=0;
q3->process_counter=0;
processassign(process,n);	
printf("The processes after sorting them to three queues are as\n");
printf("QueueNo\tProcess_id\tProcess_Priority\tProcess_arrival_time\tBurst_time\n");

	if(q1->process_counter>0)
	{
	for(i=0;i<q1->process_counter;i++)
	{
		printf("%d\t\t%d\t\t%d\t\t%d\t\t\t%d\n",1,(q1+i)->process_id,(q1+i)->priority,(q1+i)->arrival_time,(q1+i)->burst_time);
    }
    }
   	if(q2->process_counter>0)
	{
	for(i=0;i<q2->process_counter;i++)
	{
		printf("%d\t\t%d\t\t%d\t\t%d\t\t\t%d\n",2,(q2+i)->process_id,(q2+i)->priority,(q2+i)->arrival_time,(q2+i)->burst_time);
    }
    }
    if(q3->process_counter>0)
	{
	for(i=0;i<q3->process_counter;i++)
	{
		printf("%d\t\t%d\t\t%d\t\t%d\t\t\t%d\n",3,(q3+i)->process_id,(q3+i)->priority,(q3+i)->arrival_time,(q3+i)->burst_time);
    }
    }
printf("applying Round robin for queue 1 which contains %d processes\n",q1->process_counter);
Roundrobin(q1,q1->process_counter);
printf("applying Priority Scheduling for queue 2 which contains %d processes\n",q2->process_counter);
priority_schedule(q2,q2->process_counter);
}
