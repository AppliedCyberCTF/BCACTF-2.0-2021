# BCACTF 2.0 – Infinite Zip

* **Category:** Foren
* **Points:** 75
* **Author:** [Vivian](https://github.com/vivian-dai)

## Challenge

> Here's a zip, there's a zip. Zip zip everywhere.

## Solution

Open the zip file with [7zip](https://www.7-zip.org/) and realize there are more zip files inside. Hold down Ctrl + PgDn for a while before extracting `flag.png`
![flag](https://user-images.githubusercontent.com/38384400/121815565-1ca02880-cc45-11eb-90fc-a2fd53f3389e.png)
This is a false flag.  
If we run `strings flag.png` or [view the exif data](https://www.exifdata.com/) we'll find the flag.
![image](https://user-images.githubusercontent.com/38384400/121815643-7acd0b80-cc45-11eb-8543-142038d1936f.png)