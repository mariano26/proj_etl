# proj_etl
projeto sdw2023

sdw2023_api_url = 'https://sdw-2023-prd.up.railway.app'

import pandas as pd

df = pd.read_csv('SDW2023.csv')
user_ids = df['UserID'].tolist()
print(user_ids)

import requests
import json

def get_user(id):
  response = requests.get(f'{sdw2023_api_url}/users/{id}')
  return response.json() if response.status_code == 200 else None

users = [user for id in user_ids if (user := get_user(id)) is not None]
print(json.dumps(users, indent=2))

openai_api_key = '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'

for user in users:
  news = "Obtenha sua habilitação e desfrute de mobilidade, segurança e oportunidades profissionais."
  print(news)
  user['news'].append({
      "icon": "https://digitalinnovationone.github.io/santander-dev-week-2023-api/icons/credit.svg",
      "description": news
})

def update_user(user):
  response = requests.put(f"{sdw2023_api_url}/users/{user['id']}", json=user)
  return True if response.status_code == 200 else False

for user in users:
  success = update_user(user)
  print(f"User {user['name']} updated? {success}!")
