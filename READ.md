# devops
1. What is OSI?
Open Systems Interconnection.

2. How many layers are in the OSI model?
7 layers.

3. Which is Layer 1?
Physical Layer.

4. Which is Layer 2?
Data Link Layer.

5. Which is Layer 3?
Network Layer.

6. Which is Layer 4?
Transport Layer.

7. Which layer uses IP addresses?
Layer 3 – Network.

8. Which layer uses MAC addresses?
Layer 2 – Data Link.

9. Which layer uses TCP and UDP?
Layer 4 – Transport.

10. HTTP works at which layer?
Layer 7 – Application.

11. Which device works at Layer 1?
Hub or repeater.

12. Which device works at Layer 2?
Switch.

13. Which device works at Layer 3?
Router.

14. What is the PDU of Layer 4?
Segment (TCP) or datagram (UDP).

15. What is the PDU of Layer 3?
Packet.

16. What is the PDU of Layer 2?
Frame.

17. What is the PDU of Layer 1?
Bits.

18. Which layer handles routing?
Network Layer.

19. Which layer provides reliable data delivery?
Transport Layer (TCP).

20. Which layer handles encryption and data formatting in the OSI model?
Presentation Layer.

21. What is DNS?
Domain Name System; it translates domain names into IP addresses.

22. Which port does HTTP use?
Port 80.

23. Which port does HTTPS use?
Port 443.

24. Which port does SSH use?
Port 22.

25. Which port does DNS commonly use?
Port 53.

26. What is the full form of TCP?
Transmission Control Protocol.

27. What is the full form of UDP?
User Datagram Protocol.

28. What is the full form of IP?
Internet Protocol.

29. Which layer performs error detection using Ethernet FCS?
Data Link Layer.

30. Which layer interacts with user applications?
Application Layer.

----------------------------------------------------------

Q. What is Windows IIS Server?
Ans > IIS (Internet Information Services) is a web server developed by Microsoft. It runs on Windows Server and is used to host websites, web applications, and APIs.

1. What is IIS?

IIS stands for Internet Information Services. It is Microsoft's web server for hosting websites and web applications on Windows.

2. What is the default port of IIS?
- HTTP: Port 80
- HTTPS: Port 443
3. On which operating system does IIS run?

Windows Server and supported Windows desktop editions.

4. How do you install IIS on Windows Server?

Using Server Manager → Add Roles and Features → Web Server (IIS).

You can also install it using PowerShell:
Install-WindowsFeature -Name Web-Server -IncludeManagementTools

5. What is IIS Manager?

A graphical tool used to configure websites, application pools, bindings, authentication, and other IIS settings.

6. What is a website in IIS?

A website is a configured site that serves web content using a hostname, IP address, port, and physical directory.

7. What is an application pool in IIS?

An application pool isolates web applications into separate worker processes, helping manage their execution and resources.

8. What is a worker process in IIS?

w3wp.exe is the IIS worker process that handles requests for applications running in an application pool.

9. What is a virtual directory?

A virtual directory maps a URL path to a physical directory on the server.

10. What is an IIS binding?

A binding specifies how IIS receives requests, typically using an IP address, port, protocol, and hostname.

11. What is the difference between HTTP and HTTPS in IIS?

HTTP transfers data without TLS encryption. HTTPS uses TLS to encrypt communication between the client and server.

12. What is the difference between a website and an application pool?

A website defines how users access web content. An application pool provides an isolated runtime environment for applications.

13. What is the default IIS website directory?
C:\inetpub\wwwroot


14. How do you host multiple websites on one IIS server?

Configure multiple IIS websites with distinct hostnames, IP addresses, or ports using site bindings.

15. What is an SSL certificate in IIS?

A certificate used to establish a trusted TLS connection for HTTPS.
IIS Troubleshooting Questions
Error / Issue	Common cause
400 Bad Request	Invalid request or malformed headers
401 Unauthorized	Authentication required or failed
403 Forbidden	Access denied or permissions issue
404 Not Found	Requested resource not found
500 Internal Server Error	Application or server-side error
502 Bad Gateway	Proxy or upstream application problem
503 Service Unavailable	Application pool stopped or service unavailable

21. How do you troubleshoot an IIS website that is not opening?
22. Check whether the IIS service is running.
23. Verify the website and application pool status.
24. Check port 80/443 and firewall rules.
25. Verify DNS and IIS bindings.
26. Check IIS logs and Windows Event Viewer.

16. How do you check which process is using port 80?
netstat -ano | findstr :80


17. How do you enable directory browsing in IIS?

Open IIS Manager → select the website → Directory Browsing → Enable. Enable this only when required, because it can expose files.

18. How do you deploy an application to IIS?

Publish the application, copy its files to the configured website directory, configure the application pool and permissions, and test the website.

19. How do you secure IIS?
- Enable HTTPS and use valid certificates.
- Apply Windows security updates.
- Use appropriate authentication and authorization.
- Restrict firewall access and unnecessary modules.
- Keep directory browsing disabled unless needed.
- Monitor logs and configure appropriate permissions.

20. Common IIS Errors and Solutions
Error	Common cause	Solution
400 – Bad Request	Invalid request or headers	Check request URL, headers, and IIS logs
401 – Unauthorized	Authentication failure	Check authentication settings and credentials
403 – Forbidden	Permissions or access restrictions	Check NTFS permissions and IIS authorization
404 – Not Found	File or URL not found	Verify physical path, URL, and application routing
500 – Internal Server Error	Application exception or configuration error	Check application logs and Event Viewer
500.19	Invalid IIS configuration	Validate web.config and configuration sections
500.21	Missing or unregistered handler/module	Install or enable the required IIS module
502 – Bad Gateway	Reverse proxy or upstream failure	Check upstream application and proxy configuration
503 – Service Unavailable	Application pool stopped or unavailable	Check pool status, identity, and event logs
504 – Gateway Timeout	Upstream application taking too long	Investigate application response time and proxy timeout


21. Issue 1: IIS website is not opening
Symptoms: Browser shows “This site can’t be reached” or connection refused.

Possible causes:
- IIS service is stopped.
- Website is stopped.
- Port 80 or 443 is blocked.
- DNS or IIS binding is incorrect.
Solution — run PowerShell as Administrator:

# Check IIS service
Get-Service W3SVC

# Start IIS service if stopped
Start-Service W3SVC

# Check IIS websites
Import-Module WebAdministration
Get-Website

# Check ports
netstat -ano | findstr ":80"
netstat -ano | findstr ":443"

Then verify the website's state, hostname bindings, firewall rules, and DNS resolution.

22. IIS application pool keeps stopping

Symptoms: Website returns HTTP 503, and the application pool is stopped.

Possible causes:
- Application pool identity credentials are invalid.
- Worker process crashes.
- Rapid-Fail Protection has disabled the pool.
- Application startup or configuration errors occur.
Solution:
Import-Module WebAdministration

# List application pools
Get-ChildItem IIS:\AppPools

# Start a specific application pool
Start-WebAppPool -Name "MyAppPool"

# Check its state
Get-WebAppPoolState -Name "MyAppPool"

Next, open Event Viewer → Windows Logs → Application and inspect errors from WAS, W3SVC, or the application runtime. Fix the underlying error before repeatedly restarting the pool.

23. HTTP Error 500.19 — Internal Server Error
Symptoms: IIS cannot load the website configuration.

Common causes:
- Invalid XML in web.config.
- Duplicate or unrecognized configuration sections.
- Missing IIS modules.
- Insufficient permissions to read configuration files.
Solution:
1. Read the detailed IIS error page and identify the configuration error code.
2. Validate the XML in web.config.
3. Check whether the referenced IIS modules and features are installed.
4. Review configuration inheritance and duplicate entries.
5. Check file permissions if IIS cannot read the configuration.
Example configuration file:

<configuration>
  <system.webServer>
    <defaultDocument enabled="true" />
  </system.webServer>
</configuration>

Use this only as a minimal example, not as a replacement for your application's existing configuration.

24. HTTP Error 403 — Forbidden

Symptoms: IIS receives the request but refuses access.

Possible causes and solutions:
- NTFS permissions: Grant the appropriate read/execute permissions to the site's application pool identity.
- Default document missing: Configure a default document such as index.html or default.aspx.
- Directory browsing disabled: Deploy a valid default document rather than enabling directory browsing unnecessarily.
- IP restrictions or authorization rules: Review the site's IIS security settings.
Example: If the application pool is named MyAppPool, its virtual identity is typically:
IIS AppPool\MyAppPool
Grant only the minimum required permissions on the website directory.

25. HTTP Error 404 — Page Not Found
Symptoms: The IIS server responds, but the requested page or resource is unavailable.

Troubleshooting steps:
1. Verify the URL and physical path.
2. Check whether the requested file exists.
3. Confirm the site's binding and application configuration.
4. Check whether the application needs URL Rewrite or specific handler mappings.
5. For ASP.NET applications, verify that the appropriate runtime and hosting components are installed.
For example, a request to /products/123 may need application routing rather than a physical file named 123.

26. HTTPS website shows a certificate error
Symptoms: Browser reports an untrusted, expired, or hostname-mismatched certificate.

Solution:
1. Open IIS Manager → Sites → your website → Bindings.
2. Edit the HTTPS binding on port 443.
3. Select the correct certificate.
4. Check the certificate's expiry date, hostname, trust chain, and private key.
5. For multiple HTTPS hostnames on the same IP and port, check the SNI setting.
6. Ensure inbound TCP port 443 is allowed.
Do not disable certificate validation as a workaround.

27. IIS website is very slow
Symptoms: Pages take a long time to load or requests time out.

Possible causes:
- High CPU or memory usage.
- Database queries are slow.
- Application pool recycling or frequent crashes.
- Too many concurrent requests.
- Network latency or external API delays.
Solution:

Check server resource usage:
Get-Process w3wp |
    Select-Object Id, CPU, WorkingSet64

netstat -ano | findstr ":80"

Then review IIS logs, application logs, Windows Performance Monitor, and database response times. If needed, use Failed Request Tracing to investigate slow requests.

28. IIS website works locally but not from another computer

Symptoms: http://localhost works on the server, but remote users cannot connect.

Solution:

Check the website binding and firewall:
Test-NetConnection localhost -Port 80

From a remote Windows computer, run:
Test-NetConnection SERVER_IP -Port 80

If local access works but remote access fails, inspect Windows Firewall, network ACLs, security appliances, routing, and the IIS binding address.

29. ASP.NET application fails after deployment

Symptoms: The application worked in testing but returns HTTP 500 after deployment.

Possible causes:
- Missing .NET hosting bundle.
- Incorrect application pool configuration.
- Missing environment variables or configuration settings.
- Incorrect application folder permissions.
- Missing application dependencies.
Solution:
- Verify the target .NET version and hosting requirements.
- Check the application pool's .NET CLR setting; modern ASP.NET Core apps commonly use No Managed Code in IIS because the application runs through the ASP.NET Core Module.
- Confirm that the ASP.NET Core Hosting Bundle is installed when required.
- Check Event Viewer and application logs for the actual exception.

30. IIS port 80 is already in use

Symptoms: IIS cannot bind to port 80, or another service is receiving requests.

Solution:

netstat -ano | findstr ":80"

Find the process using the PID:

Get-Process -Id <PID>

Replace <PID> with the actual process ID. Identify the service before stopping it; do not terminate an unknown process.

===========================================================================================================

Linux command
-----------------------
Command				Purpose							Example
pwd					Show current directory			pwd
ls					List files and directories		ls -l
ls -a				Show hidden files				ls -a
cd					Change directory				cd /etc
cd ..				Move one directory back			cd ..
clear				Clear terminal screen			clear
whoami				Show current user				whoami
hostname			Show system hostname			hostname
date				Show date and time				date
history				Show command history			history

-----------
2. File and Directory Commands

Command				Purpose							Example
touch				Create an empty file			touch file.txt
mkdir				Create a directory				mkdir test
mkdir -p			Create nested directories		mkdir -p app/logs
cat					Display file contents			cat file.txt
less				Read a file page by page		less file.txt
head				Show first 10 lines	head 		file.txt
tail				Show last 10 lines				tail file.txt
cp					Copy files						cp a.txt b.txt
mv					Move or rename files			mv old.txt new.txt
rm					Delete a file					rm file.txt
rmdir				Delete an empty directory		rmdir test
rm -r				Delete a directory recursively	rm -r test

---------------------------
3. File Permissions and Ownership

Command				Purpose							Example
chmod				Change file permissions			chmod 755 script.sh
chown				Change file owner				sudo chown user:user file.txt
ls -l				View permissions and ownership	ls -l
sudo				Run a command with elevated privileges	sudo apt update

-------------------------------
4. User Management Commands

Command				Purpose							Example
whoami				Current username				whoami
id					Show user and group IDs			id
sudo useradd		Create a user					sudo useradd john
sudo passwd			Set a user's password			sudo passwd john
sudo usermod		Modify a user account			sudo usermod -aG sudo john
who					Show logged-in users			who
su					Switch user						su - john

--------------------------------------
5. System Monitoring Commands

Command				Purpose
top					Monitor processes and system resources
free -h				Check RAM usage
df -h				Check disk space
du -sh /var/log		Check directory size
uptime				Check system uptime and load
ps aux				List running processes
uname -a			Show kernel and system information
lscpu				Display CPU information
lsblk				List disks and partitions
dmesg				View kernel messages

-----------------------------------------
6. Process Management Commands

Command				Purpose							Example
ps					View processes					ps aux
top					Monitor processes live			top
pgrep				Find a process by name			pgrep nginx
kill				Send a signal to a process		kill 1234
kill -9				Forcefully terminate a process	kill -9 1234
jobs				List background jobs			jobs
&					Run a command in the background	sleep 100 &

-----------------------------------------------
7. Networking Commands
Command				Purpose							Example
ip addr				Show IP addresses				ip addr
ip route			Show routing table				ip route
ping				Test network connectivity		ping -c 4 google.com
curl	S			end HTTP requests				curl -I https://example.com
ss -tulpn			Show listening ports and sockets	sudo ss -tulpn
hostname -I			Show assigned IP addresses		hostname -I
nslookup			Query DNS						nslookup google.com
traceroute			Trace network path				traceroute google.com

------------------------------------
8. Package Management Commands (Ubuntu)

Command							Purpose
sudo apt update					Refresh package lists
sudo apt upgrade				Upgrade installed packages
sudo apt install nginx			Install Nginx
sudo apt remove nginx			Remove a package
apt list --installed			List installed packages

----------------------------------------------
9. Service Management Commands

Command										Purpose
systemctl status nginx						Check service status
sudo systemctl start nginx					Start a service
sudo systemctl stop nginx					Stop a service
sudo systemctl restart nginx				Restart a service
sudo systemctl enable nginx					Start service automatically at boot
sudo systemctl disable nginx				Disable automatic startup
journalctl -u nginx							View service logs

--------------------------------------------------
10. Searching and Log Troubleshooting
Command				Purpose					Example
grep				Search text				grep "error" app.log
find				Find files				find /var/log -name "*.log"
tail -f				Follow a log live		tail -f /var/log/syslog
sort				Sort lines				sort names.txt
wc -l				Count lines				wc -l file.txt
history				Review previous commands	history


==================================================================================================

Apache Server
--------------------------------
1. What is Apache?

Apache HTTP Server is an open-source web server used to host websites and web applications.

2. What is the default port of Apache?
- HTTP: Port 80
- HTTPS: Port 443

3. How do you install Apache on Ubuntu?
sudo apt update
sudo apt install apache2 -y

4. How do you check Apache status?
sudo systemctl status apache2

5. How do you start Apache?
sudo systemctl start apache2

6. How do you restart Apache?
sudo systemctl restart apache2

7. How do you enable Apache at boot?
sudo systemctl enable apache2

8. Where is the default website directory on Ubuntu?
/var/www/html

9. Where is the main Apache configuration file on Ubuntu?
/etc/apache2/apache2.conf

10. How do you check the Apache version?
apache2 -v

11. What is a Virtual Host?
A Virtual Host allows one Apache server to host multiple websites using different domain names or IP addresses.

12. What is the difference between name-based and IP-based Virtual Hosts?
- Name-based: Multiple websites share an IP address and are distinguished by hostname.
- IP-based: Websites use different IP addresses.

13. Where are Virtual Host configurations stored in Ubuntu?
/etc/apache2/sites-available/
/etc/apache2/sites-enabled/

14. How do you enable a website configuration?
sudo a2ensite example.conf
sudo systemctl reload apache2

15. How do you disable a website?
sudo a2dissite example.conf
sudo systemctl reload apache2

16. How do you test Apache configuration syntax?
sudo apache2ctl configtest

17. What is .htaccess?
A directory-level configuration file used for supported directives such as URL rewriting and access control. Its use depends on the server's AllowOverride configuration.

18. What is mod_rewrite?
An Apache module used to rewrite or redirect URLs.

Enable it on Ubuntu with:
sudo a2enmod rewrite
sudo systemctl reload apache2

19. What is a reverse proxy in Apache?
A reverse proxy forwards client requests to backend servers. Apache can provide this functionality using modules such as mod_proxy and mod_proxy_http.

20. How do you enable HTTPS in Apache on Ubuntu?
sudo a2enmod ssl
sudo systemctl restart apache2

You must also configure an HTTPS Virtual Host and a valid TLS certificate.

21. Apache Troubleshooting Questions
Issue							Possible solution
Website not opening				Check Apache service, firewall, DNS and ports
HTTP 403 Forbidden				Check file permissions and access rules
HTTP 404 Not Found				Verify document root and requested path
HTTP 500 Internal Server Error	Check application and Apache error logs
Apache fails to start			Run apache2ctl configtest and inspect logs
Port 80 already in use			Run sudo ss -ltnp \| grep ':80'
Website shows the wrong page	Check Virtual Host configuration and hostname
HTTPS certificate error			Check certificate validity, hostname and TLS configuration


22. How do you view Apache error logs live?
sudo tail -f /var/log/apache2/error.log

23. How do you check whether Apache is listening on port 80?
sudo ss -ltnp | grep ':80'

24. How do you reload Apache without a full restart?
sudo systemctl reload apache2

25. What should you check if Apache returns 403?
Check directory permissions, file permissions, Apache access directives, and whether the directory has an appropriate index file.

26. How can Apache host two websites on one server?
Configure separate Virtual Hosts with different ServerName values and document roots, enable both sites, test the configuration, and reload Apache.

27. How do you troubleshoot a slow Apache website?
Check CPU and memory usage, Apache logs, backend response times, database performance, network latency, and the active Apache MPM and worker configuration.

==========================================================================================
Docker 
-------------------------

1. Basic Docker Questions
1. What is Docker?

Docker is a containerization platform used to package and run applications with their dependencies in isolated containers.

2. What is a container?

A container is an isolated environment that runs an application and its dependencies using the host operating system's kernel.

3. What is a Docker image?

A Docker image is a read-only template used to create containers.

4. What is the difference between an image and a container?
- Image: Blueprint for an application.
- Container: Running or stopped instance created from an image.
5. What is Docker Hub?

Docker Hub is a cloud-based registry for storing and sharing container images.

6. What is a Dockerfile?

A Dockerfile is a text file containing instructions to build a Docker image.

7. What is Docker Engine?

Docker Engine is the core technology used to build and run containers.

8. What is Docker Compose?

Docker Compose is a tool for defining and managing multi-container applications using a YAML file.

9. What is a Docker registry?

A Docker registry stores and distributes Docker images. Docker Hub is one example.

10. What is the difference between a virtual machine and a Docker container?

A virtual machine runs its own guest operating system, while containers share the host kernel and generally use fewer resources.

-------------------
Basic Docker Commands
------------------

Command							Purpose
docker --version				Check Docker version
docker pull nginx				Download an image
docker images					List local images
docker run nginx				Create and start a container
docker ps						List running containers
docker ps -a					List all containers
docker stop CONTAINER_ID		Stop a container
docker start CONTAINER_ID		Start a stopped container
docker restart CONTAINER_ID		Restart a container
docker rm CONTAINER_ID			Remove a stopped container
docker rmi IMAGE_NAME			Remove an image
docker logs CONTAINER_ID		View container logs
docker exec -it CONTAINER_ID sh	Open a shell inside a running container

11. What is the FROM instruction?

It specifies the base image.
FROM nginx:alpine

12. What is the RUN instruction?

It executes commands while building an image.
RUN apk add --no-cache curl

13. What is the CMD instruction?

It specifies the default command to run when a container starts. Runtime arguments can override it.

14. What is the ENTRYPOINT instruction?

It defines the main executable for a container. It is commonly used with CMD to provide default arguments.

15. What is the COPY instruction?

It copies files from the build context into the image.
COPY index.html /usr/share/nginx/html/

16. What is the EXPOSE instruction?

It documents the port the application is expected to listen on. It does not publish the port by itself.

17. How do you build a Docker image?
docker build -t myapp:v1 .

18. How do you run a container on port 8080?
docker run -d -p 8080:80 --name web nginx
Here, host port 8080 maps to container port 80.

19. What is Docker networking?

Docker networking allows containers to communicate with each other, the host, and external networks.

20. What are common Docker network drivers?
- bridge — common for standalone containers.
- host — uses the host network namespace.
- none — disables normal container networking.
- overlay — connects containers across Docker hosts in supported setups.
21. What is a Docker volume?

A volume provides persistent storage that can survive container removal.
docker volume create mydata

22. What is port mapping in Docker?

Port mapping exposes a container port through a host port.
docker run -d -p 8080:80 nginx

23. How do you troubleshoot a container that exits immediately?
docker ps -a
docker logs CONTAINER_ID
docker inspect CONTAINER_ID

Check the exit code, logs, startup command, and configuration.

24. How do you check container resource usage?
docker stats

25. How do you copy a file into a running container?
docker cp file.txt CONTAINER_ID:/tmp/file.txt

26. How do you remove unused Docker resources?
docker system prune
Review the prompt before confirming. This removes unused resources, and additional flags can delete more data.

27. What is a Docker layer?

A layer is a filesystem change created by certain Dockerfile instructions. Images reuse layers to improve storage efficiency and build performance.

28. What is a .dockerignore file?

It excludes specified files and directories from the Docker build context.

29. What is the difference between docker stop and docker kill?
- docker stop requests graceful termination before forcing a stop after a timeout.
- docker kill sends a signal immediately, by default SIGKILL.
30. What is the difference between docker run and docker exec?
- docker run creates and starts a new container.
- docker exec runs a command inside an already-running container.

==================================================================================
Git

1. Basic Git Questions
1. What is Git?

Git is a distributed version control system used to track changes in source code and collaborate with other developers.

2. What is version control?

Version control tracks file changes over time so you can review history and restore earlier versions.

3. What is the difference between Git and GitHub?
- Git: A version control tool.
- GitHub: An online platform for hosting Git repositories and collaborating on projects.
4. What is a repository?

A repository is a project containing files and their version history.

5. What is a commit?

A commit records a snapshot of changes in a Git repository.

6. What is a branch?

A branch is an independent line of development used to work on features or fixes.

7. What is merging in Git?

Merging combines changes from one branch into another.

8. What is cloning?

Cloning creates a local copy of a remote repository and its history.
git clone https://github.com/user/project.git

9. What is the staging area?

The staging area holds selected changes that will be included in the next commit.

10. What is .gitignore?

A .gitignore file specifies files and directories that Git should normally ignore when they are untracked.

11. What is the difference between git fetch and git pull?
- git fetch downloads remote updates without integrating them into your current branch.
- git pull fetches updates and then integrates them, usually by merging or rebasing depending on configuration and options.
12. What is the difference between git add and git commit?

git add stages changes, while git commit saves the staged changes in the repository history.

13. What is a remote repository?

A repository hosted elsewhere, such as on GitHub, GitLab, or Bitbucket, that developers can share.

14. How do you check configured remote repositories?
git remote -v

15. How do you create a new branch?
git switch -c feature-login

16. How do you delete a local branch?
git branch -d feature-login

17. What is a merge conflict?

A merge conflict occurs when Git cannot automatically reconcile overlapping changes. You must resolve the affected files and complete the merge.

18. What is git stash?

It temporarily saves changes in the working tree so you can work on something else.
git stash
git stash pop

19. What is git cherry-pick?

It applies the changes introduced by a specific commit to the current branch.
git cherry-pick COMMIT_ID

20. What is HEAD in Git?

HEAD usually points to the currently checked-out commit or branch.

21. How do you undo unstaged changes to a file?

git restore file.txt
This discards the file's unstaged changes. Use it carefully.

22. How do you unstage a file?
git restore --staged file.txt

This removes the file from the staging area but keeps its working-tree changes.

23. What is the difference between git reset and git revert?
- git reset moves the current branch reference and can change the staging area or working tree.
- git revert creates a new commit that reverses the changes from an earlier commit.
For shared branches, git revert is generally safer because it preserves existing history.

24. How do you view commit history in a compact format?

git log --oneline

25. How do you rename a branch?
git branch -m new-branch-name

26. How do you resolve a merge conflict?
1. Run git status to identify conflicted files.
2. Edit the files and resolve conflict markers.
3. Stage the resolved files with git add.
4. Complete the merge with git commit, if Git requires it.

27. How is Git used in DevOps?

Git stores application code and infrastructure configuration, tracks changes, and triggers CI/CD pipelines when changes are pushed.

28. What is a pull request?

A pull request is a collaboration workflow for reviewing proposed changes before merging them into a target branch. GitHub and GitLab provide similar workflows.

29. What is CI/CD integration with Git?

CI/CD tools such as Jenkins or GitHub Actions can monitor repository changes and automatically build, test, and deploy applications.

30. What is the difference between Git merge and Git rebase?
- merge combines branch histories and may create a merge commit.
- rebase reapplies commits onto a new base, creating new commit IDs and a more linear history.


Important Git Commands

Command						Purpose
git --version				Check Git version
git init					Initialize a repository
git clone URL				Copy a remote repository
git status					Check working-tree status
git add file.txt			Stage a file
git add .					Stage changes under the current directory
git commit -m "message"		Commit staged changes
git log						View commit history
git branch					List local branches
git switch branch-name		Switch branches
git switch -c new-branch	Create and switch to a branch
git merge branch-name		Merge a branch
git pull					Fetch and integrate remote changes
git push					Upload local commits to a remote
git diff					View unstaged changes

==================================================================================================
Kubernetes
----------

1. What is Kubernetes?

Kubernetes (K8s) is an open-source container orchestration platform used to deploy, manage, scale, and monitor containerized applications.

2. What is a Kubernetes cluster?

A Kubernetes cluster consists of a control plane and worker nodes that run containerized applications.

3. What is a Pod?

A Pod is the smallest deployable unit in Kubernetes. It contains one or more containers that share networking and can share storage.

4. What is a Node?

A node is a worker machine, either physical or virtual, that runs Pods.

5. What is a Deployment?

A Deployment manages replicated Pods and supports rolling updates and rollbacks.

6. What is a Service?

A Service provides a stable network endpoint for accessing a group of Pods.

7. What are the main types of Kubernetes Services?
- ClusterIP — accessible within the cluster.
- NodePort — exposes a service through a port on each node.
- LoadBalancer — provisions or integrates with an external load balancer when supported.
8. What is a Namespace?

A Namespace logically separates resources within a Kubernetes cluster.

9. What is a ReplicaSet?

A ReplicaSet ensures that a specified number of Pod replicas are running. Deployments typically manage ReplicaSets.

10. What is kubectl?

kubectl is the command-line tool used to interact with a Kubernetes cluster.

11. What is the Control Plane?

The control plane manages the cluster and makes decisions about scheduling and maintaining the desired state.

12. What is the API Server?

The API Server is the main entry point for Kubernetes API requests.

13. What is etcd?

etcd is a distributed key-value store that stores Kubernetes cluster state and configuration data.

14. What is the Scheduler?

The Scheduler selects a suitable node for newly created Pods that have not yet been assigned to a node.

15. What is the Controller Manager?

It runs controllers that work to keep the actual cluster state aligned with the desired state.

16. What is kubelet?

Kubelet is an agent on each node that ensures assigned Pods and their containers are running as expected.

17. What is a Container Runtime?

It runs containers on Kubernetes nodes, such as containerd or CRI-O.

18. What is a Kubernetes manifest file?

A YAML or JSON file that declares Kubernetes resources and their desired configuration.

19. What is the difference between kubectl create and kubectl apply?
- kubectl create creates resources and generally returns an error if they already exist.
- kubectl apply creates or updates resources declaratively based on the manifest.
20. What is ConfigMap?

A ConfigMap stores non-sensitive configuration data, such as application settings.

21. What is a Secret?

A Secret stores sensitive configuration data, such as credentials. Proper access controls and encryption at rest are important because base64 encoding alone is not encryption.

22. What is a PersistentVolume (PV)?

A PersistentVolume represents storage available for use by workloads in a cluster.

23. What is a PersistentVolumeClaim (PVC)?

A PVC is a request for storage made by a workload or user.

24. What is an Ingress?

Ingress defines HTTP/HTTPS routing rules to services inside a Kubernetes cluster. It requires an Ingress controller to implement those rules.

25. What is CrashLoopBackOff?

It means a container repeatedly crashes and Kubernetes is delaying subsequent restart attempts. Check logs, startup commands, configuration, and resource limits.

kubectl logs POD_NAME --previous
kubectl describe pod POD_NAME

26. What is ImagePullBackOff?

Kubernetes cannot pull the requested container image. Check the image name, tag, registry access, and image pull credentials.

27. What does Pending mean for a Pod?

The Pod has not yet been scheduled or fully set up. Check node capacity, scheduling constraints, storage claims, and events.

28. What is the difference between liveness and readiness probes?
- Liveness probe: Determines whether a container should be restarted.
- Readiness probe: Determines whether a Pod should receive traffic.
A startup probe can also protect slow-starting applications from premature liveness failures.


Important Kubernetes Commands
----------------------------------

Command										Purpose
kubectl get nodes							List cluster nodes
kubectl get pods							List Pods
kubectl get pods -A							List Pods in all namespaces
kubectl get deployments						List Deployments
kubectl get services						List Services
kubectl describe pod POD_NAME				Inspect Pod details and events
kubectl logs POD_NAME						View container logs
kubectl exec -it POD_NAME -- sh				Open a shell inside a container
kubectl apply -f app.yaml					Create or update resources from YAML
kubectl delete -f app.yaml					Delete resources defined in a manifest
kubectl scale deployment APP --replicas=3	Set the Deployment to three replicas
kubectl rollout restart deployment APP		Restart Deployment Pods through a rollout

--------------------
Example of Create a file named deployment.yaml:

apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:stable
          ports:
            - containerPort: 80
			
			
====================================================================================================================
Terraform
-------------

1. What is Terraform?
Terraform is an Infrastructure as Code (IaC) tool used to create, manage, and update infrastructure using configuration files.

2. What is Infrastructure as Code (IaC)?
IaC is the practice of managing infrastructure through code instead of manually creating resources.

3. Who developed Terraform?
Terraform was developed by HashiCorp.

4. Which language does Terraform use?
Terraform uses HashiCorp Configuration Language (HCL).

5. Is Terraform an open-source tool?
Terraform is source-available under the Business Source License (BSL) 1.1. OpenTofu is an open-source alternative.

6. What is a Terraform provider?
A provider is a plugin that allows Terraform to interact with a platform or service, such as AWS, Azure, or Google Cloud.

7. What is a Terraform resource?
A resource represents an infrastructure object that Terraform manages, such as an EC2 instance, S3 bucket, or VPC.

8. What is a Terraform module?
A module is a reusable collection of Terraform configuration files used to organize and reuse infrastructure code.

9. What does terraform init do?
It initializes the directory, downloads required providers, and prepares the backend.

10. What does terraform plan do?
It compares the desired configuration with the current infrastructure and shows the changes Terraform plans to make.

11. What does terraform apply do?
It creates, updates, or deletes infrastructure according to the configuration.

12. What does terraform destroy do?
It removes infrastructure managed by the Terraform configuration and state.

13. What is a Terraform state file?
The state file tracks the resources Terraform manages and maps them to real infrastructure. The default filename is terraform.tfstate.

14. Why is the state file important?
Terraform uses the state file to understand what infrastructure exists and determine what changes are needed.

15. What is a Terraform backend?
A backend defines where Terraform stores its state file, such as locally or in an AWS S3 bucket.

16. What is remote state?
Remote state is Terraform state stored in a shared remote location, enabling teams to access the same state.

17. What is state locking?
State locking prevents multiple Terraform operations from modifying the same state at the same time.

18. What is a Terraform variable?
A variable allows users to pass values into Terraform configuration.

variable "instance_type" {
  default = "t2.micro"
}

19. What is a Terraform output?
An output displays useful information after Terraform applies a configuration.

output "instance_id" {
  value = aws_instance.web.id
}

20. What is a Terraform data source?
A data source reads information about existing infrastructure without managing its lifecycle.

21. What is the difference between a resource and a data source?
Resource										Data Source
Creates or manages infrastructure				Reads existing information
Can update or delete the managed object			Does not manage its lifecycle

22. What is the difference between terraform plan and terraform apply?
plan previews the proposed changes. apply executes those changes.

23. What happens if you manually create a resource in AWS Console?
Terraform will not automatically manage it. You can import the resource into Terraform and update your configuration and state accordingly.

24. How do you delete only one Terraform resource?
Use a targeted destroy:

terraform destroy -target=aws_instance.web

Use targeting carefully; it may not account for all related infrastructure changes.

25. What is the purpose of .tf files?
Terraform configuration files use the .tf extension and contain infrastructure definitions written in HCL.

------------------
Terraform Commands
Command						Purpose
terraform init				Initializes the working directory
terraform validate			Checks configuration syntax and validity
terraform fmt				Formats Terraform files
terraform plan				Shows proposed infrastructure changes
terraform apply				Applies the planned changes
terraform destroy			Deletes resources managed by Terraform
terraform state list		Lists resources in the state
terraform output			Displays configured output values

-------
Basic Terraform Workflow

terraform init
terraform fmt
terraform validate
terraform plan
terraform apply

==================================
Terraform Scenario-Based Interview Q&A

1. You created an EC2 instance using Terraform. How do you change its instance type?
Update the instance_type value in the Terraform configuration.

resource "aws_instance" "web" {
  ami           = "ami-123456"
  instance_type = "t3.small"
}

Run:
terraform plan
terraform apply

Terraform will determine whether the change can be applied in place or requires replacing the instance.


2. You manually created an EC2 instance in AWS Console. How can Terraform manage it?

Use terraform import to import the existing resource into Terraform state.

terraform import aws_instance.web i-0123456789abcdef0

Then ensure the Terraform configuration matches the actual resource.

3. You accidentally deleted an EC2 instance manually. What will Terraform do?

When you run:

terraform plan

Terraform detects that the instance is missing and may plan to recreate it if it is still declared in the configuration.

4. Your Terraform state file is deleted. What will you do?
- If remote state is configured, recover the state from the remote backend or its backups.
- If local state is lost, restore a backup if available.
- If no backup exists, consider importing existing resources into a new state.
Avoid running terraform apply blindly.


5. Two team members run terraform apply at the same time. What can happen?
They may conflict while modifying the same infrastructure state.

Use a remote backend with state locking to prevent concurrent state modifications.

6. You want to create resources in multiple AWS regions. How can you do that?
Use multiple AWS provider configurations with aliases.

provider "aws" {
  region = "ap-south-1"
}

provider "aws" {
  alias  = "us"
  region = "us-east-1"
}

Assign the required provider to each resource.

7. You need to create 5 EC2 instances. How can you do that?
Use count.

resource "aws_instance" "web" {
  count         = 5
  ami           = "ami-123456"
  instance_type = "t2.micro"
}


8. You need to create EC2 instances with different names. What will you use?
Use for_each with a map.

resource "aws_instance" "web" {
  for_each      = toset(["dev", "test", "prod"])
  ami           = "ami-123456"
  instance_type = "t2.micro"

  tags = {
    Name = each.key
  }
}

9. You want to create an S3 bucket in Terraform. What resource will you use?
Use aws_s3_bucket.

resource "aws_s3_bucket" "app" {
  bucket = "my-unique-app-bucket-123"
}

The bucket name must be globally unique.

10. You want to use an existing security group in Terraform. What will you do?
Use a data source.

data "aws_security_group" "existing" {
  id = "sg-0123456789abcdef0"
}

Then reference it in the EC2 resource:

vpc_security_group_ids = [data.aws_security_group.existing.id]


11. You want to delete only one resource from Terraform. How can you do that?
Use a targeted destroy:

terraform destroy -target=aws_instance.web

Use this carefully and review the plan before applying.

12. Terraform keeps showing unexpected changes. What should you check?
- Check whether someone changed the resource manually.
- Run terraform plan.
- Verify the configuration and state.
- Use terraform refresh only if appropriate; in modern workflows, terraform plan and apply refresh state automatically unless refresh is disabled.


13. You want to store Terraform state in AWS S3. What will you configure?
Configure an S3 backend.

terraform {
  backend "s3" {
    bucket       = "my-terraform-state-bucket"
    key          = "dev/terraform.tfstate"
    region       = "ap-south-1"
    use_lockfile  = true
  }
}

The bucket must exist before Terraform initializes this backend. Configure appropriate permissions and versioning.

14. You want to pass different values for dev and prod environments. What can you use?
Use variable files.

terraform apply -var-file="dev.tfvars"
terraform apply -var-file="prod.tfvars"

Example dev.tfvars:
instance_type = "t2.micro"

15. You want to create a VPC, subnet, and EC2 instance. How do you ensure dependencies?
Terraform automatically detects dependencies when one resource references another.

subnet_id = aws_subnet.public.id

Use depends_on only when a dependency is not expressed through a direct reference.

16. Terraform says a resource already exists. What should you do?
Check whether the resource already exists in AWS and in Terraform state.

If it exists but is not in state, import it. If it is already managed, review the configuration and state before applying.

