from bs4 import BeautifulSoup
import pandas as pd 
import time
from undetected_chromedriver import Chrome # bypass cloudflare
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.common.by import By
from plyer import notification

# house data
data = [] 

# URL -> www.rumah.com
def openBrowser(page):
    url = f'https://www.rumah.com/properti-dijual/{page+1}?district_code=IDJB01&freetext=Jawa+Barat%2C+Bandung&property_type=B&property_type_code%5B0%5D=BUNG&region_code=IDJB&search=true'
    driver = Chrome() 
    driver.get(url)
    soup = BeautifulSoup(driver.page_source, "html.parser")
    WebDriverWait(driver, 5).until(EC.presence_of_element_located((By.CLASS_NAME, "layout-web")))
    contents = soup.findAll('div', class_="listing-description")
    driver.quit()
    return contents
    
# get data that we want
def getData(contents):
    for item in contents:
        nama_rumah = item.find('a', class_='nav-link').text
        lokasi_rumah = item.find('p', class_='listing-location').text
        harga_rumah = item.find('span', class_='price').text
        luas_rumah = item.find('li', class_='listing-floorarea') 
        jmlh_kamar_mandi = item.find('span', class_='bed')
        jmlh_toilet = item.find('span', class_='bath')
        tipe_properti = item.find('ul', class_='listing-property-type')
        jmlh_km = ""
        jmlh_t = ""
        luas_r = ""
        # these variables are NoneType
        if(jmlh_kamar_mandi): jmlh_km = jmlh_kamar_mandi.text
        if(jmlh_toilet): jmlh_t = jmlh_toilet.text
        if(luas_rumah): luas_r = luas_rumah.text
        if(tipe_properti): tipe_p = tipe_properti.text
        data.append((nama_rumah, lokasi_rumah, jmlh_km, jmlh_t, luas_r, harga_rumah))


first_page = 117
last_page = 132

for page in range(first_page,last_page):
    try:
        contents = openBrowser(page)
        getData(contents)
        print("Success at page: " + str(page))
        if(page == last_page-1):
            # notif_success.play()
            notification.notify(
                title = "Success",
                message = 'message',
                app_icon = None,
                timeout = 20,
            )
    except:
        print("Failed at page: " + str(page))
        notification.notify(
            title = "Failed",
            message = 'message',
            app_icon = None,
            timeout = 20,
        )
        break

df = pd.DataFrame(data, columns=['Judul', 'Lokasi' , "Kamar", "Toilet", 'Luas', 'Harga'])
df.to_csv('C:/Users/gaudh/OneDrive/Documents/proyek/data-scrapper/Rumah-Bandung-01042023-117pages.csv', mode='a', index=False, header=False)
print(df)
Chrome().quit()

print(df)
