- Cores v/s Virtual CPUs | Physical CPUs v/s Logical CPUs

[Hyper-threading](https://www.wikiwand.com/en/Hyper-threading)

[How to - CPU info on linux and mac](http://www.noktec.be/archives/1325)

[Check if Hyper-threading is supported and enabled](https://unix.stackexchange.com/questions/33450/checking-if-hyperthreading-is-enabled-or-not)

- SRAM v/s DRAM

[https://www.wikiwand.com/en/Static_random-access_memory](https://www.wikiwand.com/en/Static_random-access_memory)

- L1 cache is divided into L1d(for data) and L1i(for instructions)

[https://www.wikiwand.com/en/CPU_cache](https://www.wikiwand.com/en/CPU_cache)

- `pstree` starts with `systemd` and not `init`

[https://askubuntu.com/questions/844031/why-doesnt-pstree-command-show-init-in-ubuntu-16-04-lts](https://askubuntu.com/questions/844031/why-doesnt-pstree-command-show-init-in-ubuntu-16-04-lts)

[https://www.tecmint.com/systemd-replaces-init-in-linux/](https://www.tecmint.com/systemd-replaces-init-in-linux/)

- Process with pid 0

[https://unix.stackexchange.com/questions/83322/which-process-has-pid-0](https://unix.stackexchange.com/questions/83322/which-process-has-pid-0)

[sched](http://man7.org/linux/man-pages/man7/sched.7.html)

- PCI (Peripheral Component Interconnect)

[https://www.techwalla.com/articles/what-is-a-pci-device](https://www.techwalla.com/articles/what-is-a-pci-device)

[Memory Controller is a PCI device](https://www.wikiwand.com/en/Memory_controller)

- [Zombie Process](https://www.wikiwand.com/en/Zombie_process)

- [High Memory v/s Low Memory](https://unix.stackexchange.com/questions/4929/what-are-high-memory-and-low-memory-on-linux)

- VmSize and VmRSS

[http://careers.directi.com/display/tu/Understanding+and+optimizing+Memory+utilization](http://careers.directi.com/display/tu/Understanding+and+optimizing+Memory+utilization)

[Is VmRSS from /proc/pid/status a good indicator of memory usage? - Reddit](https://www.reddit.com/r/linux/comments/1xmsrv/is_vmrss_from_procpidstatus_a_good_indicator_of/)


# Lab 4

### How to implement syscalls in xv6

- This is my take on how syscall works in xv6 and how you should implement one. All these are hypothesis from the experiments I did:
  - The function exposed to the user is in `user.h` (say `my_syscall`)
  - `usys.S` tells the OS which function calls to make into a system call, so this has to be the same as the function exposed to the user (`SYSCALL(my_syscall)`)
  - When compiling, the OS looks in `syscall.h` for getting the syscall number for it. It looks for the value of `SYS_my_syscall` which is `#define`d. The `SYS` appended is probably for convention.
  - In `syscall.c`, this number is mapped to a function which will be used to extract arguments to the syscall from the stack and perform checks on them. We can call this function anything (say `getargs_my_syscall`). If we followed convention, we would've named it `sys_my_syscall` but you know I like breaking rules.
  - We'll define the function in a separate file `sysfile.c`, so we'll `extern` the function `getargs_my_syscall`. Note that since we don't know the arguments yet, this function will take no arguments. The linker links the object files of `sysfile.c` and `syscall.c` (We tell it to do that in the `Makefile`), so everything works out.
  - `getargs_my_syscall` will get the arguments and call the implementation of this syscall (say `implement_my_syscall`). Again we could've named it anything! This function will take the same arguments we gave `my_syscall` (if you write `getargs_my_syscall` correctly that is).
  - The function definition of `implement_my_syscall` is to be placed in `defs.h` so that `sysfile.c` can find it.
  - Implement `implement_my_syscall`
  - Make proper changes to `Makefile` for proper compilation.
