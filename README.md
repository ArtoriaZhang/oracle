# oracle
Installation doc


#### Got error when installing oracle 19c:

```
[main] [ 2021-12-03 06:23:13.894 UTC ] [ConfigureListener.isPortFree:1303]  No IP address returned for host. xxx
oracle.net.ca.IllegalEndpointException: No valid IP Address returned for the host xxx
        at oracle.net.ca.ConfigureListener.isPortFree(ConfigureListener.java:1304)
        at oracle.net.ca.ConfigureListener.validatePort(ConfigureListener.java:1193)
        at oracle.net.ca.ConfigureListener.validateEndPoint(ConfigureListener.java:1177)
        at oracle.net.ca.ConfigureListener.typicalConfigure(ConfigureListener.java:298)
        at oracle.net.ca.SilentConfigure.performSilentConfigure(SilentConfigure.java:212)
        at oracle.net.ca.InitialSetup.<init>(NetCA.java:4325)
        at oracle.net.ca.NetCA.main(NetCA.java:460)
```
My solution:
  1. Edit listener.ora file;
  2. Change listener address 'localhost' to '127.0.0.1'. Like belowe:
```
LISTENER =
  (DESCRIPTION_LIST =
    (DESCRIPTION =
      (ADDRESS = (PROTOCOL = TCP)(HOST = 127.0.0.1)(PORT = 1521))
      (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
    )
  )
```
I faced this error long time ago, but didn't find any working solution. So I decided to install oracle 19c step-by-step(Use RPM installation method before).
After installing oracle software, I tried to create a new database instance. But I got this error again.

Why I figured out this time? It's accidental.
I copied the sample listener.ora, and uncommented the config.
There was a config like: 
```
       (SID_DESC=
                       #BEQUEATH CONFIG
          (GLOBAL_DBNAME=salesdb.mycompany)
          (SID_NAME=sid1)
          (ORACLE_HOME=/private/app/oracle/product/8.0.3)
                       #PRESPAWN CONFIG
         (PRESPAWN_MAX=20)
         (PRESPAWN_LIST=
           (PRESPAWN_DESC=(PROTOCOL=tcp)(POOL_SIZE=2)(TIMEOUT=1))
         )
```
"PRESPAWN_MAX=20" noticed me, because I also found NETCA error log show 20 times "No valid IP Address returned for the host xxx".
So...


