## Environment

iso: ubuntu-16.04.3-desktop-amd64.iso

kernel: 

```
root@integrity:~# uname -a
Linux integrity 4.10.0-28-generic #32~16.04.2-Ubuntu SMP Thu Jul 20 10:19:48 UTC 2017 x86_64 x86_64 x86_64 GNU/Linux
root@integrity:~#
```

get the kernel source code：

```
apt-get install linux-source
```

check when compiling kernel：

```
    Kernel hacking  --->
        [*] Export kernel pagetable layout to userspace via debugfs
```

## Description

```
├── patch
│   └── dump_all_pagetables.c
├── x86_32
└── x86_64
    ├── kernel_all_page_tables
    ├── kernel_all_user_page_tables
    └── kernel_page_tables
```

- `patch`: the patch to be added in the kernel source code.
    - `dump_all_pagetables.c` should be added into `arch/x86/mm/`. Additionally, please modify the Makefile: `obj-$(CONFIG_X86_PTDUMP_CORE)   += dump_pagetables.o dump_all_pagetables.o` 
- `x86_64`: page tables in `x86_64`
    - `kernel_all_page_tables`: exported by `dump_all_pagetables.c` when the pgd points to `&init_level4_pgt`(`x86_64`) or `swapper_pg_dir`(`x86_32`) 
    - `kernel_all_user_page_tables`: exported by `dump_all_pagetables.c` when the pgd points to `current->mm->pgd`
- - `x86_32`: page tables in `x86_32`
