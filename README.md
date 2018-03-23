
Saeed Mirzamohammadi, Justin A. Chen, Ardalan Amiri Sani, Sharad Mehrotra, Gene Tsudik, "Ditio: Trustworthy Auditing of Sensor Activities in Mobile & IoT Devices," in Proc. ACM Int. Conf. Embedded Networked Sensor Systems (SenSys) [PDF](http://www.ics.uci.edu/~saeed/Mirzamohammadi_SenSys17.pdf).


# People

* [Saeed Mirzamohammadi](http://www.ics.uci.edu/~saeed)
* Justin A. Chen
* [Ardalan Amiri Sani](http://www.ics.uci.edu/~ardalan)
* [Sharad Mehrotra](http://www.ics.uci.edu/~sharad)
* [Gene Tsudik](http://www.ics.uci.edu/~gts)


# 1. Ditio Components Overview

Components of Ditio are in two main categories.

First, the logging components which consists of the configuration app in the user space and the log recorder in the kernel that records the I/O activities in Nexus 5.

Second, the companion tool which consists of the frontend and the formally verified backend.
The backend includes the implementation and the proof written in Coq verification language.


## 1.1 Logging components

There are 2 repositories.

* Android Nexus 5 kernel (LineageOS hammerhead) which includes the added fault handler for logging and hardcoded addresses for logging device registers of camera in Nexus 5.
* The logging components including the configuration app for starting and stopping a logging session and the log recorder kernel module.

### 1.1.1 Setting up the base system

Originally, Ditio is built over Cyanogenmod. Due to unavailability of Cyanogenmod, we provide our instructions using LineageOS.
Follow the instructions here to build LineageOS For Google Nexus 5 ("hammerhead"):
https://wiki.lineageos.org/devices/hammerhead/build

**Note: In the initialization section, use the version cm-12.1 instead of cm-14.1.

Change your current directory to the android system kernel directory and check out Ditio repository using the following instructions:

```sh
cd ~/android/system/kernel/lge/hammerhead
git add remote ditio_origin https://github.com/trusslab/Ditio_hammerhead_v1
git checkout Ditio_hammerhead_v1
```sh

### 1.1.2 Copying files to Nexus 5

```sh
$ git clone https://github.com/trusslab/Ditio_LogRecorder.git
$ cd Ditio_LogRecorder
$ source push_to_Nexus5.sh
```


### 1.1.3 Loading the kernel modules

The log recorder component is implemented in a kernel module.
Follow the instructions to load the module.
First, you need to be in adb shell.
For example, to get to the server shell, use:

```sh
$ sudo adb shell
$ su
```

Now, load the kernel module using:

```sh
$ cd /data/local
$ source load_log_recorder.sh
```

### 1.1.4 Starting the configuration app

Now run the configuration app using the following command to start recording.

```sh
$ source run_config_app_start.sh
```

To stop recording, run the following:

```sh
$ source run_config_app_stop.sh
```

### 1.1.5 The log files

The log files are generated in the /data/local/ directory.
Copy the generated log files to the companion tool for query processing.
You will get the companion tool folder in the next section.

## 1.2 Companion Tool Implementation and Proof

This includes the frontend and the backend.

### 1.2.1 Downloading the files

Clone the source files with the command below:

```sh
$ git clone https://github.com/trusslab/Ditio_companion.git
```

### 1.2.2 Generating Backend checks and Downloading the proof

Clone the Coq source files with the command below:

```sh
$ git clone https://github.com/trusslab/Ditio_companion_proof.git
```

Follow the instructions on README file to build the Coq files and to generate the query checks for ARM.
You can also go over the proof files using the Coq Proof Assistant: https://coq.inria.fr/download

### 1.2.3 Running the frontend for processing the logs

Add the generated camera_audit_backend.S to the Ditio_companion folder. Then run the following command to process the logs.

```sh
$ source process_query.sh
```

# 2. ChangeLog

* November 6, 2017: version 1.0 is released.


# 3. Supported Features

In this released source code, we open-sourced the kernel-only implemented of our design mentioned in the paper. Here are the features of Ditio that we support in this release version. The rest of the features and the TrustZone-enabled design in Juno board will be added in the future.

* Recording camera sensor activities in Nexus 5 and auditing the activity logs in a companion tool.
* Required extensions to Android framework (LineageOS-12.1).
* Log recorder kernel module for recording the Nexus 5 camera acitivities.
* A companion tool for processing the log files generated the by the Log recorder module.
* Proof of the correctness of the companion tool backend in Coq.

