## Table of Contents


**Namespaces**: Various types that act as the isolating block from one application to another. The creation of namespaces happends throguh the `clone` system call. Developers sometimes also attach existing namespaces. Some of the namespaces used by docker include the following:
- PID Namespace
	- Isolates the process identification number space. that mean processing using different PID namespaces can have the same PID. The PID namespace allows containers to deliver functionalities like suspending and resuming the set of original processes within the container. It also gives devlopers the ability to migrate the container from one host to another while the processes running within the container maintain their original PIDs.
- The Net Namespace
- IPC Namespace
- MNT Namespace
- User Namespace

https://github.com/CTFd/ctfcli/blob/master/ctfcli/spec/challenge-example.yml


