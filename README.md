import requests
import json
import pandas as pd

def Rate_US(x):
  # GMOコインのAPIで最新の為替レートを取得
    end_point = 'https://forex-api.coin.z.com/public'
    api_path = f'/v1/ticker'

    response = requests.get(f'{end_point}{api_path}')
    if response.status_code != 200:
     print(f'エラーが発生しました。ステータス：{response.status_code}, メッセージ：{response.text}')
     exit(-1)

    # レスポンス結果を取得
    data = response.json()

    # pandasにデータを格納
    df = pd.DataFrame(data["data"])


    # USDJPYのデータのみ抽出
    usd_jpy_data = df.loc[df['symbol'] == 'USD_JPY']

    y = float(usd_jpy_data["ask"].values[0])


    print(f'現在のUSドルのレート：{y}')

    c = y * x

    return c

string = input("知りたいドルを入力してください：")

ans = Rate_US(float(string))

print(f'現在の{float(string)}ドルは日本円に変換すると{ans}円です！')


