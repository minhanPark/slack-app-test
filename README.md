# 슬랙 봇 테스트

## api 연결

```js
const { App } = require("@slack/bolt");

(async () => {
  const port = 3000;
  // Start your app
  await app.start(process.env.PORT || port);
  console.log(`⚡️ Slack Bolt app is running on port ${port}!!`);
})();
```

bolt를 이용하면 쉽게 연결이 가능하다. 기본적으로 express를 이용했다고 한다.  
api의 엔드포인트는 **http:~/slack/events** 형태로 보내는데, 여기서 Socket Mode를 사용하지 않는다면 그대로 엔드포인트를 입력하면 되고, 소켓모드를 사용하면 appToken을 주면서 socket mode 값을 true로 주면 된다.

## command와 message 차이

기본적으로 두개를 사용했는데, command는 /명령어 형태를 받는다.  
message는 @로봇명 형태를 받아서 사용한다.

## block이란?

슬랙에서 형태를 구성하는 것(가령 html 태그처럼)을 편하게 해주는 것이 블록이다. 블록을 쌓듯이 이용하면 된다. api 페이지에서 블록을 넣어서 테스트할 수 있도록 마련되어있음

## ack

ack는 속도를 측정해서(3초?) 넘어버리면 error를 띄우는 듯 하다.  
기본적으로 모든 요청 전에 ack를 사용하는 패턴을 쓴다.

## command 사용하기

```js
app.command("/익명", async ({ command, ack, say }) => {
  try {
    await ack();
    const { text } = command;
    say(text);
  } catch (error) {
    console.log("err");
    console.error(error);
  }
});
```

콜백함수의 command에 기본적으로 정보들이 들어있다.  
해당 객체에 팀명, 유저명, 이메일 등 다 들어있었고, 보낸 텍스트도 있었기 때문에 여기서 text를 가지고 와서 say에 넣어줬음.

## 메시지 캐치하기

```js
app.message(/hey/, async ({ command, say }) => {
  try {
    say("YAAAAAAAAA! command works");
  } catch (error) {
    console.log("err");
    console.error(error);
  }
});
```

메시지에 "hey"를 주면 hey를 보냈을 때만 반응하지만 정규표현식을 넣으면 메시지에서 정규표현식에 해당하면 콜백함수를 실행시킨다.
