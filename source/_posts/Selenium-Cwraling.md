---
title: Selenium Cwraling
date: 2024-01-29 09:21:41
tags: python
categories:
    - Python
---
# 셀레니움을 활용한 DB 크롤링

- 셀레니움 패키지를 활용하여 크롤링한 데이터를 DB에 저장하기

```python
import time

from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.chrome.options import Options

import pandas as pd

from sqlalchemy import create_engine
import pymysql

import schedule
import chromedriver_autoinstaller

from webdriver_manager.chrome import ChromeDriverManager
from selenium.webdriver.chrome.service import Service

def auto():

    '''
    브라우저 열지 않은 상태로 진행 옵션
    '''

    # chrome_ver = chromedriver_autoinstaller.get_chrome_version().split('.')[0]  # 크롬드라이버 버전 확인
    #
    # opts = Options()
    # opts.add_argument('headless')
    #
    # try:
    #     driver = webdriver.Chrome(f'./{chrome_ver}/chromedriver.exe', options=opts)
    # except:
    #     chromedriver_autoinstaller.install(True)
    #     driver = webdriver.Chrome(f'./{chrome_ver}/chromedriver.exe', options=opts)
    #
    # driver.implicitly_wait(10)

    # path = chromedriver_autoinstaller.install()
    # driver = webdriver.Chrome(path)
    #
    # driver = webdriver.Chrome(executable_path=ChromeDriverManager().install())
    #
    # driver = webdriver.Chrome(ChromeDriverManager().install())

    # Set Chrome options
    chrome_options = webdriver.ChromeOptions()
    # Add any necessary options here

    # Install the ChromeDriver compatible with the detected Chrome version
    driver_path = ChromeDriverManager().install()

    # Initialize Chrome webdriver using the installed ChromeDriver path and options
    driver = webdriver.Chrome(options=chrome_options)


    '''
    ---
    '''

    url = 'url주소'
    driver.get(url)
    time.sleep(1)

    usr = driver.find_element(By.CSS_SELECTOR, 'body > div.loginwrap > div > form > ul > li:nth-child(1) > input[type=text]')
    usr.send_keys('아이디')
    pwd = driver.find_element(By.CSS_SELECTOR, 'body > div.loginwrap > div > form > ul > li.bordert > input[type=password]')
    pwd.send_keys('비번')

    driver.find_element(By.CSS_SELECTOR, 'body > div.loginwrap > div > form > input').click()

    time.sleep(2)

    driver.find_element(by=By.CSS_SELECTOR, \
                        value='#container > div > div.con_bx.filter_box > div > form > table > tbody > \
                        tr:nth-child(1) > td > div.daterange > input[type=text]').click()

    driver.find_element(by=By.CSS_SELECTOR, \
                        value='body > div.daterangepicker.dropdown-menu.ltr.show-calendar.opensright \
                              > div.calendar.left > div.daterangepicker_input > input[type=text]').clear()

    fil = driver.find_element(by=By.CSS_SELECTOR, \
                              value='body > div.daterangepicker.dropdown-menu.ltr.show-calendar.opensright \
                              > div.calendar.left > div.daterangepicker_input > input[type=text]')
    fil.send_keys('2021-01-01')

    driver.find_element(by=By.CSS_SELECTOR, \
                        value='body > div.daterangepicker.dropdown-menu.ltr.show-calendar.opensright\
                         > div.ranges > div > button.applyBtn.btn.btn-sm.btn-success').click()

    driver.find_element(by=By.CSS_SELECTOR, value='#container > div > div.con_bx.filter_box > div > div > a').click()

    time.sleep(30)

    '''
    크롤링
    '''

    tbody = driver.find_element(by=By.CSS_SELECTOR, value="#DataTables_Table_1 > tbody")

    dates = tbody.find_elements(by=By.CSS_SELECTOR, value="td.date.sorting_1")

    revenue = tbody.find_elements(by=By.CSS_SELECTOR, value="td:nth-child(6)")

    data_result=[]

    print(len(dates))

    for i in range(len(dates)):
        temporary_data=(dates[i].text,
                        revenue[i].text)
        data_result.append(temporary_data)

    print(data_result)

    driver.close()

    '''
    전처리
    '''

    columns = ['Dates', 'Revenue']

    df = pd.DataFrame(data_result, columns=columns)

    print(df.head())
    print(df.info())

    df['Dates'] = df['Dates'].str.replace("(", "").str.replace(")", "").str.replace("Mon", "")
    df['Dates'] = df['Dates'].str.replace("(", "").str.replace(")", "").str.replace("Tue", "")
    df['Dates'] = df['Dates'].str.replace("(", "").str.replace(")", "").str.replace("Wed", "")
    df['Dates'] = df['Dates'].str.replace("(", "").str.replace(")", "").str.replace("Thu", "")
    df['Dates'] = df['Dates'].str.replace("(", "").str.replace(")", "").str.replace("Fri", "")
    df['Dates'] = df['Dates'].str.replace("(", "").str.replace(")", "").str.replace("Sat", "")
    df['Dates'] = df['Dates'].str.replace("(", "").str.replace(")", "").str.replace("Sun", "")

    df['Revenue'] = df['Revenue'].str.replace(",", "")

    df['Dates'] = pd.to_datetime(df['Dates'])
    df['Revenue'] = pd.to_numeric(df['Revenue'])

    print(df)
    print(df.info())


    '''
    db연동
    '''

    pymysql.install_as_MySQLdb()

    db_connection_str = 'mysql://아이디:비번@주소:포트/테이블이름'
    db_connection = create_engine(db_connection_str)
    conn = db_connection.connect()

    df.to_sql(name='AD_Revenue', con=db_connection, if_exists='replace', index=False)

'''
스케줄
'''

# schedule.every(1).minute.do(auto)
schedule.every().day.at("11:00").do(auto)

while True:
    schedule.run_pending()
    time.sleep(1)
```

- 해당 예시는 ADX사이트를 예시로 들었으며, 사이트마다 구조가 다르기 때문에<br>
크롤링 부분 코드는 천차만별

![](/image/캡처4.PNG)
