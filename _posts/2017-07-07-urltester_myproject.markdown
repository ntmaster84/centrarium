---
layout: post
title:  "[CasperJS] 로그인 테스트 자동화 Tool"
date:   2017-07-07 09:00:10
author: Beom
categories: Project
---

40여개의 계열사 사이트를 매일 로그인 해보고 로그인이 정상적으로 되는지 확인하는 단순 업무를 하고 있는.. 친구를 위해 만들었던 Tool..

url 리스트를 담은 텍스트 파일을 생성하면 Java Application이 url을 읽어 CasperJS를 멀티 스레드로 띄워 접속 테스트 후 결과를 text파일로 떨궈 준다.

![](https://raw.githubusercontent.com/ntmaster84/ntmaster84.github.io/master/_posts/resource/urlTester.png)


CasperJS Script

스크립트 자체가 별로 특별한게 없다.. 간단히 살펴보면

 LoginUrl을 받아 접속하고 셋팅된 폼 값에 ID와 패스워드를 입력한 후 정상 로그인 됐을 때 리다이렉트 되는
 AfterUrl과 비교해서 Sucess / Fail을 반환한다..
 

```

var casper = require('casper').create({
    pageSettings: {
    	method: 'POST',
    	loadImages: false,
        loadPlugins: false,
        userAgent: 'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/44.0.2403.157 Safari/537.36'
    }	
});


var loginUrl = casper.cli.get("LoginUrl");
var afterUrl = casper.cli.get("AfterUrl");

var id = casper.cli.get("Id");
var pass = casper.cli.get("Pass");

var idForm = casper.cli.get("idForm");
var passForm = casper.cli.get("passForm");
var submitButton = casper.cli.get("submitButton");

//Page Open
casper.start().thenOpen(loginUrl, function() {
    console.log(loginUrl);
});


//Form 셋팅
casper.then(function(){
    console.log("Login using username and password");
    
    this.evaluate(function(uid,upass,uidForm, upasswordForm, btn){    	
    	document.getElementById(uidForm).value=uid;
		document.getElementById(upasswordForm).value=upass;
		document.getElementById(btn).click();
    },id, pass, idForm, passForm, submitButton);

});


//리다이렉트 대기
casper.then(function(){
    console.log("Login Waiting..");
	this.wait(3000);
	
	var curUrl = this.getCurrentUrl().toLowerCase();
	
	this.echo(afterUrl);
	this.echo(curUrl);
	
	if( afterUrl.toLowerCase() != curUrl.toLowerCase())
		this.echo("Fail");
	else
		this.echo("Sucess");
	
	this.capture('AfterLogin.png');
});


//런
casper.run();
```