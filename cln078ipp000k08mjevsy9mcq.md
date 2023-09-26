---
title: "How to Use Chrome's Copy as CURL for Postman API Calls"
seoTitle: "Use Chrome's Copy as CURL in Postman API"
seoDescription: "Optimize API debugging with Chrome's Copy as cURL feature for Postman calls, simplifying request replication and accelerating development process"
datePublished: Tue Sep 26 2023 10:53:14 GMT+0000 (Coordinated Universal Time)
cuid: cln078ipp000k08mjevsy9mcq
slug: how-to-use-chromes-copy-as-curl-for-postman-api-calls
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/NScCnMEYHQ0/upload/733b63ad0eb2c9cc16324e25cce8fd22.jpeg
tags: postman, curl, rest-api

---

During the development process, many times we need to replicate the backend API call for debugging purposes. This is also required to write the automation & performance tests with proper API configuration details to successfully get the response as expected. Manually recreating the request with the correct URI, headers, and cookies can be tedious, and time-consuming too. Fortunately, most modern browsers come with a simple utility bundled along to solve this issue.

Let's take an example API call for this. We will be using the below free fake API site to demonstrate this.

[JSON Placeholder Fake API](https://jsonplaceholder.typicode.com/)

* Open the above link in the **Chrome** browser
    
* Open developer tools either via navigating to **Chrome Options &gt; More Tools &gt; Developer Tools** OR using the shortcut **CTRL + SHIFT + I / CMD + OPTION + I (Mac)**
    
* Click on the *Run Script* button to invoke the below sample API and see the response in the *developer tools* under the *Network* tab
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1695721087573/1740e9fd-e4fe-4bf8-92b7-577e3048c123.png align="center")
    
* Take a look at the request details like - Request URL, headers, cookies, etc.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1695721308408/c61cf704-6780-47ad-a884-b2ce222c0afd.png align="left")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1695721366533/9fc8b19a-d5a6-431a-bdd4-4740835131c3.png align="center")

* Select the request and right-click to see the options menu, then choose **Copy &gt; Copy as cURL**
    

> [Curl](https://curl.se/) (short for "Client URL") is a command-line tool that enables data transfer over various network protocols.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1695721663892/1c262d62-19c3-45fc-a752-db387e249179.png align="center")

> If the chrome is opened in windows, there will be 2 options for cURL - one for BASH and another for CMD

* Now the entire request details are added to the clipboard, something like this
    

```bash
curl 'https://jsonplaceholder.typicode.com/todos/1' \
  -H 'authority: jsonplaceholder.typicode.com' \
  -H 'accept: */*' \
  -H 'accept-language: en-US,en;q=0.9,te;q=0.8' \
  -H 'cookie: _ga=GA1.1.63507224.1695719458; ajs_anonymous_id=c601a09a-3cc6-4b84-a482-f58a353eea86; _ga_E3C3GCQVBN=GS1.1.1695719458.1.1.1695720002.0.0.0' \
  -H 'if-none-match: W/"53-hfEnumeNh6YirfjyjaujcOPPT+s"' \
  -H 'referer: https://jsonplaceholder.typicode.com/' \
  -H 'sec-ch-ua: "Chromium";v="116", "Not)A;Brand";v="24", "Google Chrome";v="116"' \
  -H 'sec-ch-ua-mobile: ?0' \
  -H 'sec-ch-ua-platform: "macOS"' \
  -H 'sec-fetch-dest: empty' \
  -H 'sec-fetch-mode: cors' \
  -H 'sec-fetch-site: same-origin' \
  -H 'user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/116.0.0.0 Safari/537.36' \
  --compressed
```

## Use in terminal

We can simply paste the copied content and hit the request in the terminal. As we see, the correct response is being shown.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1695722127108/d6f08b45-5c15-4aef-afc3-53b6ca0e6ff2.png align="center")

## Use in Postman

[Postman](https://www.postman.com/) has an import option to load the API requests. Let's use the below option to import the request and check the response.

* Open Postman, navigate to **File &gt; Import &gt; Raw text** option paste the text that we copied in the previous step, and click **Continue** and then **Import**
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1695722413354/1264fb6f-6a04-4bdb-914d-23c848926bfb.png align="center")

* Once the request is sent to the server, we see the same response.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1695722797615/05b03cf6-eeac-4410-a991-739daa586fff.png align="center")

> We may ignore the response codes for this example - as the server responds with 304 in the browser due to internal redirection logic whereas it gives 200 in the postman for the same call.

Using the copied cURL command in *Postman*, we can effortlessly edit it according to our needs and begin utilizing the APIs immediately. Other browsers - Firefox, Edge, Safari, etc. also have similar features to get this info from the developer tools making developers' lives easy.

Thank you for reading!