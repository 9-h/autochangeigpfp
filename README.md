# igpfp
Auto change your Instagram profile picture with a photo file with selected photos of your choice.

import requests
import time
import random
import os


try:
    os.mkdir('photo')
except:
    pass

art ="""
  _____ _____                    __ _ _              _      
 |_   _/ ____|                  / _(_) |            (_)     
   | || |  __   _ __  _ __ ___ | |_ _| | ___   _ __  _  ___ 
   | || | |_ | | '_ \| '__/ _ \|  _| | |/ _ \ | '_ \| |/ __|
  _| || |__| | | |_) | | | (_) | | | | |  __/ | |_) | | (__ 
 |_____\_____| | .__/|_|  \___/|_| |_|_|\___| | .__/|_|\___|
               | |                            | |           
               |_|                            |_|           
                                    by: @k9
"""
print(art)

s = requests.session()

user = str(input('username: '))
password = str(input('password: '))
ss = int(input('sleep: '))

url_ig = 'https://www.instagram.com/accounts/login/ajax/'
ig = s.get('https://www.instagram.com').cookies['csrftoken']
user_agent = 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.116 Safari/537.36'

s.headers = {'user-agent': user_agent}
s.headers.update({'referer': 'https://www.instagram.com'})
s.headers.update({'x-csrftoken': ig})

payload = {"username": user, 'enc_password': '#PWD_INSTAGRAM_BROWSER:0:' + str(int(time.time())) + ':' + password}

r = s.post(url_ig, data=payload)

if r.json()["authenticated"]:
    print(f"Login successful @{user}")
else:
    print("Login failed!")
    input('Enter to exit')
    exit()

s.headers.update({'x-csrftoken': r.cookies['csrftoken']})
profile_picture = 'https://www.instagram.com/accounts/web_change_profile_picture/'

print('\n')

while True:
    path = os.getcwd()
    photo = os.listdir('photo')
    r = random.choice(photo)
    x = (r'{}\photo\{}'.format(path, r))

    p_pic = x
    p_pic_s = os.path.getsize(x)
    files = {'profile_pic': open(p_pic, 'rb')}

    s.headers.update({"Content-Length": str(p_pic_s)})

    payload2 = {"Content-Disposition": "form-data", "name": "profile_pic", "filename":"profilepic.jpg",
    "Content-Type": "image/jpeg"}

    try:
        change_profile_picture = s.post(profile_picture, data=payload2, files=files)
        status = change_profile_picture.json()['status']
        print('[*] Done change profile pic', status)
    except:
        print('[-] error change profile pic', f'({r})')
    time.sleep(ss)
