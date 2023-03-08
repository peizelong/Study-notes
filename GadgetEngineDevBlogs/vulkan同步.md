#### Fence

同步渲染队列和CPU之间的同步

signaled unsignaled两种状态

vkQueueSubmit所有指令完成后传入的Fence会变成signaled

vkWaitForFences一直等待Fence变成signaled状态

#### Semaphore

signaled unsignaled两种状态

GPU内同步
