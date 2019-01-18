# Boot and install a Jetstream foo

**note** I think we should still put conda install commands at the top of each tutorial :)

## Log in as tx160085, diblions, or dibtiger

Here: https://use.jetstream-cloud.org/application/

## Create instance

Use m1.medium, Ubuntu 18.04 LTS "Devel and Docker".

Log in etc.

## Install miniconda

Install miniconda, [per instructions](https://bioconda.github.io/#using-bioconda)
```
curl -O -L https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh
```
say "yes" to everything and accept default locations.

Log out and log back in to update settings.

Add bioconda channels:
```
conda config --add channels defaults
conda config --add channels conda-forge
conda config --add channels bioconda
```

and now you can install packages like so: `conda install sourmash`.

## Install RStudio

```
sudo apt-get update && sudo apt-get -y install gdebi-core r-base
```

then
```
wget https://download2.rstudio.org/rstudio-server-1.1.453-amd64.deb
sudo gdebi -n rstudio-server-1.1.453-amd64.deb

```

You'll need to set your account password using
```
sudo passwd $USER
```

To get your Web address for the machine, run:

```
echo http://$(hostname):8787/
```
paste the resulting string into your Web browser,
go there, and log in!

## A lesson!

Install blast:

```
conda install blast
```

and now you should be able to go through https://github.com/ngs-docs/2018-ggg-rstudio-bioinformatics-ws/blob/master/running-command-line-blast.md and https://github.com/ngs-docs/2018-ggg-rstudio-bioinformatics-ws/blob/master/visualizing-blast-scores-with-RStudio.md with only minor modifications.

## Appendix: install miniconda in a central location

run as root

put in /opt/miniconda. make rw (and directory rwx) --

```
find /opt/miniconda -exec chmod a+rw {} \;
find /opt/miniconda -type d -exec chmod a+rwx {} \;
```

add `export PATH="/opt/miniconda/bin:$PATH"` to bottom of `/etc/bash.bashrc`

you might also do (as root)

```
echo '5 * * * * /usr/bin/killall fail2ban-server' | crontab
```
to get rid of fail2ban

also probably want to run `apt-get update`, and install R Studio.

###

To upload and download data to home from the new webshell, Contl-Shift-Command. Control-Shift-Windows.