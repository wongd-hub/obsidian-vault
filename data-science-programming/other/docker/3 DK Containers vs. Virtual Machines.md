#course_datacamp-docker #docker

## Containers and Virtual Machines
- Containers and VMs both enable you to run processes side-by-side on the same physical  machine without interfering with each other. However, there is a difference from the technical perspective, making their use-cases different.

### Resource Virtualisation
- Running processes side-by-side on the same physical machine safely is done via resource virtualisation.
- Resource virtualisation means that resources such as RAM, Disk, and CPU can be split up and look like separate resources to the software using them. e.g. A 100GB hard disk can be virtualised into 4 25GB disks, each used by separate processes with no overlap. 
    - Both containers and VMs are virtualisation technologies.

![[Pasted image 20230904221906.png]]

### Main Differences
- The key difference between containers and virtual machines is that VMs virtualise the entire computer down to the hardware. Whereas virtualisation in containers happens in a software layer above the operating system.
    - Separation in VMs is better since only the hardware is shared by processes; whereas in containers, the host OS is also shared

- This means that VMs are more secure. Attackers may be able to break out of a container and gain access to the host OS, thereby gaining access to all other containers. 
    - This risk means that Docker and other container providers spend extensive resources on ensuring security. Using industry-standard containers is generally pretty safe; however consider using VMs when security is paramount, like when working with sensitive data.

![[Pasted image 20230904222148.png]]

![[Pasted image 20230904223055.png]]

- Containers are more lightweight than VMs - they hold a much smaller size in memory and on disk compared to VMs. This is because containers only need to hold a small part of the full OS, sharing the rest of the OS with the host and other containers. This means that:
    - Containers are faster to start and stop
    - Containers are easy to distribute and update
    - There is a large library of pre-made images with many popular software applications such as programming languages, databases, or web servers pre-installed
- In comparison, VMs can be GBs in size, resulting in them generally being made from scratch each time
    - However, if your workflow uses applications that require a GUI, then VMs are the only way to go. VMs support GUI and command-line based applications, whereas containers only support command-line based applications.