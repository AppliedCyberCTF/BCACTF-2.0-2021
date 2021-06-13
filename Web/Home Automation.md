# BCACTF 2.0 – Home Automation

* **Category:** Web
* **Points:** 75
* **Author:** [Vivian](https://github.com/vivian-dai)

## Challenge

> Check out my super secure home automation system! No, don't try turning the lights off...

## Solution

If we go to the website, there's a "Log in as guest" button. If we click that, we are greeted with this:  
![image](https://user-images.githubusercontent.com/38384400/121814967-e6ad7500-cc41-11eb-938a-b12291e2b03a.png)  
Attempting to turn the lights on results in the message "You must be admin to turn off the lights. Currently you are "vampire"."  
If we inspect the website and navigate to cookies we see our user is a vampire:  
![image](https://user-images.githubusercontent.com/38384400/121815018-178daa00-cc42-11eb-903e-eac0157ed373.png)  
Change "vampire" to "admin", refresh the page, then turn the lights on again.  
Flag get! `bcactf{c00k13s_s3rved_fr3sh_fr0m_th3_smart_0ven_cD7EE09kQ}` Now to turn this light back on...
