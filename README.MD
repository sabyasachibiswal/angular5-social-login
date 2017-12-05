Social login api for Angular 5. Includes Facebook and Google login.  

[AOT](https://angular.io/guide/aot-compiler) Compatible.


## Getting started


### Install via npm 

```sh
npm install --save angular5-social-login
```

### Import the module

In `app.module.ts`,

```javascript
...

import {
    SocialLoginModule,
    AuthServiceConfig,
    GoogleLoginProvider,
    FacebookLoginProvider,
} from "angular5-social-login";


// Configs 
export function getAuthServiceConfigs() {
  let config = new AuthServiceConfig(
      [
        {
          id: FacebookLoginProvider.PROVIDER_ID,
	      provider: new FacebookLoginProvider("Your-Facebook-app-id")
        },
        {
          id: GoogleLoginProvider.PROVIDER_ID,
	      provider: new GoogleLoginProvid("Your-Google-Client-Id")
        },
      ];
  );
  return config;
}

@NgModule({
  imports: [
    ...
    SocialLoginModule
  ],
  providers: [
    ...
    {
      provide: AuthServiceConfig,
      useFactory: getAuthServiceConfigs
    }
  ],
  bootstrap: [...]
})

export class AppModule { }

```

### Usage : 

In `signin.component.ts`,

```javascript
import {Component, OnInit} from '@angular/core';
import {
    AuthService,
    FacebookLoginProvider,
    GoogleLoginProvider
} from 'angular5-social-login';

@Component({
  selector: 'app-signin',
  templateUrl: './signin.component.html',
  styleUrls: ['./signin.component.css']
})


export class SigninComponent implements OnInit {

  constructor( private socialAuthService: AuthService ) {}
  
  public socialSignIn(socialPlatform : string) {
    let socialPlatformProvider;
    if(socialPlatform == "facebook"){
      socialPlatformProvider = FacebookLoginProvider.PROVIDER_ID;
    }else if(socialPlatform == "google"){
      socialPlatformProvider = GoogleLoginProvider.PROVIDER_ID;
    }
    
    this.socialAuthService.signIn(socialPlatformProvider).then(
      (userData) => {
        console.log(socialPlatform+" sign in data : " , userData);
        // Now sign-in with userData
        ...
            
      }
    );
  }
  
}
```



In `signin.component.html`,

```html
<h1>
     Sign in
</h1>

<button (click)="socialSignIn('facebook')">Sign in with Facebook</button>
<button (click)="socialSignIn('google')">Signin in with Google</button>              
```



### Facebook App Id : 

You need to create your own app by going to [Facebook Developers](https://developers.facebook.com/) page.
Add `Facebook login` under products and configure `Valid OAuth redirect URIs`.

### Google Client Id : 

Follow this official documentation on how to [
Create a Google API Console project and client ID](https://developers.google.com/identity/sign-in/web/devconsole-project).

