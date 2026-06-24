# 🐚 Bash / Linux Cheatsheet

> Part of [CodeCraftClub-Cheatsheets](../README.md) · Bash shell & Linux commands quick reference.

---

## Navigation & Files

```bash
pwd                     # current directory
ls -la                  # list all + details
cd /path  ;  cd ..  ;  cd ~     # change dir
mkdir -p a/b/c          # create nested dirs
touch file.txt          # create empty file
cp src dst  ;  cp -r dir/ dst/  # copy (recursive)
mv old new              # move / rename
rm file  ;  rm -rf dir/ # remove (recursive+force ⚠️)
ln -s target link       # symlink
find . -name "*.js"     # find files
find . -type f -mtime -1  # modified in last day
```

## Viewing Files

```bash
cat file.txt            # whole file
less file.txt           # paged (q to quit)
head -n 20 file         # first 20 lines
tail -n 20 file         # last 20 lines
tail -f log.txt         # follow live
wc -l file              # count lines
```

## Search (grep)

```bash
grep "error" log.txt
grep -i "error" log.txt      # case-insensitive
grep -r "TODO" .             # recursive
grep -n "func" file          # show line numbers
grep -v "debug" log.txt      # invert (exclude)
grep -E "cat|dog" file       # regex (extended)
```

## Pipes & Redirection

```bash
cmd1 | cmd2             # pipe output to input
cmd > file              # redirect stdout (overwrite)
cmd >> file             # append
cmd 2> err.log          # redirect stderr
cmd > out 2>&1          # both to one file
cmd < input.txt         # read stdin from file
ls | grep ".txt" | wc -l
```

## Permissions

```bash
chmod +x script.sh      # make executable
chmod 755 file          # rwx r-x r-x
chmod 644 file          # rw- r-- r--
chown user:group file
sudo command            # run as root
```

## Processes

```bash
ps aux                  # all processes
top  /  htop            # live monitor
kill <pid>              # terminate
kill -9 <pid>           # force kill
jobs  ;  fg  ;  bg      # job control
command &               # run in background
nohup command &         # survive logout
```

## Variables & Environment

```bash
NAME="Alan"             # no spaces around =
echo "$NAME"            # use with $
export PATH="$PATH:/new/bin"
echo $HOME $USER $PWD
$(date)                 # command substitution
read -p "Name: " name   # prompt input
```

## Shell Scripting

```bash
#!/bin/bash

name="World"
echo "Hello, $name"

# Conditionals
if [ "$1" == "start" ]; then
    echo "starting"
elif [ -f config.txt ]; then     # -f file exists, -d dir, -z empty
    echo "config found"
else
    echo "unknown"
fi

# Loops
for i in 1 2 3; do echo $i; done
for f in *.txt; do echo "$f"; done
while [ $count -lt 5 ]; do count=$((count+1)); done

# Function
greet() {
    echo "Hi, $1"
}
greet "Alan"
```

## Comparisons (test)

```bash
[ "$a" = "$b" ]    # string equal
[ "$a" != "$b" ]
[ $n -eq 5 ]       # numeric: -eq -ne -lt -le -gt -ge
[ -f file ]        # file exists
[ -d dir ]         # directory exists
[ -z "$s" ]        # string empty
[ -n "$s" ]        # string not empty
```

## Networking & System

```bash
curl https://api.com           # HTTP request
curl -X POST -d '{}' url
wget https://file.zip
ping google.com
ssh user@host
scp file user@host:/path
df -h                          # disk usage
du -sh dir/                    # folder size
free -h                        # memory
env  ;  which node             # environment / locate binary
```

## Archives

```bash
tar -czf out.tar.gz dir/       # compress
tar -xzf out.tar.gz            # extract
zip -r out.zip dir/
unzip out.zip
```

---

[🔝 Back to README](../README.md)
