scrolldriver
============


import phan_proxy
import logging
import time
from selenium import webdriver
import logging
import sys
from random  import choice
from selenium import webdriver
from selenium import webdriver
from selenium.webdriver.common.desired_capabilities import DesiredCapabilities
from selenium.webdriver.support.ui import WebDriverWait
from selenium.common.exceptions import WebDriverException
from selenium.webdriver.support.ui import Select

from selenium.webdriver.common.proxy import *
from pyvirtualdisplay import Display


def ajax_complete(driver):
    try:
        return 0 == driver.execute_script("return jQuery.active")

    except WebDriverException:
        pass



def driver_scroller(driver):
    height = 0
    loop = True
    
    while loop is True :
        logging.debug("scrolling...")

        driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")
        try:
            WebDriverWait(driver, 5).until( ajax_complete,  "Timeout waiting for page to load")
        except:
            pass

        heightnow = driver.execute_script("return Math.max(document.documentElement.clientHeight, document.body.scrollHeight, document.documentElement.scrollHeight, document.body.offsetHeight, document.documentElement.offsetHeight );")

        try:
            WebDriverWait(driver, 5).until( ajax_complete,  "Timeout waiting for page to load")
        except:
            pass

        logging.debug(heightnow)

        if heightnow == height:
            loop = False

        else:
            height = heightnow
            loop = True
   

    return driver



def sub_scroller(driver):
    loop = True

    start = 1
    time.sleep(2)
    while (loop is True):
       
        try:
            print "clicking..."
             
            str8 ="html body div.main div div.section div.two-cols-holder div.content div.pagination_box div#searchPaginationDiv.pagination span#viewMoreRsltSpan a#viewMoreLink.view-all span"

            driver.execute_script("window.scrollBy(0,  -800)", "")

            try:
                WebDriverWait(driver, 50).until( ajax_complete,  "Timeout waiting for page to load")
            except:
                pass

            driver.find_element_by_css_selector(str8).click()
            
            try:
                WebDriverWait(driver, 50).until( ajax_complete,  "Timeout waiting for page to load")
            except:
                pass


            driver = driver_scroller(driver)

            try:
                WebDriverWait(driver, 50).until( ajax_complete,  "Timeout waiting for page to load")
            except:
                pass


        except:
            loop = False


        start +=1
    return driver


def supermain(link):

    #display = Display()
    #display.start()
    #f2 = open("/home/user/Desktop/proxy_160514.txt")
    #f2 = open("/home/user/Desktop/proxy_http_auth.txt")
    #proxy_list = f2.read().strip().split("\n")
    #f2.close()

    #ip_port = choice(proxy_list).strip()

    #proxy = ip_port
    #port = 8080
 

    #fp = webdriver.FirefoxProfile()
    #fp.set_preference('network.proxy.ssl_port', int(port))
    #fp.set_preference('network.proxy.ssl', proxy)
    #fp.set_preference('network.proxy.http_port', int(port))
    #fp.set_preference('network.proxy.http', proxy)
    #fp.set_preference('network.proxy.type', 1)
    #driver = webdriver.Firefox(firefox_profile=fp)
    driver = phan_proxy.main(link)
   # driver = webdriver.Firefox()
   # driver.maximize_window()
   # driver.get(link)

    #driver.find_element_by_xpath("/html/body/div[3]/div/ul/li[9]/div/div/ul/li[6]/a").click()

   # try:
    #    driver.implicitly_wait(10)

    #except:
     #   pass

    #try:
     #   driver.set_page_load_timeout(35)
   # except:
    #    pass

    #try:
     #   driver.get(link)
    #except:
    #   pass
  
    #try:
     #   WebDriverWait(driver, 10).until( ajax_complete,  "Timeout waiting for page to load")
    #except:
    #    pass

    

    driver = driver_scroller(driver)

    driver = sub_scroller(driver)
    page = driver.page_source

    driver.delete_all_cookies()
    driver.quit()
    return page
    #display.stop()
    
if __name__ =="__main__":
    link = "http://www.mirraw.com/designers/ragini-sarees/designs/jai-ho-bollywood-royal-blue-chiffon-saree-other-actress-saree"
    supermain(link)
    #f = open("myfile.html", "w+")
    #print >>f, page
    #f.close()
