
Colima -> Lima -> containerd -> container

container: An isolated space, where we can run our software application on any cloud / non-cloud environment. Consists of bundled configuration files, libraries and dependencies.

containerd : a daemon for Linux and Windows, which can manage the complete container lifecycle of its host system: image transfer and storage, container execution and supervision, low-level storage and network attachments, etc.

dockerd -> containerd
dockerd (docker daemon) will parse the request and check if the image is available, if not, will pull from specific registry. Once you have image, dockerd will shift control to containerd which will create container using that image.

lima - Linux virtual machine for running containerd on macOS
https://lima-vm.io/docs/config/vmtype/
https://lima-vm.io/docs/config/multi-arch/

rosetta - x86 to arm64 translation
Rosetta is a translation process that allows users to run apps that contain x86_64 instructions on Apple's arm processor

Problem with translation - 
slow launch of app, mostly the first time

rosetta 2 introduced AOT (ahead-of-time) translation, i.e. translate an app as soon as it is installed



https://erzeghi.medium.com/yet-another-brief-history-of-container-d-2962eac9679e#:~:text=It%20started%20in%20December%202015,a%20daemon%20to%20control%20runC.






