#实验报告
##设计思路
```
对应两个spoc：
	1. 增加一个用户进程。
		在init_main函数中增加1个int pid2 = kernel_thread(user_main, NULL, 0);
	2. 把进程的生命周期和调度动态执行过程完整地展现出来：
		2.1 从创建后的PROC_UNINIT到唤醒后的PROC_RUNNABLE(实际上应该是READY)：
			在wakeup_proc()函数中输出wakeup前后的进程的id与状态。
		2.2 显示正在运行(run)的线程信息：
			在proc_run()中输出正在运行的进程id与状态。
		2.3 do_wait()
			输出child的状态为PROC_ZOMBIE时的信息/输出转为PROC_SLEEPING的进程的转换前后信息。
		2.4 do_yield()
			显示yield进程的信息；
		2.5 do_exit()
			显示虚拟地址映射、页表、内存空间的释放；
			显示子进程退出唤醒父进程；
		2.6 do_kill()
			显示kill掉的进程的id。
```

##输出结果
从结果中看到有各种状态、进程切换、(内核态和用户态)堆栈切换、do_wait()、     
do_yield()、do_exit()、do_kill()、释放虚拟地址映射、页表、内存空间的输出。
```
use SLOB allocator
check_slab() success
kmalloc_init() succeeded!
check_vma_struct() succeeded!
page fault at 0x00000100: K/W [no page found].
check_pgfault() succeeded!
check_vmm() succeeded.
myz3 before wake up: id: 1 ; state: PROC_UNINIT
myz4 after wake up: id: 1 ; state: PROC_RUNNABLE
ide 0:      10000(sectors), 'QEMU HARDDISK'.
ide 1:     262144(sectors), 'QEMU HARDDISK'.
SWAP: manager = fifo swap manager
BEGIN check_swap: count 31830, total 31830
setup Page Table for vaddr 0X1000, so alloc a page
setup Page Table vaddr 0~4MB OVER!
set up init env for check_swap begin!
page fault at 0x00001000: K/W [no page found].
page fault at 0x00002000: K/W [no page found].
page fault at 0x00003000: K/W [no page found].
page fault at 0x00004000: K/W [no page found].
set up init env for check_swap over!
write Virt Page c in fifo_check_swap
write Virt Page a in fifo_check_swap
write Virt Page d in fifo_check_swap
write Virt Page b in fifo_check_swap
write Virt Page e in fifo_check_swap
page fault at 0x00005000: K/W [no page found].
swap_out: i 0, store page in vaddr 0x1000 to disk swap entry 2
write Virt Page b in fifo_check_swap
write Virt Page a in fifo_check_swap
page fault at 0x00001000: K/W [no page found].
do pgfault: ptep c03a8004, pte 200
swap_out: i 0, store page in vaddr 0x2000 to disk swap entry 3
swap_in: load disk swap entry 2 with swap_page in vadr 0x1000
write Virt Page b in fifo_check_swap
page fault at 0x00002000: K/W [no page found].
do pgfault: ptep c03a8008, pte 300
swap_out: i 0, store page in vaddr 0x3000 to disk swap entry 4
swap_in: load disk swap entry 3 with swap_page in vadr 0x2000
write Virt Page c in fifo_check_swap
page fault at 0x00003000: K/W [no page found].
do pgfault: ptep c03a800c, pte 400
swap_out: i 0, store page in vaddr 0x4000 to disk swap entry 5
swap_in: load disk swap entry 4 with swap_page in vadr 0x3000
write Virt Page d in fifo_check_swap
page fault at 0x00004000: K/W [no page found].
do pgfault: ptep c03a8010, pte 500
swap_out: i 0, store page in vaddr 0x5000 to disk swap entry 6
swap_in: load disk swap entry 5 with swap_page in vadr 0x4000
count is 5, total is 5
check_swap() succeeded!
++ setup timer interrupts
myz... run id:1 state:PROC_RUNNABLE
myz1... before switch:prev: 0 state PROC_RUNNABLE
myz1... before switch:next: 1 state PROC_RUNNABLE (actrual: READY)
myz... create user process original
myz3 before wake up: id: 2 ; state: PROC_UNINIT
myz4 after wake up: id: 2 ; state: PROC_RUNNABLE
myz... create user process A
myz3 before wake up: id: 3 ; state: PROC_UNINIT
myz4 after wake up: id: 3 ; state: PROC_RUNNABLE
myz...before sleep: id 1 state PROC_SLEEPING
myz... run id:3 state:PROC_RUNNABLE
myz1... before switch:prev: 1 state PROC_SLEEPING
myz1... before switch:next: 3 state PROC_RUNNABLE (actrual: READY)
kernel_execve: pid = 3, name = "exit".
I am the parent. Forking the child...
myz3 before wake up: id: 4 ; state: PROC_UNINIT
myz4 after wake up: id: 4 ; state: PROC_RUNNABLE
I am parent, fork a child pid 4
I am the parent, waiting now..
myz...before sleep: id 3 state PROC_SLEEPING
myz... run id:2 state:PROC_RUNNABLE
myz1... before switch:prev: 3 state PROC_SLEEPING
myz1... before switch:next: 2 state PROC_RUNNABLE (actrual: READY)
kernel_execve: pid = 2, name = "exit".
I am the parent. Forking the child...
myz3 before wake up: id: 5 ; state: PROC_UNINIT
myz4 after wake up: id: 5 ; state: PROC_RUNNABLE
I am parent, fork a child pid 5
I am the parent, waiting now..
myz...before sleep: id 2 state PROC_SLEEPING
myz... run id:5 state:PROC_RUNNABLE
myz1... before switch:prev: 2 state PROC_SLEEPING
myz1... before switch:next: 5 state PROC_RUNNABLE (actrual: READY)
I am the child.
myz... process 5 do_yield
myz... run id:4 state:PROC_RUNNABLE
myz1... before switch:prev: 5 state PROC_RUNNABLE
myz1... before switch:next: 4 state PROC_RUNNABLE (actrual: READY)
I am the child.
myz... process 4 do_yield
myz... run id:5 state:PROC_RUNNABLE
myz1... before switch:prev: 4 state PROC_RUNNABLE
myz1... before switch:next: 5 state PROC_RUNNABLE (actrual: READY)
myz2... after switch:prev: 5 state PROC_RUNNABLE
myz2... after switch:next: 4 state PROC_RUNNABLE (actrual: READY)
myz7... before switch process 5 kernel ? 0
myz7... after switch process 4 kernel ? 0
myz... process 5 do_yield
myz... run id:4 state:PROC_RUNNABLE
myz1... before switch:prev: 5 state PROC_RUNNABLE
myz1... before switch:next: 4 state PROC_RUNNABLE (actrual: READY)
myz2... after switch:prev: 4 state PROC_RUNNABLE
myz2... after switch:next: 5 state PROC_RUNNABLE (actrual: READY)
myz7... before switch process 4 kernel ? 0
myz7... after switch process 5 kernel ? 0
myz... process 4 do_yield
myz... run id:5 state:PROC_RUNNABLE
myz1... before switch:prev: 4 state PROC_RUNNABLE
myz1... before switch:next: 5 state PROC_RUNNABLE (actrual: READY)
myz2... after switch:prev: 5 state PROC_RUNNABLE
myz2... after switch:next: 4 state PROC_RUNNABLE (actrual: READY)
myz7... before switch process 5 kernel ? 0
myz7... after switch process 4 kernel ? 0
myz... process 5 do_yield
myz... run id:4 state:PROC_RUNNABLE
myz1... before switch:prev: 5 state PROC_RUNNABLE
myz1... before switch:next: 4 state PROC_RUNNABLE (actrual: READY)
myz2... after switch:prev: 4 state PROC_RUNNABLE
myz2... after switch:next: 5 state PROC_RUNNABLE (actrual: READY)
myz7... before switch process 4 kernel ? 0
myz7... after switch process 5 kernel ? 0
myz... process 4 do_yield
myz... run id:5 state:PROC_RUNNABLE
myz1... before switch:prev: 4 state PROC_RUNNABLE
myz1... before switch:next: 5 state PROC_RUNNABLE (actrual: READY)
myz2... after switch:prev: 5 state PROC_RUNNABLE
myz2... after switch:next: 4 state PROC_RUNNABLE (actrual: READY)
myz7... before switch process 5 kernel ? 0
myz7... after switch process 4 kernel ? 0
myz... process 5 do_yield
myz... run id:4 state:PROC_RUNNABLE
myz1... before switch:prev: 5 state PROC_RUNNABLE
myz1... before switch:next: 4 state PROC_RUNNABLE (actrual: READY)
myz2... after switch:prev: 4 state PROC_RUNNABLE
myz2... after switch:next: 5 state PROC_RUNNABLE (actrual: READY)
myz7... before switch process 4 kernel ? 0
myz7... after switch process 5 kernel ? 0
myz... process 4 do_yield
myz... run id:5 state:PROC_RUNNABLE
myz1... before switch:prev: 4 state PROC_RUNNABLE
myz1... before switch:next: 5 state PROC_RUNNABLE (actrual: READY)
myz2... after switch:prev: 5 state PROC_RUNNABLE
myz2... after switch:next: 4 state PROC_RUNNABLE (actrual: READY)
myz7... before switch process 5 kernel ? 0
myz7... after switch process 4 kernel ? 0
myz... process 5 do_yield
myz... run id:4 state:PROC_RUNNABLE
myz1... before switch:prev: 5 state PROC_RUNNABLE
myz1... before switch:next: 4 state PROC_RUNNABLE (actrual: READY)
myz2... after switch:prev: 4 state PROC_RUNNABLE
myz2... after switch:next: 5 state PROC_RUNNABLE (actrual: READY)
myz7... before switch process 4 kernel ? 0
myz7... after switch process 5 kernel ? 0
myz... process 4 do_yield
myz... run id:5 state:PROC_RUNNABLE
myz1... before switch:prev: 4 state PROC_RUNNABLE
myz1... before switch:next: 5 state PROC_RUNNABLE (actrual: READY)
myz2... after switch:prev: 5 state PROC_RUNNABLE
myz2... after switch:next: 4 state PROC_RUNNABLE (actrual: READY)
myz7... before switch process 5 kernel ? 0
myz7... after switch process 4 kernel ? 0
myz... process 5 do_yield
myz... run id:4 state:PROC_RUNNABLE
myz1... before switch:prev: 5 state PROC_RUNNABLE
myz1... before switch:next: 4 state PROC_RUNNABLE (actrual: READY)
myz2... after switch:prev: 4 state PROC_RUNNABLE
myz2... after switch:next: 5 state PROC_RUNNABLE (actrual: READY)
myz7... before switch process 4 kernel ? 0
myz7... after switch process 5 kernel ? 0
myz... process 4 do_yield
myz... run id:5 state:PROC_RUNNABLE
myz1... before switch:prev: 4 state PROC_RUNNABLE
myz1... before switch:next: 5 state PROC_RUNNABLE (actrual: READY)
myz2... after switch:prev: 5 state PROC_RUNNABLE
myz2... after switch:next: 4 state PROC_RUNNABLE (actrual: READY)
myz7... before switch process 5 kernel ? 0
myz7... after switch process 4 kernel ? 0
myz... process 5 do_yield
myz... run id:4 state:PROC_RUNNABLE
myz1... before switch:prev: 5 state PROC_RUNNABLE
myz1... before switch:next: 4 state PROC_RUNNABLE (actrual: READY)
myz2... after switch:prev: 4 state PROC_RUNNABLE
myz2... after switch:next: 5 state PROC_RUNNABLE (actrual: READY)
myz7... before switch process 4 kernel ? 0
myz7... after switch process 5 kernel ? 0
myz... process 4 do_yield
myz... run id:5 state:PROC_RUNNABLE
myz1... before switch:prev: 4 state PROC_RUNNABLE
myz1... before switch:next: 5 state PROC_RUNNABLE (actrual: READY)
myz2... after switch:prev: 5 state PROC_RUNNABLE
myz2... after switch:next: 4 state PROC_RUNNABLE (actrual: READY)
myz7... before switch process 5 kernel ? 0
myz7... after switch process 4 kernel ? 0
myz... before do_exit: proc pid 5 will exit, state is PROC_RUNNABLE
myz8... proc 5 exit_mmap
myz8... proc 5 put_pgdir
myz8... proc 5 mm_destroy
myz... change to parent: pid 2 state PROC_SLEEPING
myz3 before wake up: id: 2 ; state: PROC_SLEEPING
myz4 after wake up: id: 2 ; state: PROC_RUNNABLE
myz... run id:4 state:PROC_RUNNABLE
myz1... before switch:prev: 5 state PROC_ZOMBIE
myz1... before switch:next: 4 state PROC_RUNNABLE (actrual: READY)
myz2... after switch:prev: 4 state PROC_RUNNABLE
myz2... after switch:next: 5 state PROC_ZOMBIE (actrual: PROC_ZOMBIE)
myz7... before switch process 4 kernel ? 0
myz7... after switch process 5 kernel ? 0
myz... before do_exit: proc pid 4 will exit, state is PROC_RUNNABLE
myz8... proc 4 exit_mmap
myz8... proc 4 put_pgdir
myz8... proc 4 mm_destroy
myz... change to parent: pid 3 state PROC_SLEEPING
myz3 before wake up: id: 3 ; state: PROC_SLEEPING
myz4 after wake up: id: 3 ; state: PROC_RUNNABLE
myz... run id:3 state:PROC_RUNNABLE
myz1... before switch:prev: 4 state PROC_ZOMBIE
myz1... before switch:next: 3 state PROC_RUNNABLE (actrual: READY)
myz2... after switch:prev: 3 state PROC_RUNNABLE
myz2... after switch:next: 2 state PROC_RUNNABLE (actrual: READY)
myz7... before switch process 3 kernel ? 0
myz7... after switch process 2 kernel ? 1
myz...child zombie: id 4 state PROC_ZOMBIE
myz5...remove_links indicate thread dead, id: 4, state: Dead
waitpid 4 ok.
exit pass.
myz... before do_exit: proc pid 3 will exit, state is PROC_RUNNABLE
myz8... proc 3 exit_mmap
myz8... proc 3 put_pgdir
myz8... proc 3 mm_destroy
myz... change to parent: pid 1 state PROC_SLEEPING
myz3 before wake up: id: 1 ; state: PROC_SLEEPING
myz4 after wake up: id: 1 ; state: PROC_RUNNABLE
myz... run id:2 state:PROC_RUNNABLE
myz1... before switch:prev: 3 state PROC_ZOMBIE
myz1... before switch:next: 2 state PROC_RUNNABLE (actrual: READY)
myz2... after switch:prev: 2 state PROC_RUNNABLE
myz2... after switch:next: 5 state PROC_ZOMBIE (actrual: PROC_ZOMBIE)
myz7... before switch process 2 kernel ? 0
myz7... after switch process 5 kernel ? 0
myz...child zombie: id 5 state PROC_ZOMBIE
myz5...remove_links indicate thread dead, id: 5, state: Dead
waitpid 5 ok.
exit pass.
myz... before do_exit: proc pid 2 will exit, state is PROC_RUNNABLE
myz8... proc 2 exit_mmap
myz8... proc 2 put_pgdir
myz8... proc 2 mm_destroy
myz... run id:1 state:PROC_RUNNABLE
myz1... before switch:prev: 2 state PROC_ZOMBIE
myz1... before switch:next: 1 state PROC_RUNNABLE (actrual: READY)
myz2... after switch:prev: 1 state PROC_RUNNABLE
myz2... after switch:next: 3 state PROC_ZOMBIE (actrual: PROC_ZOMBIE)
myz7... before switch process 1 kernel ? 1
myz7... after switch process 3 kernel ? 1
myz5...remove_links indicate thread dead, id: 3, state: Dead
myz5...remove_links indicate thread dead, id: 2, state: Dead
all user-mode processes have quit.
init check memory pass.
kernel panic at kern/process/proc.c:491:
    initproc exit.
```
