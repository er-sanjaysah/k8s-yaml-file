kubectl run -i --tty load-generator --rm --image=busybox -- /bin/sh


run this busybox pod and then run this command

while true; do wget -q -O- http://34.94.65.171:30080; done
