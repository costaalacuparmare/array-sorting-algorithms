# USO Cheatsheet

## Laboratoare

1. [Lab 1](https://ocw.cs.pub.ro/courses/uso/laboratoare/laborator-01)
2. [Lab 2](https://ocw.cs.pub.ro/courses/uso/laboratoare/laborator-02)
3. [Lab 3](https://ocw.cs.pub.ro/courses/uso/laboratoare/laborator-03)
    - tar
    - zip
    - find
4. [Lab 4](https://ocw.cs.pub.ro/courses/uso/laboratoare/laborator-04)
5. [Lab 5](https://ocw.cs.pub.ro/courses/uso/laboratoare/laborator-05)
    - ip
6. [Lab 6](https://ocw.cs.pub.ro/courses/uso/laboratoare/laborator-06)
7. [Lab 7](https://ocw.cs.pub.ro/courses/uso/laboratoare/laborator-07)
    - ps extra
    - dd
8. [Lab 8](https://ocw.cs.pub.ro/courses/uso/laboratoare/laborator-08)
    - cut and awk
    - tr and sed
    - for and if
9. [Lab 9](https://ocw.cs.pub.ro/courses/uso/laboratoare/laborator-09)
    - users
    - permission
10. [Lab 10](https://ocw.cs.pub.ro/courses/uso/laboratoare/laborator-10)
    - encryption
    - base64
    - hashing
11. [Lab 11](https://ocw.cs.pub.ro/courses/uso/laboratoare/laborator-11)

## Commands

### Tar & Zip

```bash
tar cvf         # Add to archive
tar xvf         # Extract
tar tf          # Show contents

tar czvf        # Add to archive - compressed
tar xzvf        # Extract        - compressed (optional)
tar ztf         # Show contents  - compressed (optional)
```

```bash
zip filename.zip files      # Add to archive
zip -u filename.zip files   # Update archive
unzip filename.zip          # Extract
zip -sf                     # Show contents
```

### Ps

```bash
# Sort every process by memory, shows only user,uid,pid,%mem,%cpu,rss,cmd
ps -e -o user,uid,pid,%mem,%cpu,rss,cmd --sort=%mem
```

### Random

```bash
shuf -i 0-255 -n 1

/dev/urandom

$RANDOM
```

### Create files with zero or random

```bash
dd if=/dev/urandom of=random bs=1M count=100    # File with random characters

dd if=/dev/zero of=zeros bs=1M count=69         # File with zeros
```

### Mount files

```bash
mkfs.ext3                                   # Make an ext3 partition
mount -t ext3 filename mount/location/here  # Mount filename to that location
```

### Random strings

```bash
# 20 character long strings
echo $RANDOM | md5sum | head -c 20; echo;
tr -dc A-Za-z0-9 </dev/urandom | head -c 20 ; echo;
pwgen 20 1
```

### Word counter

```bash
wc -l file      # Line counter
wc -w file      # Word counter
wc -c file      # Character/byte counter
```

### System info

```bash
# Cpu info
lscpu
cat /proc/meminfo

# Memory info
lsmem
cat /proc/cpuinfo
free

lshw        # Hardware info
lspci       # PCI devices info
lsusb       # USB info
```

### Users & groups

```bash
sudo adduser                            # Longer more complex creation of user

sudo useradd -m gigel                   # Create user with username

sudo useradd -m -d /home/gigel gigel    # Create user with spicific home directory

sudo useradd -p $(openssl passwd -1 password) username  # User with password

w               # Show who is logged on and what they are doing
who             # Show who is logged on
whoami          # Show the users name
id              # Shows id, uid, groups of current user, or a specific user
finger gigel    # Display information about a specific user (empty means all)
passwd gigel    # Change the password of a specific user (empty means current user)

su gigel        # Switch to gigel user

sudo userdel gigel   # Deletes gigel from the computer :(

```

### Chmod and chown

```bash
#   Read    |   Write   |   Execute
#     4           2            1

#   -rwxrwxrwx

#   User    |Users group|   Everyone else
#   rwx         rwx             rwx

chmod +x / +r / +w      # Adds read, execute or write persision to everyone
chmod 666               # Adds read and write to everyone

chown gigel tema-pclp   # Switches the ownership of tema-pclp to gigel
chown user:group file 1 # Switches the ownership of tema-pclp to user from the group group
```

### SSH

```bash
ssh student@<ip-address>            # Connects to <ip-address> as student
ssh student@<ip-address> -p 2222    # Connects to <ip-address> as student on port 2222
ssh student@<ip-address>:~ ls       # Execute ls in home of student

scp /etc/passwd student@<ip-address>:~ # Copy passwd to the home directory of student
scp student@<ip-address>:/etc/passwd ~ # Copy passwd from remote to home directory
```

### Encryption

```bash
# AES CBC type encryption with 256 bit key
openssl aes-256-cbc -in file.txt -out encrypted_file.enc -pass pass:"uso rules"

# Same but to decrypt
openssl aes-256-cbc -d -in encrypted_file.enc -out decrypted_file.txt -pass pass:"uso rules"

# Hashing
md5sum
sha256sum

echo -n "apples" | md5sum
```

## Bash

### Awk

```bash
awk '{print}' /etc/passwd                   # Prints the whole file
awk -F":" '{print $1}' /etc/passwd          # Prints the users
awk -F":" '/regex/ {print $1}' /etc/passwd  # Prints the line that match that regex
awk '{print NR,$0}' /etc/passwd             # Prints every line with the line number
awk -F":" '{print $1, $NF}' /etc/passwd     # Prints the first and last column
```

### If

```bash
if condition1
then
...
elif codtion2
then
...
else
...
fi
```

### For & while loops

```bash
# For loops from 1 to 10
for i in 1 2 3 4 5 6 7 8 9 10; do ... done
for ((i = 1; i <= 10; i++)); do ... done
for i in $(seq 1 10); do ... done
for i in $(seq -f "%02g" 1 10); do ... done

for f in *; do ... done                         # For loops for every file
for arg in $@; do ... done                      # For every argument

# For every user in passwd
for user in $(awk -F ":" '{print $1}' /etc/passwd);
do 
... 
done

# For every line in countries.csv
cat countries.csv | while read line
do
...
done

# While loop
while condition
do
...
done
```

### Split string

```bash
IFS=":"     # Your separator

# Get's every argument from /etc/passwd
cat /etc/passwd | while read user password uid gid gecos home shell
do
...
done
```
