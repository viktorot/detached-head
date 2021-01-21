---
title: "Objective 2 - Investigate S3 Bucket"
date: 2021-01-20T00:00:00Z
draft: false
hide_on_homepage: false
---

The gondola takes us to Santa's new castle. We arrive in front and take a short walk around. In front of the entrance an elf by the name of `Shinny Upatree` waves to us. Next to him there's a kiosk with a terminal. Activating the terminal, we get the following prompt:

```md
Can you help me? Santa has been experimenting with new wrapping technology, and we’ve run into a ribbon-curling nightmare! 
We store our essential data assets in the cloud, and what a joy it’s been!
Except I don’t remember where, and the **Wrapper3000** is on the fritz! 
Can you find the missing package, and unwrap it all the way?

Hints: Use the file command to identify a file type. You can also examine tool help using the man command. 
Search all man pages for a string such as a file extension using the apropos command.
```

# Finding the bucket

On the terminal we find a folder named `bucket_finder`, which contains a Ruby script for finding public AWS S3 buckets. Alongside the script we also find a sample wordlist file with the contents:

```
kringlecastle
wrapper
santa
```

Running the script with the wordlist file tells us that there is a bucket with the name `santa`, but unfortunately we cannot access it's contents. Here's the output:

```md
$ ./bucket_finder.rb wordlist

http://s3.amazonaws.com/kringlecastle
Bucket found but access denied: kringlecastle
http://s3.amazonaws.com/wrapper
http://s3.amazonaws.com/santa
Bucket santa redirects to: santa.s3.amazonaws.com
http://santa.s3.amazonaws.com/
**Bucket found but access denied: santa**
```

If we take a closer look at the prompt we got when we opened the terminal, we see that it mentions something about a `Wrapper3000` that is somehow malfunctioning. Taking that information, we update the wordfile and add a new entry with the string `wrapper3000`. Re-running the script we get the following:

```md
$ ./bucket_finder.rb wordlist

http://s3.amazonaws.com/wrapper3000
Bucket Found: wrapper3000 ( http://s3.amazonaws.com/wrapper3000 )
**<Public> http://s3.amazonaws.com/wrapper3000/package**
```

Bingo!

# Opening the bucket

Now, lets see what can we find inside. The scripts has an option to download bucket contents, so lets use it:

```
$ ./bucket_finder.rb --download wordlist
```

# Unwrapping gifts

This creates a folder `wrapper3000` with a file `package` with the content:

```
UEsDBAoAAAAAAIAwhFEbRT8anwEAAJ8BAAAcABwAcGFja2FnZS50eHQuWi54ei54eGQudGFyLmJ6MlVUCQADoBfKX6AXyl91eAsAAQT2AQAABBQAAABCWmg5MUFZJlNZ2ktivwABHv+Q3hASgGSn//AvBxDwf/xe0gQAAAgwAVmkYRTKe1PVM9U0ekMg2poAAAGgPUPUGqehhCMSgaBoAD1NNAAAAyEmJpR5QGg0bSPU/VA0eo9IaHqBkxw2YZK2NUASOegDIzwMXMHBCFACgIEvQ2Jrg8V50tDjh61Pt3Q8CmgpFFunc1Ipui+SqsYB04M/gWKKc0Vs2DXkzeJmiktINqjo3JjKAA4dLgLtPN15oADLe80tnfLGXhIWaJMiEeSX992uxodRJ6EAzIFzqSbWtnNqCTEDML9AK7HHSzyyBYKwCFBVJh17T636a6YgyjX0eE0IsCbjcBkRPgkKz6q0okb1sWicMaky2Mgsqw2nUm5ayPHUeIktnBIvkiUWxYEiRs5nFOM8MTk8SitV7lcxOKst2QedSxZ851ceDQexsLsJ3C89Z/gQ6Xn6KBKqFsKyTkaqO+1FgmImtHKoJkMctd2B9JkcwvMr+hWIEcIQjAZGhSKYNPxHJFqJ3t32Vjgn/OGdQJiIHv4u5IpwoSG0lsV+UEsBAh4DCgAAAAAAgDCEURtFPxqfAQAAnwEAABwAGAAAAAAAAAAAAKSBAAAAAHBhY2thZ2UudHh0LloueHoueHhkLnRhci5iejJVVAUAA6AXyl91eAsAAQT2AQAABBQAAABQSwUGAAAAAAEAAQBiAAAA9QEAAAAA
```

This is base64, so we can decode it and see what has been encoded.

```
$ cat package | base64 -d >> decoded
```

This creates a new `decoded` on the terminal. Running `file` on it, we see it's is a zip archive, so we unzip it.

```
$ file decoded
decoded: Zip archive data, at least v1.0 to extract

$ unzip decoded
Archive:  decoded
 extracting: package.txt.Z.xz.xxd.tar.bz2
```

The file that was extracted from the archive has a lot of extensions, meaning it probably has been compressed/re-packaged multiple times. The first extension is `.bz2`. We continue with the unpacking:

```
$ bunzip2  package.txt.Z.xz.xxd.tar.bz2
```

which creates a new file `package.txt.Z.xz.xxd.tar`. We continue with:

```
$ tar xvf package.txt.Z.xz.xxd.tar
```

For the next step we have a file with the `xxd` extension. [xxd](https://linux.die.net/man/1/xxd) is a program that dumps the contents of a given file in hex. The file we have to process is a hexdump that we have to re-package. Fortunately the tool that creates hexdumps also allows is to reverse them.

``` 
$ xxd -r package.txt.Z.xz.xxd >> unhexed 
```

Now we have a new file `unhexed`. Checking the filetype, we see that it is an `xz` compressed file, which is also hinted by the next extension from of file in the previous step.

```
$ file unhexed

unhexed: XZ compressed data
```

We can un-compress it with:

```
cat package.txt.Z.xz | xz -d > package.txt.Z
```

We're almost at the finish line. From the `xz` compressed file we get a file with a `.Z` extension. A little bit of googling tells us that it's a `gzip` file. The search give us (zcat)[https://linux.die.net/man/1/zcat] that can un-compress it read it's contents. Running it gives us the solution:

```md
zcat package.txt.Z
**North Pole: The Frostiest Place on Earth**
```

------------------

You can find the solutions to the other challenges [here](/post/kringle20/intro/).