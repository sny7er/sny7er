
### SSH Reverse Tunnel

```bash

#!/bin/bash

while [[ true  ]] ; do

        echo Starting tunnel on `date` >> tunnel.log

        ssh -N -R 192.168.253.101:3300:10.60.1.110:3301 -p 6022 -l tunnel demo.blah.com

        echo Tunnel exited on `date` >> tunnel.log

        sleep 30

done
```





