# 并发通信

在工程上 , 有两种最常见的并发通信模型 : 共享数据和消息 . 

共享数据是指多个并发单元分别保持对同一个数据的引用 , 实现对该数据的共享 . 被共享的数据可能有多种形式 , 比如内存数据块、磁盘文件、网络数据等 . 在实际工程应用中最常见的无疑是内存了 , 也就是常说的共享内存 . 

先看看我们在C语言中通常是怎么处理线程间数据共享的 : 

```c
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>
void *count();
pthread_mutex_t mutex1 = PTHREAD_MUTEX_INITIALIZER;
int counter = 0;
int main()
{
    int rc1, rc2;
    pthread_t thread1, thread2;
    /* 创建线程，每个线程独立执行函数functionC */
    if((rc1 = pthread_create(&thread1, NULL, &count, NULL)))
    {
        printf("Thread creation failed: %d\n", rc1);
    }
    if((rc2 = pthread_create(&thread2, NULL, &count, NULL)))
    {
        printf("Thread creation failed: %d\n", rc2);
    }
    /* 等待所有线程执行完毕 */
    pthread_join( thread1, NULL);
    pthread_join( thread2, NULL);
    exit(0);
}
void *count()
{
    pthread_mutex_lock( &mutex1 );
    counter++;
    printf("Counter value: %d\n",counter);
    pthread_mutex_unlock( &mutex1 );
}
```



