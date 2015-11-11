[docker hub build status](https://hub.docker.com/r/brownman/linno_pro/builds/)


linnovate project magic (aka: linno_pro )
======

### requirements: docker

```bash
sudo apt-get purge docker docker.io
wget -qO- https://get.docker.com/ | sh
wget -qO- https://get.docker.com/gpg | sudo apt-key add -
ln -sf /usr/bin/docker /usr/local/bin/docker
sudo usermod -aG docker $LOGNAME 
docker -v | grep 'Docker version 1.8' || ( echo 1>&2 your docker is not updated !; )
```


### how to run ?

 Option1 
```

cd /tmp
test -d linno_pro || { git clone https://github.com/brownman/linno_pro.git; }
cd linno_pro
git pull
chmod +x outside.sh
docker pull brownman/linno_pro
./outside.sh run
```

 Option2 (copy+paste)

```bash
cmd_bash="${cmd_bash:-./inside.sh}"
cmd_inside="bash -c $cmd_bash"

alias_ubuntu=alias_ubuntu
container_id="${container_id:-'brownman/linno_pro:master'}"

volume_apparmor='-v /usr/lib/x86_64-linux-gnu/libapparmor.so.1:/usr/lib/x86_64-linux-gnu/libapparmor.so.1'
volume_ssh="-v $HOME/.ssh:/root/.ssh"
volume_socket='-v /var/run/docker.sock:/var/run/docker.sock'
volume_bin='-v /usr/bin/docker:/usr/bin/docker'



docker stop $alias_ubuntu 2>/dev/null
docker rm $alias_ubuntu 2>/dev/null

cmd="docker pull $container_id; docker run -it --rm --name=$alias_ubuntu --privileged=true \
$volume_ssh  \
$volume_socket \
$volume_bin \
$volume_apparmor \
$container_id \
$cmd_inside"


echo $cmd
eval "$cmd"
```


how to build image mysql:
---------

```bash
container_id=some_name
docker build -t $container_id .
```


--------


####  Troobleshoot


git/rsync can't clone:

```bash
chmod +x mail_my_key.sh
bash -c mail_my_key.sh
```
