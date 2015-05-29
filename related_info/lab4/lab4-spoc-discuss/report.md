#实验报告
##设计思路
```
对应spoc中两道题。
	1. 增加内核线程。
		kern/proc/proc_init中：增加kernel_thread：init3。
	2. 把进程的生命周期和调度动态执行过程完整地展现出来：
			在wakeup_proc()函数中输出wakeup前后的进程的id与状态。
			在proc_run()中输出正在运行的进程id与状态。
			在proc_run()中的switch_to()前后输出当前进程的信息、下一个进程(通过链表)的信息。
			下一个进程的状态可能为PROC_ZOMBIE或者PROC_RUNNABLE(实际上应该是READY)。
			do_wait()函数：输出child的状态为PROC_ZOMBIE时的信息；
			在do_wait()函数中，用remove_links将消亡的进程移除链表并进行释放资源时，输出对应的DEAD状态。
```
##程序输出 其中打头为myz的语句为自己添加的printf
```
use SLOB allocator
kmalloc_init() succeeded!
myz3 before wake up: id: 1 ; state: PROC_UNINIT
myz4 after wake up: id: 1 ; state: PROC_RUNNABLE
myz3 before wake up: id: 2 ; state: PROC_UNINIT
myz4 after wake up: id: 2 ; state: PROC_RUNNABLE
myz3 before wake up: id: 3 ; state: PROC_UNINIT
myz4 after wake up: id: 3 ; state: PROC_RUNNABLE
proc_init:: Created kernel thread init_main--> pid: 1, name: init1
proc_init:: Created kernel thread init_main--> pid: 2, name: init2
proc_init:: Created kernel thread init_main--> pid: 3, name: init3
++ setup timer interrupts
myz... run id:1 state:PROC_RUNNABLE
myz1... before switch:prev: 0 state PROC_RUNNABLE
myz1... before switch:next: 1 state PROC_RUNNABLE (actrual: READY)
 kernel_thread, pid = 1, name = init1
myz... run id:2 state:PROC_RUNNABLE
myz1... before switch:prev: 1 state PROC_RUNNABLE
myz1... before switch:next: 2 state PROC_RUNNABLE (actrual: READY)
 kernel_thread, pid = 2, name = init2
myz... run id:3 state:PROC_RUNNABLE
myz1... before switch:prev: 2 state PROC_RUNNABLE
myz1... before switch:next: 3 state PROC_RUNNABLE (actrual: READY)
 kernel_thread, pid = 3, name = init3
myz... run id:1 state:PROC_RUNNABLE
myz1... before switch:prev: 3 state PROC_RUNNABLE
myz1... before switch:next: 1 state PROC_RUNNABLE (actrual: READY)
myz2... after switch:prev: 1 state PROC_RUNNABLE
myz2... after switch:next: 2 state PROC_RUNNABLE (actrual: READY)
 kernel_thread, pid = 1, name = init1 , arg  init main1: Hello world!! 
myz... run id:2 state:PROC_RUNNABLE
myz1... before switch:prev: 1 state PROC_RUNNABLE
myz1... before switch:next: 2 state PROC_RUNNABLE (actrual: READY)
myz2... after switch:prev: 2 state PROC_RUNNABLE
myz2... after switch:next: 3 state PROC_RUNNABLE (actrual: READY)
 kernel_thread, pid = 2, name = init2 , arg  init main2: Hello world!! 
myz... run id:3 state:PROC_RUNNABLE
myz1... before switch:prev: 2 state PROC_RUNNABLE
myz1... before switch:next: 3 state PROC_RUNNABLE (actrual: READY)
myz2... after switch:prev: 3 state PROC_RUNNABLE
myz2... after switch:next: 1 state PROC_RUNNABLE (actrual: READY)
 kernel_thread, pid = 3, name = init3 , arg  myz... init main3: Hello world!! 
myz... run id:1 state:PROC_RUNNABLE
myz1... before switch:prev: 3 state PROC_RUNNABLE
myz1... before switch:next: 1 state PROC_RUNNABLE (actrual: READY)
myz2... after switch:prev: 1 state PROC_RUNNABLE
myz2... after switch:next: 2 state PROC_RUNNABLE (actrual: READY)
 kernel_thread, pid = 1, name = init1 ,  en.., Bye, Bye. :)
myz... before do_exit: proc pid 1 will exit, state is PROC_RUNNABLE
 do_exit: proc pid 1 will exit
 do_exit: proc  parent c02ff008
myz... run id:2 state:PROC_RUNNABLE
myz1... before switch:prev: 1 state PROC_ZOMBIE
myz1... before switch:next: 2 state PROC_RUNNABLE (actrual: READY)
myz2... after switch:prev: 2 state PROC_RUNNABLE
myz2... after switch:next: 3 state PROC_RUNNABLE (actrual: READY)
 kernel_thread, pid = 2, name = init2 ,  en.., Bye, Bye. :)
myz... before do_exit: proc pid 2 will exit, state is PROC_RUNNABLE
 do_exit: proc pid 2 will exit
 do_exit: proc  parent c02ff008
myz... run id:3 state:PROC_RUNNABLE
myz1... before switch:prev: 2 state PROC_ZOMBIE
myz1... before switch:next: 3 state PROC_RUNNABLE (actrual: READY)
myz2... after switch:prev: 3 state PROC_RUNNABLE
myz2... after switch:next: 1 state PROC_ZOMBIE (actrual: PROC_ZOMBIE)
 kernel_thread, pid = 3, name = init3 ,  en.., Bye, Bye. :)
myz... before do_exit: proc pid 3 will exit, state is PROC_RUNNABLE
 do_exit: proc pid 3 will exit
 do_exit: proc  parent c02ff008
myz... run id:0 state:PROC_RUNNABLE
myz1... before switch:prev: 3 state PROC_ZOMBIE
myz1... before switch:next: 0 state PROC_RUNNABLE (actrual: READY)
myz2... after switch:prev: 0 state PROC_RUNNABLE
myz2... after switch:next: 1 state PROC_ZOMBIE (actrual: PROC_ZOMBIE)
do_wait: begin
myz...child zombie: id 1 state PROC_ZOMBIE
do_wait: has kid find child  pid1
myz5...remove_links indicate thread dead, id: 1, state: Dead
do_wait: begin
myz...child zombie: id 2 state PROC_ZOMBIE
do_wait: has kid find child  pid2
myz5...remove_links indicate thread dead, id: 2, state: Dead
do_wait: begin
myz...child zombie: id 3 state PROC_ZOMBIE
do_wait: has kid find child  pid3
myz5...remove_links indicate thread dead, id: 3, state: Dead
do_wait: begin
100 ticks
```
