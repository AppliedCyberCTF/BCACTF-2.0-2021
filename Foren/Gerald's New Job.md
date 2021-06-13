# BCACTF 2.0 – Gerald's New Job

* **Category:** Foren
* **Points:** 100
* **Author:** [Vivian](https://github.com/vivian-dai)

## Challenge

> Being a secret agent didn't exactly work out for Gerald. He's been training to become a translator for dinosaurs going on vacation, and he just got his traslator's licence. But when he sees it, it doesn't seem to belong to him... can you help him find his licence?

## Solution

The PDF contains a false flag. If we perform a [binwalk](https://tools.kali.org/forensics/binwalk) analysis on the file (`binwalk gerald.pdf`):
![image](https://user-images.githubusercontent.com/38384400/121815864-7b19d680-cc46-11eb-922e-89d80d2dfaff.png)
We see there are things hidden inside it. We can extract by doing `binwalk -e gerald.pdf`  
Inside the folder `_gerald.pdf.extracted` there's a `GeraldFlag.png`
![GeraldFlag](https://user-images.githubusercontent.com/38384400/121815920-b4eadd00-cc46-11eb-8f8c-46692b821b79.png)