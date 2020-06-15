**FLAG JOKES** 


**Category:** Web
**Concepts:** jwt, jwks

**Problem Statement:**
Want to hear a joke? Want the flag? How about both? Why don't YOU tell me a joke!

**Hints**
None


![image](../images/fj1.png)


The first thing I tried to do was to login as an admin, which was disabled. So I logged in as a random user and took a look at the generated cookies. It had generated this jwt:


```json
[
    {
        "alg": "RS256",
        "jku": "http://localhost:5000/static/jwks.json",
        "kid": "sqcE1a9gj9p08zNMR1MWbLLvuaPyUeJEsClBhy7Q4Jc"
    }

    {
        "username": "plssubscribe"
    }
]
```


It was evident that the website used the set url from the url `http://localhost:5000/static/jwks.json`. I took a look at /static/jwks.json and found the key set. Then I tried to generate a public key from this jwks and forge the jwt by converting the algorithm to HS256 using only the publickey, but no luck.
So I uploaded my own jwks to a url(to which I knew the public and private key pair) and made the jku to be that url. Then I forged my username to be 'admin' through the public and private key pair and got admin access and hence the flag.


My jwks:
```json
{
  "keys": [
    {
    "kty": "RSA",
    "n": "33TqqLR3eeUmDtHS89qF3p4MP7Wfqt2Zjj3lZjLjjCGDvwr9cJNlNDiuKboODgUiT4ZdPWbOiMAfDcDzlOxA04DDnEFGAf-kDQiNSe2ZtqC7bnIc8-KSG_qOGQIVaay4Ucr6ovDkykO5Hxn7OU7sJp9TP9H0JH8zMQA6YzijYH9LsupTerrY3U6zyihVEDXXOv08vBHk50BMFJbE9iwFwnxCsU5-UZUZYw87Uu0n4LPFS9BT8tUIvAfnRXIEWCha3KbFWmdZQZlyrFw0buUEf0YN3_Q0auBkdbDR_ES2PbgKTJdkjc_rEeM0TxvOUf7HuUNOhrtAVEN1D5uuxE1WSw",
    "e": "AQAB",
    "kid": "sqcE1a9gj9p08zNMR1MWbLLvuaPyUeJEsClBhy7Q4Jc",
    "d": "DjU54mYvHpICXHjc5-JiFqiH8NkUgOG8LL4kwt3DeBp9bP0-5hSJH8vmzwJkeGG9L79EWG4b_bfxgYdeNX7cFFagmWPRFrlxbd64VRYFawZHRJt-2cbzMVI6DL8EK4bu5Ux5qTiV44Jw19hoD9nDzCTfPzSTSGrKD3iLPdnREYaIGDVxcjBv3Tx6rrv3Z2lhHHKhEHb0RRjATcjAVKV9NZhMajJ4l9pqJ3A4IQrCBl95ux6Xm1oXP0i6aR78cjchsCpcMXdP3WMsvHgTlsZT0RZLFHrvkiNHlPiil4G2_eHkwvT__CrcbO6SmI_zCtMmypuHJqcr-Xb7GPJoa64WoQ",
    "p": "8K33pX90XX6PZGiv26wZm7tfvqlqWFT03nUMvOAytqdxhO2HysiPn4W58OaJd1tY4372Qpiv6enmUeI4MidCie-s-d0_B6A0xfhU5EeeaDN0xDOOl8yN-kaaVj9b4HDR3c91OAwKpDJQIeJVZtxoijxl-SRx3u7Vs_7meeSpOfE",
    "q": "7a5KnUs1pTo72A-JquJvIz4Eu794Yh3ftTk_Et-83aE_FVc6Nk-EhfnwYSNpVmM6UKdrAoy5gsCvZPxrq-eR9pEwU8M5UOlki03vWY_nqDBpJSIqwPvGHUB16zvggsPQUyQBfnN3N8XlDi12n88ltvWwEhn1LQOwMUALEfka9_s",
    "dp": "DB9nGuHplY_7Xv5a5UCs5YgxkWPtJFfbIZ1Zr-XHCCY09JIWReOGQG226OhjwixKtOK_OqmAKtMKM9OmKviJRHNbDhbTxumN3u7cL8dftjXpSryiEQlPmWyW94MneI2WNIrvh4wruQuDt8EztgOiDFxwcnUgey8iend7WmZnE7E",
    "dq": "O-bSTUQ4N_UuQezgkF3TDrnBraO67leDGwRbfiE_U0ghQvqh5DA0QSPVzlWDZc9KUitvj8vxsR9o1PW9GS0an17GJEYuetLnkShKK3NWOhBBX6d1yP9rVdH6JhgIJEy_g0Suz7TAFiFc8i7JF8u4QJ05C8bZAMhOLotqftQeVOM",
    "qi": "InfGmkb2jNkPGuNiZ-mU0-ZrOgLza_fLL9ErZ35jUPhGFzdGxJNobklvsNoTd-E2GAU41YkJh24bncMLvJVYxHHA5iF7FBWx1SvpEyKVhhnIcuXGD7N5PbNZzEdmr9C6I7cPVkWO-sUV7zfFukexIcANmsd_oBBGKRoYzP5Tti4"
    }
   ]
}
```

Keys:

```txt
-----BEGIN PUBLIC KEY-----
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA33TqqLR3eeUmDtHS89qF
3p4MP7Wfqt2Zjj3lZjLjjCGDvwr9cJNlNDiuKboODgUiT4ZdPWbOiMAfDcDzlOxA
04DDnEFGAf+kDQiNSe2ZtqC7bnIc8+KSG/qOGQIVaay4Ucr6ovDkykO5Hxn7OU7s
Jp9TP9H0JH8zMQA6YzijYH9LsupTerrY3U6zyihVEDXXOv08vBHk50BMFJbE9iwF
wnxCsU5+UZUZYw87Uu0n4LPFS9BT8tUIvAfnRXIEWCha3KbFWmdZQZlyrFw0buUE
f0YN3/Q0auBkdbDR/ES2PbgKTJdkjc/rEeM0TxvOUf7HuUNOhrtAVEN1D5uuxE1W
SwIDAQAB
-----END PUBLIC KEY-----

-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEA33TqqLR3eeUmDtHS89qF3p4MP7Wfqt2Zjj3lZjLjjCGDvwr9
cJNlNDiuKboODgUiT4ZdPWbOiMAfDcDzlOxA04DDnEFGAf+kDQiNSe2ZtqC7bnIc
8+KSG/qOGQIVaay4Ucr6ovDkykO5Hxn7OU7sJp9TP9H0JH8zMQA6YzijYH9LsupT
errY3U6zyihVEDXXOv08vBHk50BMFJbE9iwFwnxCsU5+UZUZYw87Uu0n4LPFS9BT
8tUIvAfnRXIEWCha3KbFWmdZQZlyrFw0buUEf0YN3/Q0auBkdbDR/ES2PbgKTJdk
jc/rEeM0TxvOUf7HuUNOhrtAVEN1D5uuxE1WSwIDAQABAoIBAA41OeJmLx6SAlx4
3OfiYhaoh/DZFIDhvCy+JMLdw3gafWz9PuYUiR/L5s8CZHhhvS+/RFhuG/238YGH
XjV+3BRWoJlj0Ra5cW3euFUWBWsGR0SbftnG8zFSOgy/BCuG7uVMeak4leOCcNfY
aA/Zw8wk3z80k0hqyg94iz3Z0RGGiBg1cXIwb908eq6792dpYRxyoRB29EUYwE3I
wFSlfTWYTGoyeJfaaidwOCEKwgZfebsel5taFz9Iumke/HI3IbAqXDF3T91jLLx4
E5bGU9EWSxR675IjR5T4opeBtv3h5ML0//wq3GzukpiP8wrTJsqbhyanK/l2+xjy
aGuuFqECgYEA8K33pX90XX6PZGiv26wZm7tfvqlqWFT03nUMvOAytqdxhO2HysiP
n4W58OaJd1tY4372Qpiv6enmUeI4MidCie+s+d0/B6A0xfhU5EeeaDN0xDOOl8yN
+kaaVj9b4HDR3c91OAwKpDJQIeJVZtxoijxl+SRx3u7Vs/7meeSpOfECgYEA7a5K
nUs1pTo72A+JquJvIz4Eu794Yh3ftTk/Et+83aE/FVc6Nk+EhfnwYSNpVmM6UKdr
Aoy5gsCvZPxrq+eR9pEwU8M5UOlki03vWY/nqDBpJSIqwPvGHUB16zvggsPQUyQB
fnN3N8XlDi12n88ltvWwEhn1LQOwMUALEfka9/sCgYAMH2ca4emVj/te/lrlQKzl
iDGRY+0kV9shnVmv5ccIJjT0khZF44ZAbbbo6GPCLEq04r86qYAq0woz06Yq+IlE
c1sOFtPG6Y3e7twvx1+2NelKvKIRCU+ZbJb3gyd4jZY0iu+HjCu5C4O3wTO2A6IM
XHBydSB7LyJ6d3taZmcTsQKBgDvm0k1EODf1LkHs4JBd0w65wa2juu5XgxsEW34h
P1NIIUL6oeQwNEEj1c5Vg2XPSlIrb4/L8bEfaNT1vRktGp9exiRGLnrS55EoSitz
VjoQQV+ndcj/a1XR+iYYCCRMv4NErs+0wBYhXPIuyRfLuECdOQvG2QDITi6Lan7U
HlTjAoGAInfGmkb2jNkPGuNiZ+mU0+ZrOgLza/fLL9ErZ35jUPhGFzdGxJNobklv
sNoTd+E2GAU41YkJh24bncMLvJVYxHHA5iF7FBWx1SvpEyKVhhnIcuXGD7N5PbNZ
zEdmr9C6I7cPVkWO+sUV7zfFukexIcANmsd/oBBGKRoYzP5Tti4=
-----END RSA PRIVATE KEY-----
```


**FLAG:** flag{whoops_typo_shoulda_been_flag_jwks}


