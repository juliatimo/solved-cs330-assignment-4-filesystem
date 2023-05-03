Download Link: https://assignmentchef.com/product/solved-cs330-assignment-4-filesystem
<br>
<strong>Prerequisites: </strong>This assignment is to be done on FUSE (file system in user space) framework. You should use a Linux system with FUSE packages installed. To install the FUSE packages on Ubuntu, use the following commands.

$sudo apt-get install fuse

$sudo apt-get install libfuse-dev

Ensure that your system has at least 10GB free hard disk space. Play around with the attached source (along with a simple implementation in <em>example </em>folder). Read the README files to understand the usage.

Objectives of the assignment is to implement an object store with key-value semantics (with hidden file semantics) using the FUSE APIs. As part of the assignment, you are required to build an object store on the disk (emulated using disk.img file), provide concurrent access mechanisms to the object store and implement caching. The assignment is split into two parts where Part B is a superset of Part A. Design the solution for high performance and scalability. During evaluation, we may use disk of size up to 32 GB storing up to 10<sup>6 </sup>objects in the store. Assume that, maximum size of the key is 32 bytes and maximum object size is 16MB.

<h2>Part A – The object store</h2>

In this part of the assignment, you are required to implement the object store functionalities. The template file (objstore.c) contains all the function definitions that you are required to implement. To understand the exact semantics, please refer to the template code and the example implementation. Below listed are some important points regarding the design constructs.

<ul>

 <li>A generic structure representing the file system (struct objfs state) is declared in h. An instance of this structure is passed to all the object store functions. One of the members of this function is a generic pointer (objstore data). You can declare and initialize your own structures and assign the objstore data pointer during file system mount (objstore init) and cleanup during unmount (objstore destroy). <em>Do not modify the existing structures and declarations</em></li>

 <li>You are required to maintain a unique object ID (integer) for all the objects. Object ID 0, 1 are reserved. Object IDs are useful when reading/writing from the objects.</li>

 <li>Read operations read all the contents of an object and write operations overwrite the content and update the size. Note that, the requested read size can be more than the object size. In such a case read should read the contents into the buffer and return the requested size.</li>

 <li>You can access (read from and write to) the block device using the following functions,</li>

</ul>

extern int read_block(struct objfs_state *objfs, long block_offset, char *buf); extern int write_block(struct objfs_state *objfs, long block_offset, char *buf);

1

Note that, the block offset is in multiples of 4KB (Block size of the FS). The buf parameter address must be 4KB aligned (see the example).

<ul>

 <li>The object store must work correctly for concurrent access scenarios. The only assured scenario is that the same object will not be concurrently accessed by multiple threads/processes.</li>

 <li>You can use any data structures of your choice. Make sure that, the in-memory data structures used are space efficient and are scalable.</li>

 <li>Use dprintf and check the logs in log file. Normal printf does not work.</li>

</ul>

<h2>Part B – The object store with caching</h2>

As part of this assignment, you are required to use a cache to improve the application throughput by employing some caching scheme. The cache size is 128MB (already initialized) and accessible through the struct objfs state instance (member cache). The cache is size is in bytes and cache is 4KB aligned. Your cache implementation should get enabled by a compile time flag CACHE (adding -DCACHE option in the Makefile). Refer to the example directory as an illustrative implementation.