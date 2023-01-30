# TowersSource - A FREE bloxflip version of auto tower. No webhook or BS.
Source code is here.

import tls_client, random

session = tls_client.Session(
    client_identifier="chrome_105"
)

betamount = 5

authtoken = "token"

headers = {
    'authority': 'api.bloxflip.com',
    'accept': 'application/json, text/plain, */*',
    'accept-language': 'pl-PL,pl;q=0.9,en-US;q=0.8,en;q=0.7',
    'content-type': 'application/json',
    'origin': 'https://bloxflip.com',
    'sec-ch-ua': '"Not_A Brand";v="99", "Google Chrome";v="109", "Chromium";v="109"',
    'sec-ch-ua-mobile': '?0',
    'sec-ch-ua-platform': '"Windows"',
    'sec-fetch-dest': 'empty',
    'sec-fetch-mode': 'cors',
    'sec-fetch-site': 'same-site',
    'user-agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/109.0.0.0 Safari/537.36',
    'x-auth-token': authtoken,
    'x-timezone': 'Europe/Warsaw',
}
def bet():
    global betamount
    json_data = {
        'difficulty': 'normal',
        'betAmount': betamount,
    }

    response = session.post('https://api.bloxflip.com/games/towers/create', headers=headers, json=json_data)

    print(response.json())

    json_data = {
        'cashout': False,
        'tile': random.randint(0, 1),
        'towerLevel': 0,
    }

    response = session.post('https://api.bloxflip.com/games/towers/action', headers=headers, json=json_data)
    print(response.json())
    if (response.json()["exploded"]):
        print("LOSS")
        betamount = betamount * 2
        bet()
    else:
        print("WON, CASHOUT")
        betamount = 5
        json_data = {
            'cashout': True,
        }

        response = session.post('https://api.bloxflip.com/games/towers/action', headers=headers, json=json_data)
        print(response.json())
        bet()
bet()
