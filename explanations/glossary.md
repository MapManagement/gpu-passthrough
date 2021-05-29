# Glossary

## SVM / AMD-V
SVM is the apronym for "**S**ecure **V**irtual **M**achine". In the past, most virtualization was software-based but
because of modern tasks for virtualized operating systems, a hardware-based solutions had to be invented since software-
based solutions often caused errors and crashes if they wanted to use the same resouces at the same time. Therefore AMD invented SVM (Intel also supports hardware-based virtualization, it's called [Intel® VT](https://www.intel.com/content/www/us/en/virtualization/virtualization-technology/intel-virtualization-technology.html).
Because of SVM, modern AMD processors support directly virtualization. That means that your virtual machines runs much
faster than without it.

## KVM

KVM is also an apronym and means nothing less than "**K**ernel-based **V**irtual **M**achine". Beside VMs, there are also KVM switches but in this
case it's an abbreviation for "**K**eyboard, **V**ideo and **M**ouse". The KVM I'm talking about is a software for virtualization which is included 
in the Linux kernel. It runs under x86 hardware and is able to use hardwae-virtualization like AMD's AMD-V or Intel's VT. Because of KVM you can
easily create multiple virtual machines and run them simultaneously. Since KVM is directly built into the Linux kernel, we call the system which runs
the virtual machines **host** and it's virtual machines **guests**. More information is provided [here](https://www.redhat.com/en/topics/virtualization/what-is-KVM.)

## CSM