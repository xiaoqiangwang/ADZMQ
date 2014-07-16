ADZMQ
=====

This project attempts to integrate ZeroMQ with areaDetector framework, in two folds:

* a driver pulls data from a ZeroMQ server and generates NDArray.
* a plugin publishes NDArray as ZeroMQ server.

The ZeroMQ message format is detailed [here](https://docs.google.com/document/d/1npYDPxXL4yXiSZNyNKXW_E8ut0p2XhfSb0AJP5Wjpi0/edit#heading=h.mea8pioczwh2).

![Architecture Overview](documentation/Architecture.png)

Build
-----
If you build under PSI environment, 
```bash
    cd psiBuild
    make
```

Otherwise, use the standard EPICS base system. Edit *configure/RELEASE* and then run
```bash
    make
```

Test
----

After building, the examplar IOC *zmqApp* is created. Follow the test procedure,

1. Start the IOC, 
   ```bash
   cd iocBoot/iocZMQ
   ../../bin/linux-x86_64/zmqApp st.cmd
   ```

2. Start the ZeroMQ server. It creates an image of 800x600 of int8 type. The update
frequency is 1 Hz. 
   ```bash
   cd tests
   python zmq_server.py
   ```

3. Launch MEDM panel.
   ```bash
   medm -x -macro P=13ZMQ1:,R=cam1: ADBase.adl
   ```

4. Start acquisition and observe the image counter increases.

5. Now enable the plugin to publish NDArray as ZeroMQ server.
   ```bash
   caput 13ZMQ1:cam1:ArrayCallbacks 1
   caput 13ZMQ1:ZMQ1:EnableCallbacks 1
   ```

6. Launch a ZeroMQ client to receive data.
   ```bash
   cd tests
   python zmq_client.py
   ```
