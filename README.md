# Oauth 2.0 authentication
[![Build Status](https://travis-ci.org/fraxedas/eloqua-oauth.svg)](https://travis-ci.org/fraxedas/eloqua-oauth)
[![NPM](https://nodei.co/npm/eloqua-oauth.png?mini=true)](https://npmjs.org/package/eloqua-oauth)

## Installing the package via npm
```
npm install eloqua-oauth --save
```

## Eloqua configuration
1. Get an eloqua account and create a provider: [Visit AppCloud docs!](http://docs.oracle.com/cloud/latest/marketingcs_gs/OMCAB/index.html#Developers/AppCloud/appcloud.htm%3FTocPath%3DAppCloud%2520Development%2520Framework%7C_____0)
2. Create an application with the following urls:
  2.1 Enable Url: https://[app]/oauth/{appId}/{installId}?callback={CallbackUrl}  
  2.2 Status Url: https://[app]/app/status  
  2.3 Callback Url: https://[app]/callback  
3. Copy the app id and the app secret to your application

>[app] is the domain where you are hosting your application.
>The parameters in {} will get replaced in Eloqua before calling your application.

## The oauth workflow
Eloqua will call the enable url with the installId, the appId and the eloqua callback.  
You'll need to persist those values and redirect the user to the oauth url.  
```JavaScript
    var eloqua = require('eloqua-oauth');
    var uri = eloqua.authorize(appId, 'https://[app]/callback', installId);
    res.redirect(uri);
```

Handle the callback from eloqua with the grant token  
```
HTTP/1.1 302 Found
Location: https://[app]/callback?code=SplxlOBeZQQYbYS6WxSbIA&state=xyz
```
```JavaScript
    eloqua.grant(appId, client_secret, code, 'https://[app]/callback', function (error, body) {
        if (error) {
            //Handle the error
        }else{
			//Finish the installtion in Eloqua by redirecting to the callback in the previous step
            res.redirect(callback);
        }
    });
```
