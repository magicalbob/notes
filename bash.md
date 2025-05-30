# bash 

## Using set -o pipefail

The set -o pipefail option alters the behavior of pipelines:

The pipeline’s exit code becomes the exit code of the first failing command.

## Using $PIPESTATUS for Detailed Exit Codes

While set -o pipefail provides the overall exit code of a pipeline, the $PIPESTATUS array allows you to inspect the exit codes of each command individually.

## for loops

To echo numbers from 1 to 4:

	for i in {1..4}; do echo $i; done

So to get uptime of nodes from node1 to node4:

	for i in {1..4}; do ssh node$i uptime; done

## time

The time command is a Unix/Linux utility that measures the execution time of a command. When you prefix any command with time, it runs that command and then outputs timing statistics after the command completes.

e.g.

	time git pull origin master  
	From github.com:magicalbob/vagrant_kubernetes.wiki
	 * branch            master     -> FETCH_HEAD
	Already up to date.
	git pull origin master  0.08s user 0.07s system 5% cpu 2.762 total

These statistics mean:

	0.08s user: CPU time spent in user mode (executing your program's code)
	0.07s system: CPU time spent in kernel mode (executing system calls)
	5% cpu: Percentage of CPU utilized during execution
	2.762 total: Total elapsed real time (wall clock time) in seconds

This is a common way to benchmark or measure the performance of commands on Unix-like systems. In this case, it shows that the git pull operation took about 2.76 seconds in total, but only used a small amount of CPU time, likely because most of the time was spent waiting for network communication with GitHub.

## ports

Use `netstat -ntlp` to list all the ports that have tcp listeners. You will have to do it with `sudo` to get all the process IDs.

On a Mac use `sudo lsof -iTCP -sTCP:LISTEN -P -n` instead.
