# BCACTF 2.0 – Wasm Protected Site 1

* **Category:** Web
* **Points:** 100
* **Author:** [Vivian](https://github.com/vivian-dai)

## Challenge

> Check out my super safe website! Enter the password to get the flag

## Solution

If we go to the website and inspect, we'll see there's a `main.js` file. Appending `/main.js` to the end of the URL will get us to the site's JavaScript source:
```js
if (typeof window.WebAssembly === 'undefined') {
    alert('WebAssembly seems to not be implemented on your browser. Try using an updated version of Firefox, Chrome, or Safari. ');
    throw 'no wasm :(';
}

const fetchWASMCode = () => {
    return new Promise((res, rej) => {
        const req = new XMLHttpRequest();

        req.onload = function () {
            res(req.response);
        }
        req.onerror = (err) => {
            console.warn('If you\'re seeing this logged, something broke');
            rej(err)
        }
        req.open("GET", "./code.wasm");
        req.responseType = "arraybuffer";
        req.send();
    });
};

let wasm = null;
fetchWASMCode().then(buffer => {
    WebAssembly.instantiate(buffer).then(res => {
        wasm = res;
    }).catch(err => {
        console.warn('If you\'re seeing this logged, something broke');
        throw err;
    })
});


const input = document.querySelector('input#password');
const response = document.querySelector('p#response-text');

document.querySelector('button').addEventListener('click', () => {
    if (wasm) {
        const memory = new Uint8Array(wasm.instance.exports.memory.buffer);
        memory.set(new TextEncoder().encode(input.value + "\x00"));

        const resultAddr = wasm.instance.exports.checkPassword(0);

        const end = memory.indexOf(0, resultAddr);

        response.innerText = "Response: " + new TextDecoder().decode(memory.subarray(resultAddr, end));
    } else {
        response.innerText = "Please try again in a few seconds";
    }
}, 1);
```
`req.open("GET", "./code.wasm");` is important so we append `/code.wasm` to the end of the URL and get:
```wasm
 asm   ``  *memory 
compareString  
checkPassword 
Q8@   j-    j-  G@A    j-  @ Aj!AA  @  A 
 AèAø< Aè5INVALIDPASSWORD bcactf{w4sm-m4g1c-xRz5} WASMP4S5W0RD  Fname 
compareString
checkPassword  str1str2index addr
```
Here we see the flag `bcactf{w4sm-m4g1c-xRz5}`
