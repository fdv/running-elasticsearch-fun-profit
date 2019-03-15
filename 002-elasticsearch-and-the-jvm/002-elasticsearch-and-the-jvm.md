```
WIP, COVERS ELASTICSEARCH 5.5.x, UPDATING TO ES 6.5.x
```

# Elasticsearch and the Java Virtual Machine

Elasticsearch is a software written in Java. It requires the Java Runtime Environment (JRE) deployed on the same host to run. Currently supported versions of Elasticsearch can run on the following operating systems / distributions and Java.

## Supported JVM and operating systems / distributions

The following matrices present the various operating systems and Java Virtual Machines (JVM) officially supported by Elastic for both 2.4.x and 5.5.x versions. Every operating system or JVM not mentioned here is not supported by Elastic and therefor should not be used.

### Operating system matrix 

|     | CentOS/RHEL 6.x/7.x | Oracle Enterprise Linux 6/7 with RHEL Kernel only | Ubuntu 14.04 | Ubuntu 16.04 | **Ubuntu 18.04** | SLES 11 SP4\*\*/12 | SLES 12 | openSUSE Leap 42 |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **ES 5.0.x** | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ |
| **ES 5.1.x** | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ |
| **ES 5.2.x** | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ |
| **ES 5.3.x** | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ |
| **ES 5.4.x** | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ |
| **ES 5.5.x** | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ |
| **ES 6.0.x** | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ✅ | ✅ |
| **ES 6.1.x** | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ✅ | ✅ |
| **ES 6.2.x** | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ✅ | ✅ |
| **ES 6.3.x** | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ✅ | ✅ |
| **ES 6.4.x** | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ✅ | ✅ |
| **ES 6.5.x** | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ |


|     | Windows Server 2012/R2 | Windows Server 2016 | Debian 7 | Debian 8 | Debian 9 | **Solaris / SmartOS** | Amazon Linux |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **ES 5.0** | ✅ | ❌ | ✅ | ✅ | ❌ | ❌ | ✅ |
| **ES 5.1.x** | ✅ | ❌ | ✅ | ✅ | ❌ | ❌ | ✅ |
| **ES 5.2.x** | ✅ | ❌ | ✅ | ✅ | ❌ | ❌ | ✅ |
| **ES 5.3.x** | ✅ | ❌ | ✅ | ✅ | ❌ | ❌ | ✅ |
| **ES 5.4.x** | ✅ | ❌ | ✅ | ✅ | ❌ | ❌ | ✅ |
| **ES 5.5.x** | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ |
| **ES 6.0.x** | ✅ | ✅ | ❌ | ✅ | ✅ | ❌ | ✅ |
| **ES 6.1.x** | ✅ | ✅ | ❌ | ✅ | ✅ | ❌ | ✅ |
| **ES 6.2.x** | ✅ | ✅ | ❌ | ✅ | ✅ | ❌ | ✅ |
| **ES 6.3.x** | ✅ | ✅ | ❌ | ✅ | ✅ | ❌ | ✅ |
| **ES 6.4.x** | ✅ | ✅ | ❌ | ✅ | ✅ | ❌ | ✅ |
| **ES 6.5.x** | ✅ | ✅ | ❌ | ✅ | ✅ | ❌ | ✅ |

Elasticsearch runs on both OpenSolaris and FreeBSD. FreeBSD 11.1 provides an Elasticsearch 6.4.2 package maintained by [Mark Felder](mailto:feld@freebsd.org), but neither of these operating systems are officially supported by Elastic.

### Java Virtual Machine matrix

|     | Oracle/OpenJDK 1.8.0u111+ | Oracle/OpenJDK 9 | OpenJDK 10 | OpenJDK 11 | Azul Zing 16.01.9.0+ | IBM J9 |
| --- |:---:|:---:|:---:|:---:|:---:| --- |
| **ES 5.0.x** | ✅ | ❌ | ❌ | ❌ | ✅ | ❌ |
| **ES 5.1.x** | ✅ | ❌ | ❌ | ❌ | ✅ | ❌ |
| **ES 5.2.x** | ✅ | ❌ | ❌ | ❌ | ✅ | ❌ |
| **ES 5.3.x** | ✅ | ❌ | ❌ | ❌ | ✅ | ❌ |
| **ES 5.4.x** | ✅ | ❌ | ❌ | ❌ | ✅ | ❌ |
| **ES 5.5.x** | ✅ | ❌ | ❌ | ❌ | ✅ | ❌ |
| **ES 5.6**.x | ✅ | ❌ | ❌ | ❌ | ✅ | ❌ |
| **ES 6.0.x** | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |
| **ES 6.1.x** | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |
| **ES 6.2.x** | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| **ES 6.3.x** | ✅ | ❌ | ✅ | ❌ | ❌ | ❌ |
| **ES 6.4.x** | ✅ | ❌ | ✅ | ❌ | ❌ | ❌ |
| **ES 6.5.x** | ✅ | ❌ | ❌ | ✅ | ❌ | ❌ |



## Memory management

TODO

## Garbage collection

Java is a garbage collected language. The developer does not have to manage the memory allocation. The Java Virtual Machine periodically runs a specific system thread called GC Thread that takes care of the various garbage collection activities. One of them is reclaiming the memory occupied by objects that are no longer in use by the program.

Java 1.8 comes with 3 different garbage collector families, which all have their own feature.

The *Single Collector* uses a single thread to perform the whole garbage collection process. It is efficient on single processor machines, as it suppresses the overhead implied by the communication between threads, but not suitable for most real world use today. It was designed for heaps managing small datasets, of an order of 100MB.

The *Parallel Collector* runs small garbage collections in parallel. Running parallel collections reduces the garbage collection overhead. It was designed for medium to large datasets running on multi threaded hosts.

The *Mostly Concurrent Collector* perform most of its work concurrently to keep garbage-collection pauses short. It is designed for large sized datasets, when response time matters, because the technique used to minimise pauses can affect the application performances. Java 1.8 offers two Mostly Concurrent Collectors, the *Concurrent Mark & Sweep Garbage Collector*, and the *Garbage First Garbage Collector*, also known as G1GC.

### Concurrent Mark & Sweep Garbage Collector

TODO

### Garbage First Garbage Collector

TODO
