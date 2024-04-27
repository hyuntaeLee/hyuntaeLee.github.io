---
layout: post
title: 【Back_01】 Character Device Drivers
subtitle: Review
categories: Linux
banner:
  image: https://github.com/hyuntaeLee/hyuntaeLee.github.io/assets/97331148/10f87e65-fde3-4a2d-9af9-7d7b442f3ad7
tags: [linux, kernel, Review]
---

## Device Driver?

Device란 컴퓨터에 물려있는 여러 주변장치를 뜻한다.  

네트워크 어댑터, 오디오, 터미널, 키보드, 하드 디스크, 플린터 등이 Device에 해당하고, 이러한 디바이들을 컨트롤하기 위한 Device Driver가 존재한다.

`/dev/`아래에 들어있는 파일들은 Device Driver와 연결된 interface 역할을 하며(Device Driver interface), 이를 통해 응용 프로그램이 하드웨어오 독립적으로 디바이스에 접근할 수 있다.

<p align="center">
<img src="https://github.com/hyuntaeLee/hyuntaeLee.github.io/assets/97331148/b19d21d1-e4f5-42c9-9e79-dc384656d6c9" width = 500>
</p>

1. **실제 물리적 하드웨어(Real Device)**
    - 실제 컴퓨터 시스템에 연결된 다양한 물리적 하드웨어 장치들이 있습니다.

2. **디바이스 드라이버(Device Driver)**
    - 각 하드웨어 장치는 특정 규격과 인터페이스를 가지고 있습니다.
    - 개발자가 모든 하드웨어 규격에 맞춰 직접 구현하는 것은 비효율적이다.
        - 따라서 리눅스에서는 각 장치별로 해당 하드웨어를 제어할 수 있는 Device Driver를 제공한다.

3. **VFS(Virtual File System)**
    - 리눅스는 VFS라는 가상 파일 시스템 계층을 제공한다.
    - 이를 통해 모든 디바이스 드라이버가 `/dev` 디렉터리 아래에 파일로 표현된다.

4. **디바이스 파일 인터페이스**
    - 응용 프로그램은 `open()`, `read()`, `write()` 등의 시스템 호출을 통해 디바이스 파일에 접근할 수 있다.
    - 이렇게 함으로써 하드웨어와 독립적으로 디바이스를 제어할 수 있다.

5. **디바이스 파일 등록**
    - 모든 디바이스 파일은 고유한 번호(major, minor)와 이름을 가지고 있다.
    - 따라서 새로운 디바이스 드라이버를 제작하고 등록하려면, 고유한 번호와 이름을 할당받아야 한다.

Linux는 Device Driver와 VFS 계층을 통해 물리적 하드웨어와 응용 프로그램 사이의 추상화 계층을 제공합니다. 이를 통해 개발자는 효율적으로 다양한 하드웨어를 지원할 수 있다.

<p align="center">
<img src="https://github.com/hyuntaeLee/hyuntaeLee.github.io/assets/97331148/b4939a43-752e-4ea1-bf91-31db7fcc0688" width = 500>
</p>

사진으로 통해, 한눈에 Device Driver의 종류를 확인할수 있으며, 해당 Device Driver가 어떠한 Device를 위해 존재하는지 알수 있다.

## Character Device Drivers

<p align="center">
<img src="https://github.com/hyuntaeLee/hyuntaeLee.github.io/assets/97331148/ee7564af-c758-40bf-bdba-c9e54159d9cc" width = 500>
</p>

* **Character Device Drivers**는 유저 프로세스로부터 직접(**buffer cache**등을 사용하지 않고) 데이터를 읽고 쓰는 디바이스 드라이버이다.
    * ex) keyboard, Sound card, Printer 드라이버

* 그리고 **Block Device, Network Device** 드라이버가 있다.
    * 블록 디바이스 드라이버는 버퍼 캐시를 통한 임의 접근과 블록 단위로 입출력이 가능하다.
        * ex) 하드디스크

    * 네트워크 디바이스 드라이버는 네트워크 스택과 네트워크 하드웨어 사이에 위치해 데이터의 송수신을 담당한다.
        * ex) Ethernet, Network interface card

## struct file_operations

* **file_operations** 구조체는 **Character Device, Block Device** 드라이버와 일반 프로그램간의 통신을 위해 제공되는 인터페이스입니다.
    * read, write, open, release, unlocked_ioctl 등의 함수 포인터를 사용할 수 있다.
    * Network Device는 file_operations 구조체를 사용하지 않는다.
        * "include/linux/netdevice.h"파일의 "net_device" 구조체를 사용한다.

```c
struct file_operations {
	struct module *owner;
	loff_t (*llseek) (struct file *, loff_t, int);
	ssize_t (*read) (struct file *, char __user *, size_t, loff_t *);
	ssize_t (*write) (struct file *, const char __user *, size_t, loff_t *);
	ssize_t (*read_iter) (struct kiocb *, struct iov_iter *);
	ssize_t (*write_iter) (struct kiocb *, struct iov_iter *);
	int (*iopoll)(struct kiocb *kiocb, struct io_comp_batch *,
			unsigned int flags);
	int (*iterate_shared) (struct file *, struct dir_context *);
	__poll_t (*poll) (struct file *, struct poll_table_struct *);
	long (*unlocked_ioctl) (struct file *, unsigned int, unsigned long);
	long (*compat_ioctl) (struct file *, unsigned int, unsigned long);
	int (*mmap) (struct file *, struct vm_area_struct *);
	unsigned long mmap_supported_flags;
	int (*open) (struct inode *, struct file *);
	int (*flush) (struct file *, fl_owner_t id);
	int (*release) (struct inode *, struct file *);
	int (*fsync) (struct file *, loff_t, loff_t, int datasync);
	int (*fasync) (int, struct file *, int);
	int (*lock) (struct file *, int, struct file_lock *);
	unsigned long (*get_unmapped_area)(struct file *, unsigned long, unsigned long, unsigned long, unsigned long);
	int (*check_flags)(int);
	int (*flock) (struct file *, int, struct file_lock *);
	ssize_t (*splice_write)(struct pipe_inode_info *, struct file *, loff_t *, size_t, unsigned int);
	ssize_t (*splice_read)(struct file *, loff_t *, struct pipe_inode_info *, size_t, unsigned int);
	void (*splice_eof)(struct file *file);
	int (*setlease)(struct file *, int, struct file_lock **, void **);
	long (*fallocate)(struct file *file, int mode, loff_t offset,
			  loff_t len);
	void (*show_fdinfo)(struct seq_file *m, struct file *f);
#ifndef CONFIG_MMU
	unsigned (*mmap_capabilities)(struct file *);
#endif
	ssize_t (*copy_file_range)(struct file *, loff_t, struct file *,
			loff_t, size_t, unsigned int);
	loff_t (*remap_file_range)(struct file *file_in, loff_t pos_in,
				   struct file *file_out, loff_t pos_out,
				   loff_t len, unsigned int remap_flags);
	int (*fadvise)(struct file *, loff_t, loff_t, int);
	int (*uring_cmd)(struct io_uring_cmd *ioucmd, unsigned int issue_flags);
	int (*uring_cmd_iopoll)(struct io_uring_cmd *, struct io_comp_batch *,
				unsigned int poll_flags);
} __randomize_layout;
```

* 예를 들어 다음과 같은 방법으로 디바이스 모듈에서 open()함수를 제공할 수 있다.
    * file_operations 구조체에 작성된 함수 포인터의 인자값을 이용하여 open 함수의 코드를 작성한다.
    
    * 작성한 함수의 명은 "struct file_operations" 형태로 선언된 구조체의 ".open" 필드에 함수의 주소(chardev_open)를 저장한다.

```c
static int chardev_open(struct inode *inode, struct file *file)
{
    printk("chardev_open");
    return 0;
}
struct file_operations chardev_fops = {
    .open    = chardev_open,
};
```

## Simple Char Device Driver 

<p align="center">
<img src="https://github.com/hyuntaeLee/hyuntaeLee.github.io/assets/97331148/0f1059ed-ea6c-49c5-8f2b-900f1aaf8f47" width = 500>
</p>

해당 사진에 있는 과정에서 mknod 명령어를 통해 디바이스 파일을 만든다. 해당 명령어로 디바이스 파일을 만들었으면, 이제 응용프로그램에서 open(), read()등을 이용해서 디바이스 파일에 접근한다.  

kernel module이 초기에 등록될때 init과정 거치는데, 이 안에서 char device를 등록하는 로직을 넣어줘야한다. 아래와 같은 함수를 사용한다.

```c
int register_chrdev(unsigned int major, const char *name, const struct file_operations *fops);
```

* char device함수는 위와 같이 3개으 인수를 갖는다.
* file_operations 구조체는 Char Device, Block Device 드라이버와 일반 응용 프로그램간의 통신을 위해 제공되는 인터페이스라고 보면 된다.
	* read, write, open, release 등의 함수포인터를 사용하여 디바이스 드라이버를 제작하면 된다.
	* Network device는 file_operations 구조체를 사용하지 않고 "include/linux/netdevice.h" 의 net_device 구조체를 사용한다.

### system call sequence

사진으로 예시로 함 수 호출 과정을 정리하겠다. 디바이스 모듈 코드를 작성할떄 `my_open`이라는 함수를 사용한다면, .open 필드에 해당 함수 이름을 저장한다.  

제작한 디바이스 모듈이 커널에 등록되고 init() 함수가 동작할때 init() 내부에 작성한 `register_chrdev(.. , test_device , ..)` 함수가 호출되면서 char device가 등록된다.

이제 응용프로그램에서 해당 Device를 open하면, my_open함수가 호출된다.

<p align="center">
<img src="https://github.com/hyuntaeLee/hyuntaeLee.github.io/assets/97331148/dc0abf0e-47e0-4958-9151-79e5233b6670" width = 550>
</p>

리눅스에서 디바이스 드라이버 개발에 필수적인 개념 중 하나는 `시스템 호출`과 `파일 연산`의 연동이다. 사용자 공간에서 파일을 열기, 읽기, 쓰기 또는 닫기 등의 시스템 호출을 할 때, 커널은 이러한 요청을 처리하기 위해 등록된 디바이스 드라이버의 함수를 호출한다. 이 과정은 `트랩`을 통해 이루어진다.

system call은 다음에 자세하세 정리하는걸로...

### Device module code

```c
#include <linux/init.h>           // Macros used to mark up functions e.g. __init __exit
#include <linux/module.h>         // Core header for loading LKMs into the kernel
#include <linux/device.h>         // Header to support the kernel Driver Model
#include <linux/kernel.h>         // Contains types, macros, functions for the kernel
#include <linux/fs.h>             // Header for the Linux file system support
#include <linux/uaccess.h>          // Required for the copy to user function
#define  DEVICE_NAME "ebbchar"    ///< The device will appear at /dev/ebbchar using this value
#define  CLASS_NAME  "ebb"        ///< The device class -- this is a character device driver
 
MODULE_LICENSE("GPL");            ///< The license type -- this affects available functionality
MODULE_AUTHOR("Derek Molloy");    ///< The author -- visible when you use modinfo
MODULE_DESCRIPTION("A simple Linux char driver for the BBB");  ///< The description -- see modinfo
MODULE_VERSION("0.1");            ///< A version number to inform users
 
static int    majorNumber;                  ///< Stores the device number -- determined automatically
static char   message[256] = {0};           ///< 사용자 공간에서 전달된 문자열을 위한 메모리
static short  size_of_message;              ///< 저장된 문자열의 크기를 기억하기 위해 사용됨
static int    numberOpens = 0;              ///< Counts the number of times the device is opened
static struct class*  ebbcharClass  = NULL; ///< 디바이스-드라이버 클래스 구조체 포인터
static struct device* ebbcharDevice = NULL; ///< 디바이스-드라이버 디바이스 구조체 포인터
 
// The prototype functions for the character driver -- must come before the struct definition
static int     dev_open(struct inode *, struct file *);
static int     dev_release(struct inode *, struct file *);
static ssize_t dev_read(struct file *, char *, size_t, loff_t *);
static ssize_t dev_write(struct file *, const char *, size_t, loff_t *);
 
/** @brief Devices are represented as file structure in the kernel. The file_operations structure from
 *  /linux/fs.h lists the callback functions that you wish to associated with your file operations
 *  using a C99 syntax structure. char devices usually implement open, read, write and release calls
 */
static struct file_operations fops =
{
   .open = dev_open,
   .read = dev_read,
   .write = dev_write,
   .release = dev_release,
};
 
/** @brief The LKM initialization function
 *  The static keyword restricts the visibility of the function to within this C file. The __init
 *  macro means that for a built-in driver (not a LKM) the function is only used at initialization
 *  time and that it can be discarded and its memory freed up after that point.
 *  @return returns 0 if successful
 */
static int __init ebbchar_init(void){
   printk(KERN_INFO "EBBChar: Initializing the EBBChar LKM\n");
 
   // Try to dynamically allocate a major number for the device -- more difficult but worth it
   majorNumber = register_chrdev(0, DEVICE_NAME, &fops);
   if (majorNumber<0){
      printk(KERN_ALERT "EBBChar failed to register a major number\n");
      return majorNumber;
   }
   printk(KERN_INFO "EBBChar: registered correctly with major number %d\n", majorNumber);
 
   // Register the device class
   ebbcharClass = class_create(CLASS_NAME);
   if (IS_ERR(ebbcharClass)){                // Check for error and clean up if there is
      unregister_chrdev(majorNumber, DEVICE_NAME);
      printk(KERN_ALERT "Failed to register device class\n");
      return PTR_ERR(ebbcharClass);          // Correct way to return an error on a pointer
   }
   printk(KERN_INFO "EBBChar: device class registered correctly\n");
 
   // Register the device driver
   ebbcharDevice = device_create(ebbcharClass, NULL, MKDEV(majorNumber, 0), NULL, DEVICE_NAME);
   if (IS_ERR(ebbcharDevice)){               // Clean up if there is an error
      class_destroy(ebbcharClass);           // Repeated code but the alternative is goto statements
      unregister_chrdev(majorNumber, DEVICE_NAME);
      printk(KERN_ALERT "Failed to create the device\n");
      return PTR_ERR(ebbcharDevice);
   }
   printk(KERN_INFO "EBBChar: device class created correctly\n"); // Made it! device was initialized
   return 0;
}
 
/** @brief The LKM cleanup function
 *  Similar to the initialization function, it is static. The __exit macro notifies that if this
 *  code is used for a built-in driver (not a LKM) that this function is not required.
 */
static void __exit ebbchar_exit(void){
   device_destroy(ebbcharClass, MKDEV(majorNumber, 0));     // remove the device
   class_unregister(ebbcharClass);                          // unregister the device class
   class_destroy(ebbcharClass);                             // remove the device class
   unregister_chrdev(majorNumber, DEVICE_NAME);             // unregister the major number
   printk(KERN_INFO "EBBChar: Goodbye from the LKM!\n");
}
 
/** @brief 장치가 열릴 때마다 호출되는 장치 open 함수
 *  이 경우에는 numberOpens 카운터만 증가합니다.
 *  @param inodep inode 객체에 대한 포인터(리눅스/fs.h에서 정의됨)
 *  @param filep 파일 객체에 대한 포인터(리눅스/fs.h에서 정의됨)
 */
static int dev_open(struct inode *inodep, struct file *filep){
   numberOpens++;
   printk(KERN_INFO "EBBChar: Device has been opened %d time(s)\n", numberOpens);
   return 0;
}
 
/** @brief 이 함수는 사용자 공간에서 장치가 읽힐 때마다 호출됩니다. 즉, 장치에서 사용자에게 데이터가 전송됩니다.
 *  이 경우에는 사용자에게 버퍼 문자열을 보내는 copy_to_user() 함수를 사용하고 오류를 캡처합니다.
 *  @param filep 파일 객체에 대한 포인터(리눅스/fs.h에서 정의됨)
 *  @param buffer 이 함수가 데이터를 작성하는 버퍼에 대한 포인터
 *  @param len 버퍼의 길이
 *  @param offset 필요한 경우 오프셋
 */

static ssize_t dev_read(struct file *filep, char *buffer, size_t len, loff_t *offset){
   int error_count = 0;
   // copy_to_user has the format ( * to, *from, size) and returns 0 on success
   error_count = copy_to_user(buffer, message, size_of_message);
 
   if (error_count==0){            // if true then have success
      printk(KERN_INFO "EBBChar: Sent %d characters to the user\n", size_of_message);
      return (size_of_message=0);  // clear the position to the start and return 0
   }
   else {
      printk(KERN_INFO "EBBChar: Failed to send %d characters to the user\n", error_count);
      return -EFAULT;              // Failed -- return a bad address message (i.e. -14)
   }
}
 
/** @brief 이 함수는 사용자 공간에서 장치로 데이터가 전송될 때마다 호출됩니다.
 *  즉, 사용자로부터 장치로 데이터가 전송됩니다. 이 LKM에서는 sprintf() 함수를 사용하여
 *  message[] 배열로 데이터를 복사하고 문자열의 길이를 함께 추가합니다.
 *  @param filep 파일 객체에 대한 포인터
 *  @param buffer 디바이스에 쓸 문자열이 포함된 버퍼
 *  @param len 전달되는 const char 버퍼 데이터 배열의 길이
 *  @param offset 필요한 경우 오프셋
 */
static ssize_t dev_write(struct file *filep, const char *buffer, size_t len, loff_t *offset){
   sprintf(message, "%s(%zu letters)", buffer, len);   // appending received string with its length
   size_of_message = strlen(message);                 // store the length of the stored message
   printk(KERN_INFO "EBBChar: Received %zu characters from the user\n", len);
   return len;
}
 
/** @brief 장치가 사용자 공간 프로그램에 의해 닫히거나 해제될 때마다 호출되는 장치 release 함수
 *  @param inodep inode 객체에 대한 포인터(리눅스/fs.h에서 정의됨)
 *  @param filep 파일 객체에 대한 포인터(리눅스/fs.h에서 정의됨)
 */
static int dev_release(struct inode *inodep, struct file *filep){
   printk(KERN_INFO "EBBChar: Device successfully closed\n");
   return 0;
}
 
/** @brief A module must use the module_init() module_exit() macros from linux/init.h, which
 *  identify the initialization function at insertion time and the cleanup function (as
 *  listed above)
 */
module_init(ebbchar_init);
module_exit(ebbchar_exit);
```

`register_chrdev()`함수말고 추가로 `class_create()`와 `device_create()`를 사용하면 mknod를 수동으로 사용할 필요 없이 디바이스 노드(/dev/ 디렉토리 내의 파일)를 자동으로 생성할 수 있다.

### Makefile

```c
obj-m+=ebbchar.o

all:
        make -C /lib/modules/$(shell uname -r)/build/ M=$(PWD) modules
        $(CC) testebbchar.c -o test
clean:
        make -C /lib/modules/$(shell uname -r)/build/ M=$(PWD) clean
        rm test
```

### User program code
```c
#include<stdio.h>
#include<stdlib.h>
#include<errno.h>
#include<fcntl.h>
#include<string.h>
#include<unistd.h>
 
#define BUFFER_LENGTH 256               ///< The buffer length (crude but fine)
static char receive[BUFFER_LENGTH];     ///< The receive buffer from the LKM
 
int main(){
   int ret, fd;
   char stringToSend[BUFFER_LENGTH];
   
   printf("Starting device test code example...\n");
   fd = open("/dev/ebbchar", O_RDWR);             // Open the device with read/write access
   if (fd < 0){
      perror("Failed to open the device...");
      return errno;
   }
   
   printf("Type in a short string to send to the kernel module:\n");
   scanf("%[^\n]%*c", stringToSend);                // Read in a string (with spaces)
   
   printf("Writing message to the device [%s].\n", stringToSend);
   ret = write(fd, stringToSend, strlen(stringToSend)); // Send the string to the LKM
   if (ret < 0){
      perror("Failed to write the message to the device.");
      return errno;
   }
 
   printf("Press ENTER to read back from the device...\n");
   getchar();
 
   printf("Reading from the device...\n");
   ret = read(fd, receive, BUFFER_LENGTH);        // Read the response from the LKM
   if (ret < 0){
      perror("Failed to read the message from the device.");
      return errno;
   }
   printf("The received message is: [%s]\n", receive);
   printf("End of the program\n");
   return 0;
}
```

User program은 User에게 문자열을 요청하고 이를 /dev/ebbchar 장치에 쓰는 짧은 프로그램입니다. 후속 키(ENTER)를 누르면 장치에서 응답을 읽고 이를 터미널 창에 출력한다.

### build & run

```bash
ee@lee-virtual-machine:~/Desktop/kernel$ make
make -C /lib/modules/6.5.0-27-generic/build/ M=/home/lee/Desktop/kernel modules
make[1]: Entering directory '/usr/src/linux-headers-6.5.0-27-generic'
warning: the compiler differs from the one used to build the kernel
  The kernel was built by: x86_64-linux-gnu-gcc-12 (Ubuntu 12.3.0-1ubuntu1~22.04) 12.3.0
  You are using:           gcc-12 (Ubuntu 12.3.0-1ubuntu1~22.04) 12.3.0
make[1]: Leaving directory '/usr/src/linux-headers-6.5.0-27-generic'
cc testebbchar.c -o test
lee@lee-virtual-machine:~/Desktop/kernel$ ls
ebbchar.c    ebbchar.mod.c  Makefile        test
ebbchar.ko   ebbchar.mod.o  modules.order   testebbchar.c
ebbchar.mod  ebbchar.o      Module.symvers
lee@lee-virtual-machine:~/Desktop/kernel$ sudo insmod ebbchar.ko
lee@lee-virtual-machine:~/Desktop/kernel$ lsmod | grep "ebb"
ebbchar                12288  0
```

그러면 이제 mknod를 사용해서 Device를 만들어준다. 하지만 위에서 설명했듯이, `class_create()`와 `device_create()`를 사용하면 mknod를 수동으로 사용할 필요 없다. 한번 확인해보자.

```bash
lee@lee-virtual-machine:~/Desktop/kernel$ sudo rmmod ebbchar.ko
lee@lee-virtual-machine:~/Desktop/kernel$ ls -l /dev | grep "ebb"
lee@lee-virtual-machine:~/Desktop/kernel$ sudo insmod ebbchar.ko
lee@lee-virtual-machine:~/Desktop/kernel$ ls -l /dev | grep "ebb"
crw-------   1 root root    240,   0 Apr 23 11:17 ebbchar
```

rmmod를 통해 삭제하고 `/dev`를 확인해보면, 보시다시피 Device가 보이지 않는다. 하지만 insmod를 한순간 `/dev`위치에 해당 Device가 생성된게 보인다.

```bash
lee@lee-virtual-machine:~/Desktop/kernel$ sudo ./test
Starting device test code example...
Type in a short string to send to the kernel module:
hello my name is NAME
Writing message to the device [hello my name is NAME].
Killed
```

## reference
* [https://jeongzero.oopy.io/c5c9c223-d17f-4bbc-b054-4d9fa1faffd1](https://jeongzero.oopy.io/c5c9c223-d17f-4bbc-b054-4d9fa1faffd1)
* [https://pr0gr4m.tistory.com/entry/Linux-Kernel-5-Character-Device-Driver](https://pr0gr4m.tistory.com/entry/Linux-Kernel-5-Character-Device-Driver)
* [https://www.lazenca.net/display/TEC/02.Character+Device+Drivers](https://www.lazenca.net/display/TEC/02.Character+Device+Drivers)
* [https://butter-shower.tistory.com/29](https://butter-shower.tistory.com/29)
* [https://linuxhint.com/basic-character-driver-linux/](https://linuxhint.com/basic-character-driver-linux/)
* [https://derekmolloy.ie/writing-a-linux-kernel-module-part-2-a-character-device/](https://derekmolloy.ie/writing-a-linux-kernel-module-part-2-a-character-device/)
* [https://temp123.tistory.com/16?category=877924](https://temp123.tistory.com/16?category=877924)