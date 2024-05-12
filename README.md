#include <stdio.h>
#include <stdlib.h>

// 定义进程的结构体
typedef struct {
    int pid;
    int burstTime;
    int arrivalTime;
    int waitingTime;
    int completionTime;
    int turnaroundTime;
} Process;

// 比较函数，用于qsort
int compare(const void *a, const void *b) {
    const Process *processA = (const Process *)a;
    const Process *processB = (const Process *)b;
    // 如果到达时间不同，则按到达时间排序
    if (processA->arrivalTime != processB->arrivalTime) {
        return processA->arrivalTime - processB->arrivalTime;
    }
    // 如果到达时间相同，则按执行时间排序
    return processA->burstTime - processB->burstTime;
}

// SJF调度算法实现
void sjfScheduling(Process processes[], int numProcesses) {
    // 使用qsort对进程数组进行排序
    qsort(processs, numProcesses, sizeof(Process), compare);

    int currentTime = 0;
    int totalWaitingTime = 0;

    // 初始化第一个进程
    currentTime = processes[0].arrivalTime;
    printf("PID\tArrival Time\tBurst Time\tWaiting Time\tCompletion Time\n");
    printf("P%d\t%d\t\t%d\t\t%d\t\t%d\n",
           processes[0].pid, processes[0].arrivalTime, processes[0].burstTime,
           processes[0].waitingTime, processes[0].burstTime + processes[0].arrivalTime);

    // 遍历进程数组进行SJF调度
    for (int i = 1; i < numProcesses; i++) {
        // 计算当前进程的等待时间和完成时间
        processes[i].waitingTime = currentTime - processes[i].arrivalTime;
        if (processes[i].waitingTime < 0) {
            processes[i].waitingTime = 0;
        }
        processes[i].completionTime = processes[i].waitingTime + processes[i].burstTime;
        currentTime = processes[i].completionTime;

        // 更新总等待时间
        totalWaitingTime += processes[i].waitingTime;

        // 打印进程信息
        printf("P%d\t%d\t\t%d\t\t%d\t\t%d\n",
               processes[i].pid, processes[i].arrivalTime, processes[i].burstTime,
               processes[i].waitingTime, processes[i].completionTime);
    }

    // 计算平均等待时间
    double averageWaitingTime = (double)totalWaitingTime / numProcesses;
    printf("Average Waiting Time: %.2f\n", averageWaitingTime);
}

int main() {
    // 定义5个进程
    Process processes[] = {
        {1, 10, 1},
        {2, 20, 2},
        {3, 30, 3},
        {4, 5, 4},
        {5, 15, 5}
    };
    int numProcesses = sizeof(processes) / sizeof(Process);

    // 执行SJF调度
    sjfScheduling(processes, numProcesses);

    return 0
}
