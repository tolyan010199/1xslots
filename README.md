# 1xslots
error when running code in the console help pls

when you enter the code to the browser console, it gives an error help to solve

error code:

Uncaught (in promise)
{errors: 7, result: {…}}errors: 7result: 
{Error: "", ErrorCode: 104555, Id: 0, JackpotSum: null, Success: false, …}__proto__: Object
start @ VM2296:139
async function (async)
start @ VM2296:108
(anonymous) @ VM2296:140

async function killer() {
  let response = await fetch(
    `https://${window.location.hostname}/user/balance`,
    {
      credentials: 'include',
      headers: {
        accept: '*/*',
        'accept-language': 'ru-RU,ru;q=0.9,en-US;q=0.8,en;q=0.7',
        'x-requested-with': 'XMLHttpRequest',
      },
      referrer: `https://${window.location.hostname}/allgamesentrance/ivories/`,
      referrerPolicy: 'no-referrer-when-downgrade',
      body: null,
      method: 'POST',
      mode: 'cors',
    }
  );

  let json = await response.json();
  if (json.errors) {
    throw json;
  }
  return json;
}

async function startGame(bonusId, bet) {
  let response = await fetch(
    `https://${window.location.hostname}/allgamesentrance/func`,
    {
      credentials: 'include',
      headers: {
        accept: 'application/json, text/plain, */*',
        'accept-language': 'ru-RU,ru;q=0.9,en-US;q=0.8,en;q=0.7',
        'content-type': 'application/json;charset=UTF-8',
        'x-requested-with': 'XMLHttpRequest',
      },
      referrer: `https://${window.location.hostname}/allgamesentrance/ivories/`,
      referrerPolicy: 'no-referrer-when-downgrade',
      body: `{"type_action":"fortuneapple","bonus_id":${bonusId},"games_bonuses_code":0,"games_bonuses_id":0,"type_query":"place_bet","sumBet":${bet}}`,
      method: 'POST',
      mode: 'cors',
    }
  );

  let json = await response.json();
  if (json.errors) {
    throw json;
  }
  return json;
}
async function step() {
  let response = await fetch(
    `https://${window.location.hostname}/allgamesentrance/func`,
    {
      credentials: 'include',
      headers: {
        accept: 'application/json, text/plain, */*',
        'accept-language': 'ru-RU,ru;q=0.9,en-US;q=0.8,en;q=0.7',
        'content-type': 'application/json;charset=UTF-8',
        'x-requested-with': 'XMLHttpRequest',
      },
      referrer: `https://${window.location.hostname}/allgamesentrance/ivories/`,
      referrerPolicy: 'no-referrer-when-downgrade',
      body: `{"type_action":"fortuneapple","type_query":"step","control_num": 0,"index": 5 }`,
      method: 'POST',
      mode: 'cors',
    }
  );

  let json = await response.json();
  if (json.errors) {
    throw json;
  }
  return json;
}

async function end() {
  let response = await fetch(
    `https://${window.location.hostname}/allgamesentrance/func`,
    {
      credentials: 'include',
      headers: {
        accept: 'application/json, text/plain, */*',
        'accept-language': 'ru-RU,ru;q=0.9,en-US;q=0.8,en;q=0.7',
        'content-type': 'application/json;charset=UTF-8',
        'x-requested-with': 'XMLHttpRequest',
      },
      referrer: `https://${window.location.hostname}/allgamesentrance/ivories/`,
      referrerPolicy: 'no-referrer-when-downgrade',
      body: `{"type_action":"fortuneapple","type_query":"end","control_num": 2 }`,
      method: 'POST',
      mode: 'cors',
    }
  );

  let json = await response.json();
  if (json.errors) {
    throw json;
  }
  return json;
}

function sleep(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

async function start(bet) {
  const res1 = await killer();
  console.log(res1)
  const bonusesShet = res1.balance.filter(el => el.Bonus === 1);
  console.log(bonusesShet[bonusesShet.length - 1]);
  const { id, money } = bonusesShet[bonusesShet.length - 1];
  const { bonus_finish, bonus_fact } = res1.bonus[res1.bonus.length - 1];
  let needMoney = bonus_finish - bonus_fact;
  let wins = 0;
  let loses = 0;
  let balance = money;
  await sleep(200);
  for (let index = 0; 0 < needMoney; index++) {
    await sleep(200);
    await startGame(id, bet);
    let status = await step();
    needMoney -= bet;
    await sleep(200);
    if (status.ST === 3) {
      loses++;
      balance -= bet;
    } else if (status.ST === 1) {
      await end();
      balance += bet * 0.238;
      wins++;
    }
    console.clear();
    console.log(`Побед: ${wins} Поражений: ${loses}`);
    console.log(`Баланс: ${balance}`);
    console.log(`Текущая ставка: ${bet}`);
    console.log(`Осталось проставить: ${needMoney} рублей`);
  }
}
start(40)
