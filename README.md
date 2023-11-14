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

help example
```
% python3 get_device_configuration.py --help
Usage: get_device_configuration.py [OPTIONS]

Options:
  --server TEXT      Apstra server IP address
  --blueprint TEXT   Apstra blueprint label
  --username TEXT    Apstra username
  --password TEXT    Apstra password
  --output-dir TEXT  Output directory
  --help             Show this message and exit.
```

example command with password hidden
```
python get_device_configuration.py --username ckim --output-dir configs --server <apstra-ip> --blueprint <blueprint-name>                            
Password:
==== AosServer.http_post() status: 200
system_label='spine1'
-- Reading rendered configuration
...

```

The folder configs/blueprint-name will have the configuration files
```
- <blueprint-name>-pristine.txt       :in case the device was deployed
- <blueprint-name>-rendered.txt
- <blueprint-name>.txt                :intended configuration
- <blueprint-name>-configlet.txt
- <blueprint-name>-configlet-set.txt
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

## stop environment

```
deactivate
```
