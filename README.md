A dead simple git smart-http server using nginx as a frontend. 

Usage in RHEL 7 with SELinux:

```
setenforce 0

git clone https://github.com/ejlp12/git-http-backend.git

cd git-http-backend
mkdir -p /home/git/repo

sudo sh -c "echo -n 'eryan:' >> /home/git/repo/.htpasswd"
sudo sh -c "openssl passwd -apr1 >> /home/git/repo/.htpasswd"
git init --bare --shared=group /home/git/repo/test.git 

docker build -t ejlp12/git-http-backend .
docker run -d -p 4080:80 -v /home/git/repo:/git ejlp12/git-http-backend

GIT_CURL_VERBOSE=1 git clone http://localhost:4080/test.git
```

You can also use `.htpasswd` file provided in this repo, the username is `eryan` password is `P@ssw0rd!`

This repo is based on `https://github.com/ynohat/git-http-backend.git`

## NOTE:

This docker is based on alpine linux, in some machine I experienced problem to connect to repository when it run `apk add` but the problem is not occured in other machine. So if you experince the same, try to build in other machine.

Error shown when build:
```
fetch http://dl-cdn.alpinelinux.org/alpine/v3.4/main/x86_64/APKINDEX.tar.gz
ERROR: http://dl-cdn.alpinelinux.org/alpine/v3.4/main: temporary error (try again later)
```

No fix for this problem yet. https://github.com/gliderlabs/docker-alpine/issues/279#issuecomment-300859312
