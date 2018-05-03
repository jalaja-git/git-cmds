#### How to do git stash.

Run "git log" and pick the one of the hash
git rebase -i <hash>
Retain "Pick" you want to keep
Mark "f" to purge the following commits
git push origin +master


#### How to do multiple PR
1. git clone <repo> - by default this will be your master branch
2. Create a new branch- "git checkout br2" - br2 is my new branch name
3. do your work. commit, "git commit -m 'comments'"
4. Push - git push origin/br2
5. To go back to master branch, do "git checkout master"
6. From master, to go back to br2, run "git checkout br2"


#### How to Pull a PR that is not merged into your local WS

Locate the section for your github remote in the `.git/config` file. It looks like this:

```
[remote "origin"]
	fetch = +refs/heads/*:refs/remotes/origin/*
	url = git@github.com:joyent/node.git
```

Now add the line `fetch = +refs/pull/*/head:refs/remotes/origin/pr/*` to this section. Obviously, change the github url to match your project's URL. It ends up looking like this:

```
[remote "origin"]
	fetch = +refs/heads/*:refs/remotes/origin/*
	url = git@github.com:joyent/node.git
	fetch = +refs/pull/*/head:refs/remotes/origin/pr/*
```

Now fetch all the pull requests:

```
$ git fetch origin
From github.com:joyent/node
 * [new ref]         refs/pull/1000/head -> origin/pr/1000
 * [new ref]         refs/pull/1002/head -> origin/pr/1002
 * [new ref]         refs/pull/1004/head -> origin/pr/1004
 * [new ref]         refs/pull/1009/head -> origin/pr/1009
...
```

To check out a particular pull request:

```
$ git checkout pr/999
Branch pr/999 set up to track remote branch pr/999 from origin.
Switched to a new branch 'pr/999'
```


#### How to add remote upstream
```
git clone https://cto-github.cisco.com/jganapat/learn-go.git

git remote -v
git remote add upstream https://cto-github.cisco.com/atom-hacks/learn-go.git
git remote -v
git fetch upstream
git status
git rebase upstream/master

if you hit merge conflicts
git reset --hard upstream/master


g st
g commit -s -m "comments.."
git add
g push origin master

```


#### Another way to update your fork to parent
https://stackoverflow.com/questions/10773518/merge-conflicts-updating-from-upstream
https://help.github.com/articles/syncing-a-fork/



```
814  rm -rf atomix
  815  git clone https://cto-github.cisco.com/jganapat/atomix.git
  816  cd atomix
  817  git remote add upstream https://cto-github.cisco.com/atom/atomix.git
  818  git log
  819  git fetch upstream
  820  git branch
  821  git checkout master
  822  git merge upstream/master
  823  vi releases/apic-v1.txt 
  824  git log
  825  git reset --hard upstream/master
  826  git log
  827  git push origin +master
  828  git log
  829  vi releases/apic-v1.txt 
  830  git status -uno
  831  git add releases/apic-v1.txt 
  832  git commit -s -m 'enable elastic auto-start after boot'
  833  git log
  834  git push origin +master
  835  git log


```

#### How to remove a last commit
https://sethrobertson.github.io/GitFixUm/fixup.html
git reset --hard HEAD^

```
  800  rm -rf atomix
  801  git clone https://cto-github.cisco.com/jganapat/atomix.git
  802  cd atomix
  803  git remote add upstream https://cto-github.cisco.com/atom/atomix.git
  804  git remote -v
  805  git log
  806  git reset --hard HEAD^
  807  git log
  808  git status -uno
  809  git push origin master
  810  git log
  811  git push origin +master
  812  git log
  ```
  ```
  how to upload something into our artifactory
  ~/go/centos>curl -u atom-docker-deployer:pass -X PUT "http://engci-maven-master.cisco.com/artifactory/atom-release/elastic/elasticsearch-5.6.4.rpm" --ftp-create-dirs -T elasticsearch-5.6.4.rpm
{
  "repo" : "atom-release",
  "path" : "/elastic/elasticsearch-5.6.4.rpm",
  "created" : "2018-01-23T14:37:14.103-08:00",
  "createdBy" : "atom-docker-deployer",
  "downloadUri" : "http://engci-maven.cisco.com/artifactory/atom-release/elastic/elasticsearch-5.6.4.rpm",
  "mimeType" : "application/x-rpm",
  "size" : "33689931",
  "checksums" : {
    "sha1" : "9491f1b2aff9077c03fca76ff729846c87f0f599",
    "md5" : "6a17be16006af1731b3adda316d52f3c"
  },
  "originalChecksums" : {
  },
  "uri" : "http://engci-maven.cisco.com/artifactory/atom-release/elastic/elasticsearch-5.6.4.rpm"
}~/go/centos>
```


````
How to ammend to the existing commit and reflect in PR
  233  git add elasticsearch.yml.apic sgadmin_demo.sh start_es.sh
  234  git commit -s -m 'Pick hostcerts for TLS'
  235  git log
  236  git push origin master
  237  vi start_es.sh ==== Modified start_es.sh
  243  git add start_es.sh
  244  git commit --amend
  245  git status
  246  git push origin +master

root@j7:~/atomix-hostcert/atomix/ocibuilds/es#            

````

````
Push Elastic image for APIC to pick 
/local/dme/tools/kron - Makefile is present here

[root@dev-infra4-ifc1 jalaja]# docker pull dockerhub.cisco.com/atom-docker/jalaja/elastic:0.1.8
Trying to pull repository dockerhub.cisco.com/atom-docker/jalaja/elastic ...
sha256:e24cbddfb4f15a8a150a2f0e231cc5f2b8be55b3c801c217c1bcb4ff256b9f64: Pulling from dockerhub.cisco.com/atom-docker/jalaja/elastic
d9aaf4d82f24: Pull complete
33d89168d60d: Pull complete
52169ba72e4a: Pull complete
7c148f3837a8: Pull complete
Digest: sha256:e24cbddfb4f15a8a150a2f0e231cc5f2b8be55b3c801c217c1bcb4ff256b9f64
Status: Downloaded newer image for dockerhub.cisco.com/atom-docker/jalaja/elastic:0.1.8
[root@dev-infra4-ifc1 jalaja]# docker save elastic-0.1.8.tar dockerhub.cisco.com/atom-docker/jalaja/elastic:0.1.8
Cowardly refusing to save to a terminal. Use the -o flag or redirect.
[root@dev-infra4-ifc1 jalaja]# docker save -o elastic-0.1.8.tar dockerhub.cisco.com/atom-docker/jalaja/elastic:0.1.8
[root@dev-infra4-ifc1 jalaja]# ls -ltr
total 956444
-rw------- 1 root root 979395584 Mar  5 11:30 elastic-0.1.8.tar
[root@dev-infra4-ifc1 jalaja]# gzip elastic-0.1.8.tar
[root@dev-infra4-ifc1 jalaja]# ls -ltr
total 375372
-rw------- 1 root root 384379607 Mar  5 11:30 elastic-0.1.8.tar.gz
[root@dev-infra4-ifc1 jalaja]# curl -u atom-docker-deployer:pass -X PUT https://dockerhub.cisco.com/atom-docker/apic-elastic/elastic-0.1.8.tar.gz --ftp-create-dirs -T elastic-0.1.8.tar.gz
{
  "repo" : "atom-docker-local",
  "path" : "/apic-elastic/elastic-0.1.8.tar.gz",
  "created" : "2018-02-22T18:54:55.122-08:00",
  "createdBy" : "atom-docker-deployer",
  "downloadUri" : "https://dockerhub.cisco.com/atom-docker-local/apic-elastic/elastic-0.1.8.tar.gz",
  "mimeType" : "application/x-gzip",
  "size" : "384379607",
  "checksums" : {
    "sha1" : "bfed7783db94cd2ffa5d4d467a9104244322f106",
    "md5" : "b876f76efbb26da730d46c4c88cf52a7"
  },
  "originalChecksums" : {
  },
  "uri" : "https://dockerhub.cisco.com/atom-docker-local/apic-elastic/elastic-0.1.8.tar.gz"
}[root@dev-infra4-ifc1 jalaja]#                  
````
