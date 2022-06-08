---
Engineer Man
---

### Ssh reverse proxy?
https://www.youtube.com/watch?v=mV_8GbzwZMM&list=RDCMUCrUL8K81R4VBzm-KOYwrcxQ&start_radio=1

### Collection of videos
https://www.youtube.com/watch?v=mV_8GbzwZMM&list=RDCMUCrUL8K81R4VBzm-KOYwrcxQ&start_radio=1


---
Brad Traversy
---

### Docker sheat cheet
https://gist.github.com/bradtraversy/89fad226dc058a41b596d586022a9bd3

---
MIXED SOURCES
---

### Regex visualizator
https://www.debuggex.com/

### Detect, sweep and scan your network in bash
https://www.youtube.com/watch?v=qhCxKrU1AEY

### Ssh reverse tunneling
https://jfrog.com/connect/post/reverse-ssh-tunneling-from-start-to-end/

### Cut log files
https://www.youtube.com/watch?v=L2BFDyYknIg


### My notes
Remove all files starting with a digit

```
rm [0-9]*
```

Remove all files with certain letter combinations

```
rm x[abcdefghijklmnop]*
```

Loop through all files in directory, read the first word in the first line of the file, and save the file as this word

```
for f in *; do cat "$f" | tee $(head -n 1 $f | cut -d " " -f 1); done
```
