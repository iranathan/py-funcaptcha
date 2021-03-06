# py-funcaptcha
Python3 module allowing for easy automated interaction with [ArkoseLabs FunCaptcha](https://medium.com/arkoselabs/funcaptcha-security-verification-for-people-of-all-abilities-c2f9a0ba73df).

Tested on: 2020-06-18

# Features
- Does not use Selenium or any other browser automation tool
- Completely evades bot detection
- Supports all FunCaptcha clients/websites
- Supports both SOCKS4/5 and HTTP/S proxies
- Frequent updates

# Installation
```bash
pip3 install -r requirements.txt
```
On Windows machines you'll also have to install the module `pycryptodome`

# Things to note
- `.full_token` is the token you'll submit to the target website upon completing a challenge
- Pillow's *.rotate* method is reversed, therefore you'll have to multiply the degree by -1 to get the image rotated correctly
- Only the following user-agents are supported: `Google Chrome`, `Mozilla Firefox`

# Usage
```python
from py_funcaptcha import FunCaptchaSession
from random import randint


## Create session for Roblox.com's login endpoint
s = FunCaptchaSession(
    public_key="9F35E182-C93C-EBCC-A31D-CF8ED317B996",
    service_url="https://roblox-api.arkoselabs.com",
    page_url="https://www.roblox.com/login",
    predownload_images=True,
    proxy="https://127.0.0.1:3128")
    
## Obtain a new challenge
ch = s.create_challenge()


## Print some challenge details
print("Full Token:", ch.full_token)
print("Degree per rotation:", ch.angle)
print("Number of images:", ch.get_image_count())


## Iterate over challenge images (Pillow)
for image, submit in ch.get_iter():
    ## Generate a random guess
    guess = ch.angle * randint(1, int(360/ch.angle) - 1)
    
    ## Show preview (.rotate method is reversed)
    image.rotate(guess*-1).show()
    
    ## Submit guess
    print("Submitting guess:", guess)
    solved = submit(guess)


## Print final result
print("Solved:", solved)
```

If you have any questions, suggestions or offers, feel free to message me on twitter at [@h0nde](https://twitter.com/h0nde).
