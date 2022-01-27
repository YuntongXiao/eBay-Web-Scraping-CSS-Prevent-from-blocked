# eBay-Web-Scraping-CSS-Prevent-from-blocked
This project provides method to get the html file from ebay.com By setting header to imitate the human user, prevent you from being blocked by the website Using regex get the CSS rules of eBay, help identify the sponsored items.

---How to find CSS in web pages

    my_dict={}
    for i in range(1,11):
        filename = "ebay_lg_phone_"+str(i) +".html"

        with open(filename, 'r', encoding='utf-8') as f:
            webpage = BeautifulSoup(f.read(), 'html.parser')

        code=str(webpage)   ## Extract all the web code

        # The rule of css in the web code is like this format: 
        ## <style type="text/css">span.s-m1yuhh {display: inline;}span.s-o2xlx7k { display: none;}</style>
        import re
        ## apply this regex can extract the class we want to locate the item with sponored
        span = re.findall('span\.([^\s\{]+)\s*\{\s*display\:\s*inline',code)
        span = ''.join(span)
        sponsor = webpage.find_all('div', attrs={'class': 's-item__wrapper clearfix'})

        for j in sponsor:
            a= j.find('span', attrs={'class': span})  
            ## If the item has this class, it's sponsored item
            if a is not None :
                name=j.find('h3', attrs={'class': 's-item__title'}).text 
                ### get the item tilte, link
                link=j.find('a', href=True)['href']
                my_dict[name] =link
    my_dict.pop('')
    for key,value in my_dict.items(): 
        print('{key}: {value}'.format(key = key, value = value))
