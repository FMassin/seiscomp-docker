# seiscomp-docker
SeisComP in a docker container

## Mac install

### XQuartz install

Install the latest version of XQuartz from : https://www.xquartz.org/

### Homebrew install

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### Docker install

```bash
brew install docker --cask
brew install docker-buildx
```

Start Docker Desktop (Applications folder) : it will start the docker daemon.

## Building and running Docker container

### Build image and create container

#### Update configuration
The file `compose.yaml` contains instructions to mount host directories on the container.

Typically, when using SeiSComP docker, you would need :
 * a directory containing system `.cfg` configuration files for each application
 * a directory containing map tiles
 * directories containing travel time files for locators

By default:
 * Permanent user configuration files and data are on a `seiscompsysop` docker volume on the host computer. They are mounted on `/home/sysop` in the container (read and write).
 * system configuration files are expected on `$SEISCOMP_ROOT/etc/` on the host computer. They are mounted on `/opt/seiscomp/etc/` in the container (read only).
By default, map tiles directory is expected on `$SEISCOMP_ROOT/share/maps` on the host computer. It is mounted on `/opt/seiscomp/share/maps` in the container (read only).
 * NonLinLoc configuration and time travel files are expected on `$SEISCOMP_ROOT/share/nll` on the host computer. It is mounted on `/opt/seiscomp/share/nll` in the container (read only).
 * Hypo71PC configuration files and velocity models are expected to be found on `$SEISCOMP_ROOT/share/hypo71` on the host computer. It is mounted on `/opt/seiscomp/share/hypo71` in the container (read only).

You can update the host path for each volume or comment the unused ones in the `compose.yaml` file, section `volumes`.

#### Create image and container

```bash
docker compose create
```

### Run the container

```bash
docker compose start
```

### Stop the container

```bash
docker compose stop
```

## Running SeisComP GUI app

### Connect to container with SSH

```bash
ssh -Y -p 2222 sysop@localhost
```
### Start scolv (same applies for any other SeisComP GUI)

Without option and with the default container configutation, scolv will use `$HOME/seiscomp/etc/scolv.cfg` file from the host.
```bash
scolv
```

Scolv can be launched to point to a specific configuration file from `$HOME/seiscomp/etc/` directory on the host.
```bash
scolv --config-file $HOME/.seiscomp/my_scolv_config_file.cfg
```

### List of SeisComP GUI
* scolv : locator interface
* scrttv : waveform display interface
* scmv : station map interface
* scesv : event interface
* scqcv : station quality check interface

## Credits

This project was inspired by [Patrice Boissier](https://github.com/PBoissier). Many thanks!!
