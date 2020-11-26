@def title = "Set SSR on ubuntu18.04"
@def published = "28 November 2018"
@def tags = ["Tutorial", "Ubuntu"]


#### 1. Install a client from [HERE](https://github.com/erguotou520/electron-ssr), It is an amazing ssr GUI!
**...**
#### 2. Open it and setting params.
**...**
#### 3. Setting Firefox browser. 

*One way*       

- `Preferences` -> `General` -> `Network Settings` -> `Manual Proxy configuration`
- `SOCKS Host:127.0.0.1` and `Port:1080`.ps:use yours port if you set another port.
- Clicking `OK`.
- Enjoying!
     

*Another way*       

- Adding this Add-on called [FoxyProxy Standard](https://addons.mozilla.org/en-US/firefox/addon/foxyproxy-standard/).
- Setting this Add-on by clicking `Options`, it will pop a tab. 
- Enter`Edit`button then setting IP address(127.0.0.1) and Port(1080).
- Enjoying!
