# Celestial-Depolying-Record

On Wmware Ubuntu 18.04
# First Fix "Virtualized Intel VT-x/EPT is not supported on this platform. Continue without virtualized Intel VT-x/EPT"
1, Windows Task Manager Check *Virtualized*
2, Turn Windows Features on or off 
Hyper-V

Virtual Machine Platform

Windows Hypervisor Platform

Windows Subsystem for Linux
![image](https://github.com/AkiMoo/Celestial-Depolying-Record/assets/32764968/54d47a71-f704-4d53-acae-6fa957cd4564)

3, Windows Security => Device Security => Core isolation,
Memory integrity

Local Security Authority protection
![image](https://github.com/AkiMoo/Celestial-Depolying-Record/assets/32764968/86f6442d-f6cb-4385-bbd4-4cdba96ceb3d)

Finally,
![image](https://github.com/AkiMoo/Celestial-Depolying-Record/assets/32764968/466ce5dd-e996-44b1-a797-933ec65d313c)

# Second fix "cd ~/celestial // docker build -f compile.Dockerfile -t celestial-make ."

socket need root authorized, so it need to switch to root.

and another problem is go & golang
1, apt install golang-go
answer in here: https://blog.csdn.net/m0_56203480/article/details/127637912
2, https://www.cnblogs.com/zhangmingcheng/p/15870370.html cant fix
