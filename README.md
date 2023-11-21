# pull-device-config

subnet of https://github.com/kimcharli/ck-apstra-api

## python3.11 virtual environment

```
git clone git@github.com:kimcharli/pull-device-config.git
cd pull-device-config

python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt  
```

## run

### Help example
```
(.venv) ckim@ckim-mbp:pull-device-config % python get_device_configuration.py --help
Usage: get_device_configuration.py [OPTIONS]

Options:
  --server TEXT      Apstra server IP address
  --blueprint TEXT   Apstra blueprint label, or __all_pristine__
  --username TEXT    Apstra username
  --password TEXT    Apstra password
  --output-dir TEXT  Output directory
  --help             Show this message and exit.
(.venv) ckim@ckim-mbp:pull-device-config % 
```

### example command with password hidden
```
(.venv) ckim@ckim-mbp:pull-device-config % python get_device_configuration.py --username admin --output-dir configs --server local-apstra.pslab.link --blueprint terra

Password: 
==== AosServer.http_post() status: 200
system_label='spine2'
-- Reading pristine configuration
-- Reading rendered configuration
system_label='spine1'
-- Reading pristine configuration
-- Reading rendered configuration
system_label='server-leaf-2'
-- Reading pristine configuration
-- Reading rendered configuration
system_label='server-leaf-1'
-- Reading pristine configuration
-- Reading rendered configuration
system_label='border-2'
-- Reading pristine configuration
-- Reading rendered configuration
system_label='border-1'
-- Reading pristine configuration
-- Reading rendered configuration
(.venv) ckim@ckim-mbp:pull-device-config % 
...

```

### produced files
The folder configs/blueprint-name will have the configuration files
```
(.venv) ckim@ckim-mbp:pull-device-config % ls -l configs/terra
total 408
-rw-r--r--@ 1 ckim  staff    842 Nov 21 13:37 border-1-configlet.txt
-rw-r--r--@ 1 ckim  staff   7067 Nov 21 13:37 border-1-pristine.txt
-rw-r--r--@ 1 ckim  staff  30414 Nov 21 13:37 border-1.txt
-rw-r--r--@ 1 ckim  staff    842 Nov 21 13:37 border-2-configlet.txt
-rw-r--r--@ 1 ckim  staff   6221 Nov 21 13:37 border-2-pristine.txt
-rw-r--r--@ 1 ckim  staff  30405 Nov 21 13:37 border-2.txt
-rw-r--r--@ 1 ckim  staff    842 Nov 21 13:37 server-leaf-1-configlet.txt
-rw-r--r--@ 1 ckim  staff   6828 Nov 21 13:37 server-leaf-1-pristine.txt
-rw-r--r--@ 1 ckim  staff  20962 Nov 21 13:37 server-leaf-1.txt
-rw-r--r--@ 1 ckim  staff    842 Nov 21 13:37 server-leaf-2-configlet.txt
-rw-r--r--@ 1 ckim  staff   6828 Nov 21 13:37 server-leaf-2-pristine.txt
-rw-r--r--@ 1 ckim  staff  20303 Nov 21 13:37 server-leaf-2.txt
-rw-r--r--@ 1 ckim  staff    842 Nov 21 13:37 spine1-configlet.txt
-rw-r--r--@ 1 ckim  staff   5817 Nov 21 13:37 spine1-pristine.txt
-rw-r--r--@ 1 ckim  staff  11032 Nov 21 13:37 spine1.txt
-rw-r--r--@ 1 ckim  staff    842 Nov 21 13:37 spine2-configlet.txt
-rw-r--r--@ 1 ckim  staff   5639 Nov 21 13:37 spine2-pristine.txt
-rw-r--r--@ 1 ckim  staff  11041 Nov 21 13:37 spine2.txt
(.venv) ckim@ckim-mbp:pull-device-config %
```

### __all_pristine__

Put __all_pristine__ for the blueprint to get all the pristine configurations irrespective of blueprint.

```
(.venv) ckim@ckim-mbp:pull-device-config % python get_device_configuration.py --username admin --output-dir configs --server local-apstra.pslab.link --blueprint __all_pristine__

...

(.venv) ckim@ckim-mbp:pull-device-config % ls -l configs/pristine                  
total 96
-rw-r--r--@ 1 ckim  staff  5817 Nov 21 14:09 EK069-pristine.txt
-rw-r--r--@ 1 ckim  staff  5639 Nov 21 14:09 FQ824-pristine.txt
-rw-r--r--@ 1 ckim  staff  6828 Nov 21 14:09 TA3717380176-pristine.txt
-rw-r--r--@ 1 ckim  staff  6828 Nov 21 14:09 TA3717380282-pristine.txt
-rw-r--r--@ 1 ckim  staff  7067 Nov 21 14:09 XH3119430106-pristine.txt
-rw-r--r--@ 1 ckim  staff  6221 Nov 21 14:09 XH3119430153-pristine.txt
(.venv) ckim@ckim-mbp:pull-device-config % 
```


## use it

1. copy over the configuration file to the device
2. go to edit mode in the device
3. load override spine1-pristine.txt
4. load merge spine1.txt
5. load merget spine1-configlet.txt
6. load set spine1-configet-set.txt
7. show | compare
8. rollback
9. 

Example
```
root@prod-swl2-01-b> configure 
Entering configuration mode
Users currently editing the configuration:
  root terminal u0 (pid 32232) on since 2023-11-17 23:20:27 UTC, idle 17:25:47
      {master:0}[edit]

{master:0}[edit]
root@prod-swl2-01-b# load override border-1-pristine.txt 
load complete

{master:0}[edit]
root@prod-swl2-01-b# load merge border-1.txt 
load complete

{master:0}[edit]
root@prod-swl2-01-b# load merge border-1-configlet.txt 
load complete

{master:0}[edit]
root@prod-swl2-01-b# load set border-1-configlet-set.txt 
load complete

{master:0}[edit]
root@prod-swl2-01-b# show |compare 

...

{master:0}[edit]
root@prod-swl2-01-b# rollback 
load complete

{master:0}[edit]
root@prod-swl2-01-b# exit 
Exiting configuration mode

{master:0}
root@prod-swl2-01-b> 
```

## stop environment

```
deactivate
```
