# BCACTF 2.0 – Agent Gerald

* **Category:** Web
* **Points:** 125
* **Author:** [Vivian](https://github.com/vivian-dai)

## Challenge

> Agent Gerald is a spy in SI-6 (Stegosaurus Intelligence-6). We need you to infiltrate this top-secret SI-6 webpage, but it looks like it can only be accessed by Agent Gerald's special browser...

## Solution

If we go to the website, it doesn't let us do anything because we aren't Agent Gerald. We can use [`curl`](https://curl.se) to set the [user agent](https://curl.se/docs/manpage.html#-A) to "gerald"  
`curl --user-agent "gerald" http://web.bcactf.com:49156` will get us:  
```html
<!DOCTYPE html>
        <html>
            <head>
            </head>
            <body>
                <h1>Welcome to the Stegosaurus Intelligence-6 Homepage</h1>
                <h2>Are you Agent Gerald?</h2>
                <img src="gerald.PNG" alt="agent gerald" style="width: 50%"></img>
                   <h4> Welcome, Agent Gerald! Your flag is: bcactf{y0u_h@ck3d_5tegos@urus_1nt3lligence} </h4>
            </body>
        </html>
```